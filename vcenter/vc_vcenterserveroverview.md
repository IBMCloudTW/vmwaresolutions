---

copyright:

  years:  2016, 2021

lastupdated: "2021-03-18"

keywords: vCenter Server, vCenter Server architecture, tech specs vCenter Server

subcollection: vmwaresolutions

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:important: .important}

# vCenter Server overview
{: #vc_vcenterserveroverview}

VMware vCenter Server® is a hosted private cloud that delivers the VMware vSphere® stack as a service. The VMware® environment is built on top of a minimum of three {{site.data.keyword.cloud}} bare metal servers and it offers shared network-attached storage and dedicated software-defined storage options. It also includes the automatic deployment and configuration of an easy-to-manage logical edge firewall, which VMware NSX® powers.
{: shortdesc}

In many cases, the entire environment can be provisioned in less than a day and the bare metal infrastructure can rapidly and elastically scale the compute capacity up and down as needed.

After initial instance deployment, you can increase shared storage by ordering more Network File System (NFS) file shares from the {{site.data.keyword.slportal}} and by manually attaching them to all VMware ESXi™ servers in a cluster. You can also take advantage of VMware vSAN™ as a storage option. To increase the vSAN-based storage capacity of a vSAN cluster, you can add more ESXi servers post-deployment.

For dedicated storage, see [NetApp® ONTAP® Select](/docs/vmwaresolutions?topic=vmwaresolutions-netapp).

For vCenter Server with NSX-V instances, if you purchased IBM-provided VMware licensing, you can upgrade the VMware NSX Base edition to Advanced or to Enterprise edition, and you can purchase more VMware components, such as VMware vRealize® Operations™. You can also add IBM-Managed Services if you want to offload the day-to-day operations and maintenance of the virtualization, guest OS, or application layers. The {{site.data.keyword.cloud_notm}} Professional Services team is available to help you accelerate your journey to the cloud with migration, implementation, planning, and onboarding services.

For vCenter Server with NSX-T instances, applying license updates is not supported. Also, not all add-on services are supported for NSX-T instances.
{:important}

## vCenter Server with NSX-V architecture
{: #vc_vcenterserveroverview-archi}

The following graphic depicts the high-level architecture and components of a three-node vCenter Server with NSX-V deployment.

![vCenter Server with NSX-V architecture](../images/vc_architecture.svg "vCenter Server with NSX-V architecture"){: caption="Figure 1. vCenter Server with NSX-V high-level architecture for a three-node cluster" caption-side="bottom"}

## vCenter Server with NSX-T architecture
{: #vc_vcenterserveroverview-nsx-t-archi}

The following graphic depicts the high-level architecture and components of a three-node vCenter Server with NSX-T deployment.

![vCenter Server with NSX-T architecture](../images/vc_nsx-t_architecture.svg "vCenter Server with NSX-T architecture"){: caption="Figure 2. vCenter Server with NSX-T high-level architecture for a three-node cluster" caption-side="bottom"}

### Physical infrastructure
{: #vc_vcenterserveroverview-physical-infras}

This layer provides the physical infrastructure (compute, storage, and network resources) to be used by the virtual infrastructure.

### Virtualization infrastructure (Compute and Network)
{: #vc_vcenterserveroverview-virtualization-infras}

This layer virtualizes the physical infrastructure through different VMware products:
* VMware vSphere virtualizes the physical compute resources.
* VMware NSX is the network virtualization platform that provides logical networking components and virtual networks.

### Virtualization management
{: #vc_vcenterserveroverview-virtualization-mgmt}

This layer consists of the following components:
* vCenter Server Appliance (vCSA) with embedded Platform Services Controller (PSC)
* For NSX-V - one NSX Manager and three VMware NSX Controller™ nodes (total of four nodes)
* For NSX-T - three NSX Manager or Controller nodes (total of three nodes)
* VMware NSX Edge™ Services Gateways (ESGs) - two for NSX-V and four for NSX-T (two in the management cluster and two in the workload cluster)
* IBM CloudDriver virtual server instance (VSI). The CloudDriver VSI is deployed on demand as needed for certain operations such as adding hosts to the environment.

The base offering is deployed with a vCenter Server appliance that is sized to support an environment with up to 400 hosts and up to 4,000 VMs. The same vSphere API-compatible tools and scripts can be used to manage the IBM-hosted VMware environment.

In total, the base offering has the following requirements, which are reserved for the virtualization management layer.
* For NSX-V, 38 vCPU and 67 GB vRAM
* For NSX-T, 42 vCPU and 128 GB vRAM

The remaining host capacity for your VMs depends on several factors, such as oversubscription rate, virtual machine (VM) sizing, and workload performance requirements.

For more information about the architecture, see [Overview of {{site.data.keyword.vmwaresolutions_short}}](/docs/vmwaresolutions?topic=vmwaresolutions-solution_overview).

## Technical specifications for vCenter Server instances
{: #vc_vcenterserveroverview-specs}

The availability and pricing of standardized hardware configurations might vary based on the {{site.data.keyword.cloud_notm}} data center that is selected for deployment.
{:note}

The following components are included in your vCenter Server instance.

### Bare metal server
{: #vc_vcenterserveroverview-bare-metal}

* For NSX-V, you can order two or more bare metal servers.
* For NSX-T, you can order two or more bare metal servers in the consolidated or management cluster and in the workload cluster.

Skylake servers are not supported for vSphere Enterprise Plus 7.0 instances.
{:note}

The following configurations are available:
* **Skylake** - 2-CPU Intel Skylake generation servers (Intel Xeon 4100/5100/6100 series) with your selected CPU model and RAM size.
* **Cascade Lake** - 4-CPU Intel Cascade Lake generation server (Quad Intel Xeon Gold 6248 and Quad Intel Xeon Platinum 8260) and 2-CPU Intel Cascade Lake generation servers (Intel Xeon 4200/5200/6200/8200 series) with your selected CPU model and RAM size.
* **SAP-certified** - Intel Skylake generation servers (Intel Xeon 6140 series) and Intel Cascade Lake generation servers (Intel Xeon 5218, 6248, and 8280M series) with your selected CPU model.

If you plan to use vSAN storage, the configuration requires a minimum of four bare metal servers.
{:note}

### Networking
{: #vc_vcenterserveroverview-networking}

The following networking components are ordered:
*  10 Gbps dual public and private network uplinks.
*  Three VLANs (Virtual LANs) - one public and two private.
*  (NSX-V only) One VXLAN (Virtual eXtensible LAN) with DLR (Distributed Logical Router) for potential east-west communication between local workloads that are connected to layer 2 (L2) networks. The VXLAN is deployed as a sample routing topology, which you can modify, build on it, or remove it. You can also add security zones by attaching extra VXLANs to new logical interfaces on the DLR.
* (NSX-T only) One overlay network with a T1 and T0 router for potential east-west communication between local workloads that are connected to layer 2 (L2) networks. This is deployed as a sample routing topology, which you can modify, build on, or remove.
*  VMware NSX Edge Services Gateways (two for NSX-V and four for NSX-T):
  * One secure management services VMware NSX Edge Services Gateway (ESG) for outbound HTTPS management traffic, which is deployed by IBM as part of the management networking typology. This ESG is used by the IBM management virtual machines to communicate with specific external IBM management components that are related to automation. For more information, see [Configuring your network to use the customer-managed ESG](/docs/vmwaresolutions?topic=vmwaresolutions-vc_esg_config).

    This ESG is named **mgmt-nsx-edge**, it's not accessible to you and you can't use it. If you modify it, you might not be able to manage the vCenter Server instance from the {{site.data.keyword.vmwaresolutions_short}} console. In addition, by using a firewall or disabling the ESG communications to the external IBM management components might cause {{site.data.keyword.vmwaresolutions_short}} to become unusable.
    {:important}
  * Secure customer-managed ESG for outbound and inbound HTTPS workload traffic. The ESG is deployed by IBM as a template that can be modified by you to provide VPN access or public access. For NSX-V, one ESG is deployed. For NSX-T, two ESGs are deployed on the datastore with the highest IOPS.

  For more information, see [Does the customer-managed NSX Edge pose a security risk?](/docs/vmwaresolutions?topic=vmwaresolutions-faq-vmwaresolutions#faq-customer-nsx)

### Virtual Server Instances
{: #vc_vcenterserveroverview-vsi}

The following virtual server instances (VSIs) are ordered:
* A VSI for IBM CloudBuilder, which is shut down after the instance deployment is completed.
* Choose to deploy a single Microsoft® Windows® Server VSI for Microsoft Active Directory™ (AD) or two high availability Microsoft Windows VMs in the management cluster to help enhance security and robustness.

### Storage
{: #vc_vcenterserveroverview-storage}

During initial deployment, you can choose between vSAN and NFS storage options.

After deployment, you can add NFS storage shares to an existing NFS or vSAN cluster. For more information, see [Adding NFS storage to vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_addingremovingservers#section-adding-nfs-storage-to-vcenter-server-instances).
{:note}

#### vSAN storage
{: #vc_vcenterserveroverview-vsan-storage}

The vSAN option offers customized configurations, with various options for disk type, size, and quantity:
* Disk quantity - 2, 4, 6, 8, or 10
* Storage disk - 960 GB SSD SED, 1.9 TB SSD SED, or 3.8 TB SSD SED

  In addition, two cache disks of 960 GB are also ordered per host.

  3.8 TB SSD (Solid State Disk) drives are supported when they are made generally available in a data center.
  {:note}
* High Performance with Intel Optane - this option provides two extra capacity disk bays. This option depends on the CPU model.

#### NFS storage
{: #vc_vcenterserveroverview-nfs-storage}

The NFS option offers customized shared file-level storage for workloads with various options for size and performance:
* Size - 20 GB to 24 TB
* Performance - 0.25, 2, 4, or 10 IOPS/GB. The 10 IOPS/GB performance level is limited to a maximum capacity of 4 TB per file share.
* Individual configuration of file shares

(NSX-V only) If you choose the NFS option, one 2 TB and 4 IOPS/GB file share for management components are ordered.
{:note}

#### Local disk storage (NSX-V only)
{: #vc_vcenterserveroverview-local-disk-storage}

The local disks option, available to the **SAP-certified** Quad Intel Xeon E7-8890 v4 processor bare metal configuration only, offers customized configurations with various options for disk count and disk type.

### Licenses (IBM-provided or BYOL) and fees
{: #vc_vcenterserveroverview-license-and-fee}

* VMware vSphere Enterprise Plus 7.0 (NSX-T only) and 6.7  
* VMware vCenter Server 6.7 or 6.5
* VMware NSX Service Providers Edition (Base, Advanced, or Enterprise) 6.4.
* (For vSAN clusters) VMware vSAN Advanced or Enterprise 6.6
* Support and Services fee (one license per node)

## Technical specifications for vCenter Server expansion nodes
{: #vc_vcenterserveroverview-expansion-node-specs}

Each vCenter Server expansion node deploys and incurs charges for the following components in your {{site.data.keyword.cloud_notm}} account.

### Hardware for expansion nodes
{: #vc_vcenterserveroverview-expansion-node-hardware}

One bare metal server with the configuration presented in [Technical specifications for vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_vcenterserveroverview#vc_vcenterserveroverview-specs).

### Licenses and fees for expansion nodes
{: #vc_vcenterserveroverview-expansion-node-license-and-fee}

* One VMware vSphere Enterprise Plus 7.0 (NSX-T only) and 6.7u3
* One VMware NSX Service Providers Edition (Base, Advanced, or Enterprise) 6.4
* One Support and Services fee
* (For vSAN clusters) VMware vSAN Advanced or Enterprise 6.6

You must manage the {{site.data.keyword.vmwaresolutions_short}} components that are created in your {{site.data.keyword.cloud_notm}} account only from the {{site.data.keyword.vmwaresolutions_short}} console, not the {{site.data.keyword.slportal}}, or any other means outside of the console. If you change these components outside of the {{site.data.keyword.vmwaresolutions_short}} console, the changes are not synchronized with the console.
Managing any {{site.data.keyword.vmwaresolutions_short}} components, which were installed into your {{site.data.keyword.cloud_notm}} account when you ordered the instance, from outside the {{site.data.keyword.vmwaresolutions_short}} console can make your environment unstable. These management activities include:
*  Adding, modifying, returning, or removing components
*  Expanding or contracting instance capacity through adding or removing ESXi servers
*  Powering off components
*  Restarting services
   Exceptions to these activities include managing the shared storage file shares from the {{site.data.keyword.slportal}}. Such activities include - ordering, deleting (which might impact data stores if mounted), authorizing, and mounting shared storage file shares.
   {:important}

## Support and Services fee
{: #vc_vcenterserveroverview-support-services-fee}

VMware vCenter Server instances include a Support and Services fee that is charged per {{site.data.keyword.cloud_notm}} bare metal server. This fee covers support from the {{site.data.keyword.vmwaresolutions_short}} DevOps and Level 2 Support teams for any issues that pertain to:
* Automation in the platform
* VMware products included in the solution

## Related links
{: #vc_vcenterserveroverview-related}

* [vCenter Server Software Bill of Materials](/docs/vmwaresolutions?topic=vmwaresolutions-vc_bom)
* [Planning vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_planning)
* [Ordering vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_orderinginstance)
* [Attached storage for vCenter Server](/docs/vmwaresolutions?topic=vmwaresolutions-storage-benefits#storage-benefits)
* [Expanding File Share capacity](/docs/FileStorage?topic=FileStorage-expandCapacity#expandCapacity)
