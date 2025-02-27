---

copyright:

  years:  2016, 2020

lastupdated: "2020-12-02"

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Backing up components
{: #solution_backingup}

You’re responsible for the configuration, management, and monitoring of all software components, including the backup and availability of your management infrastructure and workloads.

As part of the solution, you can optionally deploy the {{site.data.keyword.IBM}} Spectrum Protect&trade; Plus or Veeam add-on services. Veeam and IBM Spectrum Protect Plus can help satisfy the requirement to back up your management components.

These add-on services are deployed together with {{site.data.keyword.cloud_notm}} Endurance storage. The services help you back up your workloads and the management components. The [IBM Spectrum Protect Plus architecture overview](https://www.ibm.com/cloud/architecture/architectures/virtualization_backup_spplus){:external} and [Veeam architecture overview](https://www.ibm.com/cloud/architecture/architectures/virtualization_backup_veeam){:external} provide helpful guidance on planning and sizing your deployment.

Different solution components require different strategies for backup. Some components are protected by using image-level backup, and other components are protected by using file-based backup for their configuration and data.

## File server for file-based backup
{: #solution_backingup-fileserver-backup}

Some components, such as VMware vCenter Server with an embedded Platform Services Controller (PSC) and VMware NSX, require their configuration to be backed up to a file server.

To host these backups, deploy a Linux file server into your cluster by using the following steps:

1. Order a private portable subnet from the {{site.data.keyword.cloud_notm}} infrastructure and locate it on the same VLAN as your system components. This is the private VLAN on which the management IP addresses for your hosts reside.
2. Upload an operating system image to your VMware management datastore, such as Ubuntu Server 18.04 LTS, from the {{site.data.keyword.cloud_notm}} private mirror.
3. Deploy this virtual machine (VM) into your cluster on the management port group by using a private portable IP address ordered previously. Ensure that the VM is configured to point to your AD/DNS servers and optionally add the VM to your DNS domain.
4. Create a non-root backup user ID on this server and ensure that all the necessary services are configured and started for file transfers. For example, FTP or SSH.
5. Ensure that this VM is included in your Veeam or IBM Spectrum Protect Plus management backup job.

## vCenter file-based backup
{: #solution_backingup-vcenter}

VMware vCenter Server with embedded PSC provides an [appliance management user interface and API to export the database and configuration to a file server](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.install.doc/GUID-3EAED005-B0A3-40CF-B40D-85AD247D7EA4.html){:external} using various protocols. VMware documents an example of how you can configure this to run periodically as a cron job directly on the vCenter Server Appliance and PSC, which you can adapt for your use.

If you have an external PSC, you must back up both the vCenter Server Appliance and the PSC separately by using this technique. If you have an embedded PSC, then the PSC backup is included in your vCenter backup. Familiarize yourself with and plan for the considerations and limitations that are documented by VMware. Also, plan for a regular rotation and expiration of the file backups on your file server.

VMware requires the backup location to be an empty folder, so plan for your backup rotation or automation to leave the location empty for each subsequent backup job.
{:note}

## NSX file-based backup
{: #solution_backingup-nsx}

Proper backup of all NSX components is crucial to restoring the system to its working state if a failure occurs. The design requires you to configure NSX backup through the NSX Manager backup function. For this purpose, you can configure NSX Manager to regularly perform backups to your file server. Ensure that your file server or its data is backed up correctly, and ensure the rotation of old NSX backups.

## Image-based backup of management virtual machines
{: #solution_backingup-image}

After you deployed your instance and deployed either the IBM Spectrum Protect Plus service or the Veeam backup service, configure a backup job for your management virtual machines. Plan to back up the following VMs with at least 7 days of daily backups:

* If present, VMware SDDC Manager
* If present, Active Directory servers
* File backup server

Plan to allocate enough Veeam or IBM Spectrum Protect Plus licenses to back up these virtual machines, and plan for at least 2 TB of backup storage for the VMs.

## Add-on services
{: #solution_backingup-addons}

If you deploy add-on solution components into your instance, you should also plan for the backup of these components as part of your management backup strategy:

* Zerto Virtual Replication: The Zerto Virtual Manager (ZVM) system is deployed as an {{site.data.keyword.cloud_notm}} virtual server instance (VSI) and its backup is not supported by Veeam or IBM Spectrum Protect Plus. If your disaster recovery strategy requires you to recover the ZVM without performing a site failover, you should use your preferred Windows backup solution to back up and restore the ZVM.
* F5 BIG-IP: F5 recommends [file-based backup of the F5 configuration](https://support.f5.com/csp/article/K13132){:external}, which you can direct to your file server.
* FortiGate Security Appliance or VM: Fortinet recommends [file-based backup of the FortiGate configuration](https://help.fortinet.com/fos50hlp/54/Content/FortiOS/fortigate-best-practices-54/Firmware/Performing_Config_Backup.htm){:external}, which you can direct to your file server.
* HyTrust CloudControl and DataControl: HyTrust supports both image and file-based backup of the HyTrust server appliances. For more information, see the HyTrust administration guides.
* VMware HCX: The HCX appliance management interface allows you to create and download a file-based backup of the HCX Manager configuration similar to the vCenter Server Appliance.

## More considerations
{: #solution_backingup-considerations}

If you choose to deploy your AD/DNS server as an {{site.data.keyword.cloud_notm}} virtual server instance (VSI), you cannot back it up by using Veeam or IBM Spectrum Protect Plus. In this case, use your preferred Windows backup solution for backup and restore operations, or plan to deploy your instance using AD/DNS VMs within your VMware cluster, which can be backed up by Veeam or IBM Spectrum Protect Plus.

Beginning with VMware vCenter 6.5u2, VMware supports the backup of the vCenter Postgres database by using image-based backups, with integrated suspend and resume scripts for the database during the backup window to ensure database integrity. If you upgrade your VMware instance to vCenter 6.5u2 or any newer release, you can choose to use Veeam or IBM Spectrum Protect Plus to back up your vCenter Server and PSC instead of using file-based backups. If you do so, you must use the Veeam or IBM Spectrum Protect Plus quiesce feature to ensure database integrity.

## Restoring from backup
{: #solution_backingup-restore}

There are several special considerations when you restore your management backups:

* For vCenter and PSC, VMware provides an installer that can deploy a new virtual appliance and restore the configuration from backup.
* When you restore an appliance from backup, the installer detects the type of appliance (vCenter Server with embedded PSC) based on the backup information you provide.
* Because you deploy directly to one of your hosts, you might not be able to deploy to a distributed switch or port group. You might need to create a temporary standard switch and port group for deploying the recovered appliances, and migrate one of your vmnics temporarily to this switch to provide network connectivity for your VMs. After deployment, you can migrate the VMs to the distributed port group and return the vmnic to the dvSwitch.
* For NSX, you might need to redeploy your NSX Manager and controllers before you restore the configuration from backup.
* Ensure that you familiarize yourself with the VMware considerations and limitations for vCenter backup and restore.

## Summary
{: #solution_backingup-summary}

With proper planning, you can ensure that your VMware instance can suffer the loss of its management components and recover successfully. Ensure to regularly monitor the success of your backup jobs and availability of your backup data and ensure to test your backup and restore plan regularly, for both your management infrastructure and your workloads.

**Next topic:** [Post-deployment considerations for your VMware instance](/docs/vmwaresolutions?topic=vmwaresolutions-solution_considerations)

## Related links
{: #solution_backingup-related}

* [Overview of {{site.data.keyword.vmwaresolutions_short}}](/docs/vmwaresolutions?topic=vmwaresolutions-solution_overview)
* [Design overview](/docs/vmwaresolutions?topic=vmwaresolutions-design_overview)
* [Scaling capacity](/docs/vmwaresolutions?topic=vmwaresolutions-solution_scaling)
