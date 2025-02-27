---

copyright:

  years:  2019, 2021

lastupdated: "2021-04-01"

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# OpenShift Bastion node setup
{: #openshift-runbook-runbook-bastion-intro}

To enable the deployment, a virtual machine has been provisioned to run the OpenShift  installation steps and host an HTTP Server. This virtual machine is know as the bastion node. The bastion node is connected to the OpenShift logical switch and the ESG firewall and NAT rules have been configured to allow SSH access from the jump-server or remote device.

The bastion node runs Red Hat Enterprise Linux, and it is used to host the scripts, files, and tools to provision the bootstrap, control-plane, and compute nodes. After the deployment, it's recommended to keep the bastion node as an administrative node for the cluster.

The bastion node setup consists of the following steps:

1. Provision a Red Hat virtual machine.
2. Register the Red Hat virtual machine.
3. Install NGINX (HTTP Server).
4. Generate an SSH private key and add it to the agent.

## Provisioning a Red Hat virtual machine
{: #openshift-runbook-runbook-bastion-prov-red-hat}

Provision a Red Hat virtual machine based on the following specifications. This can be done by using the vCenter Server user interface or by using the PowerCLI script that is documented later in this document. Record you NAT address, which is configured in the NSX ESG.

| Virtual machine | IP address | Gateway | Disk (GB) | Memory (GB) | vCPU | NAT address |
| --- | --- | --- | --- | --- | --- | --- |
| bastion | 192.168.133.8 | 192.168.133.1 | 50 | 2 | 1 | 10.208.59.197 |
{: caption="Table 1. Red Hat VM - provision" caption-side="top"}

Use the following table to record your deployment details:

| Parameter | Example | Your deployment |
| --- | --- | --- |
| vCenter Server IP address | | |
| vCenter Server user | | |
| vCenter Server password | | |
| Logical Switch | `OpenShift-LS` | |
| vCenter Server instance data store | `vsanDatastore` | |
| VM name | `bastion` | | |
| ISO file name | `rhel-server-7.6-x86_64-dvd.iso` | |
| IP address | 192.168.133.8 | |
| Netmask |255.255.255.0  | |
| Default gateway | 192.168.133.1 | |
{: caption="Table 2. Red Hat VM deployment" caption-side="top"}

Before you begin, create the VM by using the vCenter CLI or the following PowerCLI script.

```psh
# Connect to vCenter
connect-VIserver –server <IP_Address> -User <UserName> -Password '<Password>'

# Create VM
$ls = get-nsxtransportzone | get-nsxlogicalswitch OpenShift-LS | Get-NsxBackingPortGroup | Select-Object Name
$ds = get-datastore -Name vsanDatastore
$vm = New-VM -Name bastion -Datastore $ds -DiskGB 50 -DiskStorageFormat Thin -MemoryGB 2 -NumCpu 1 -Notes "OpenShift Bastion node" -NetworkName $ls.name -GuestId rhel7_64Guest

# Connect a CD Drive loaded with the RHEL ISO
New-CDDrive -VM $vm -IsoPath "[vsanDatastore] ISO\rhel-server-7.6-x86_64-dvd.iso" -StartConnected

#Start the VM
Start-VM -VM $vm

# Disconnect
Disconnect-NsxServer
```

After the VM has started, connect to the VM by using the web console or remote console and complete the following installation steps, by using the [Graphical install](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/chap-getting-started#sect-graphical-installation){:external} if needed:
* Select the required language.
* Set the date and time.
* Configure the network and hostname.
* Select the installation destination.
* Set the root password.
* Create a user.

## Registering the Red Hat virtual machine
{: #openshift-runbook-runbook-bastion-register}

For this step, you require your Red Hat subscription details:
* Username
* Password
* Subscription Pool

After the bastion node has been deployed, you are required to register and subscribe it with the Red Hat public repositories. This is achieved by connecting to the bastion node by using SSH, from the jump-host or remote device, and getting root privileges with `su` and running the following commands after replacing the Username, Password, and Pool with your variables.

```bash
export rhel_subscription_username=<email address>
export rhel_subscription_password=<password>
sudo subscription-manager register --username=${rhel_subscription_username} --password=${rhel_subscription_password} --force
subscription-manager refresh
subscription-manager attach --pool=<pool>
subscription-manager repos --disable="*"
subscription-manager repos --enable  rhel-7-server-rpms
subscription-manager repos --enable  rhel-7-server-extras-rpms
subscription-manager repos --enable  rhel-server-rhscl-7-rpms
```

## Installing NGINX (HTTP Server)
{: #openshift-runbook-runbook-bastion-http}

The deployment of the OpenShift nodes uses Ignition, and this process requires an HTTP Server to be available to download the required configuration. This deployment uses an NGINX instance running on the bastion node. To install NGNIX, complete the following steps after you are connected to the bastion node and have root privileges:

1. Use a text editor such as vi to create the following file vi /etc/yum.repos.d/nginx.repo`.
2. Type `i` to insert and paste the following information into the file:

  ```bash
  [nginx]
  name=nginx repo
  baseurl=http://nginx.org/packages/mainline/rhel/7/$basearch/
  gpgcheck=0
  enabled=1
  ```

3. Press Esc to get back to command mode and then type `:wq` to save the file and exit vi.
4. Use the `yum` command to install the NGINX package.

  ```bash
  yum update
  yum install -y nginx
  ```

5. Create the default configuration file `vi /etc/nginx/conf.d/default.conf`.
6. Type `i` to insert and paste the following information into the file:

  ```json
  server {
      listen       80;
      server_name  localhost;

      #charset koi8-r;
      #access_log  /var/log/nginx/host.access.log  main;

      location / {
          root   /usr/share/nginx/html;
          index  index.html index.htm;
      }
  }
  ```

7. Press Esc to get back to command mode and then type `:wq` to save the file and exit vi.
8. Run the following commands to start NGINX.

  ```bash
  systemctl enable nginx
  systemctl start nginx
  ```

9. The Linux firewall needs to configured to enable HTTP by using the following firewall-cmd commands:

  ```bash
  firewall-cmd --permanent --zone=public --add-service=http
  firewall-cmd --reload
  ```

## Generating an SSH private key and add it to the agent
{: #openshift-runbook-runbook-bastion-sshkey}

For the OpenShift container platform clusters on which you want to perform installation debugging or disaster recovery, you must provide an SSH key that your ssh-agent process uses to the installer.

You can use this key to SSH into the nodes as the user core. When you deploy the cluster, the key is added to the core user’s `~/.ssh/authorized_keys` list.

You must use a local key.

### Creating the SSH key
{: #openshift-runbook-runbook-sshkey-create}

1. In the SSH session on the bastion node, run the following command, which generates a public/private rsa key pair in the directory '/root/.ssh'.

  ```bash
  ssh-keygen -f ~/.ssh/id_rsa -t rsa -b 4096 -N ''
  ```

The private key is: /root/.ssh/id_rsa. The public key is: /root/.ssh/id_rsa.pub.

2. Start the ssh-agent process as a background task:

  ```bash
  eval "$(ssh-agent -s)"
  ```

3. Add your SSH private key to the ssh-agent:

  ```bash
  $ ssh-add /root/.ssh/id_rsa
  ```

## Downloading the installation tools
{: #openshift-runbook-runbook-bastion-install-red-hat}

For more information about installing Red Hat OpenShift 4.6, see [Installing a cluster on vSphere](https://docs.openshift.com/container-platform/4.6/installing/installing_vsphere/installing-vsphere.html#installation-vsphere-infrastructure_installing-vsphere){:external}.

For more information about how to access the OpenShift user provider infrastructure, see [Internet and Telemetry access for OpenShift Container Platform](https://docs.openshift.com/container-platform/4.6/installing/installing_vsphere/installing-vsphere.html#cluster-entitlements_installing-vsphere){:external}.

Before you install the OpenShift Container Platform, you need to download a number of files onto the bastion node and then extract them. The following actions are completed:
* Download `unzip` to extract the downloaded files.
* Create an installation directory and make it the working directory.
* Download the OpenShift installation and client tools.
* Extract the downloaded bundles.
* Move commands to `/usr/local/bin` for ease of use.
* Install Git to download the OpenShift installer.
* Clone the installer repository to the bastion node.
* Download and extract terraform to the `/usr/local/bin` directory for ease of use.

These commands are used in the SSH session to the bastion node that has root privileges:

```bash
# Download unzip
yum install -y wget unzip

# Create an installation directory and make it the working directory
mkdir -p /opt/ocpinstall
cd /opt/ocpinstall

# Download the OpenShift installer and client tools
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest-4.4/openshift-client-linux.tar.gz
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest-4.4/openshift-install-linux.tar.gz

# Extract the downloaded bundles
tar -xvf openshift-client-linux.tar.gz
tar -xvf openshift-install-linux.tar.gz

# Move commands to /usr/local/bin for ease of use
mv kubectl oc openshift-install /usr/local/bin
mv openshift-install /usr/local/bin

# Install git and clone the OpenShift installer
yum install -y git
git clone -b release-4.4 https://github.com/openshift/installer

# Download and extract terraform
wget https://releases.hashicorp.com/terraform/0.11.13/terraform_0.11.13_linux_amd64.zip
unzip terraform_0.11.13_linux_amd64.zip
mv terraform /usr/local/bin
```

The Bastion node is now ready for the steps to install OpenShift 4.6, which are described in [Red Hat OpenShift 4.6 user provider infrastructure installation](/docs/vmwaresolutions?topic=vmwaresolutions-openshift-runbook-runbook-install-intro).

**Next topic:** [Red Hat OpenShift 4.6 user provider infrastructure installation](/docs/vmwaresolutions?topic=vmwaresolutions-openshift-runbook-runbook-install-intro)

## Related links
{: #vcs-openshift-runbook-bastion-related}

* [Installing a cluster on vSphere](https://docs.openshift.com/container-platform/4.6/installing/installing_vsphere/installing-vsphere.html){:external}
* [Using the vi text editor](http://etutorials.org/Linux+systems/red+hat+linux+bible+fedora+enterprise+edition/Part+II+Using+Red+Hat+Linux/Chapter+4+Using+Linux+Commands/Using+the+vi+Text+Editor/){:external}
