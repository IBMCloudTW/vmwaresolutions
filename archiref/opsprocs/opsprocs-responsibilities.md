---

copyright:

  years:  2016, 2021

lastupdated: "2021-03-21"

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}

# Responsibilities for day 2 operations
{: #opsprocs-responsibilities}

The two key principles with respect to day 2 operations are:
* Your vCenter Server instance is not actively monitored by {{site.data.keyword.IBM}}. After deployment, the customer should leverage existing tooling, use a managed service such as that provided by IBM Integrated Managed Infrastructure, third-party managed service or implement a self-managed monitoring solution as described in [Operations management introduction](/docs/vmwaresolutions?topic=vmwaresolutions-opsmgmt-intro) to monitor and manage their environment.
* IBM Support will not enter the VMware management layer under normal operations without a client–written support ticket. A customer must raise a support ticket, as described in [Contacting IBM support](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_support) to invoke support from IBM. Once raised, the ticket must be actively managed by the customer and ensure that they provide the requested information required by IBM in a timely manor. For more information, see [Managing your support cases](/docs/get-support?topic=get-support-managing-support-cases) and [Escalating support cases](/docs/get-support?topic=get-support-escalation).

For more information about IBM and customer responsibilities concerning compliance, see [Customer versus IBM responsibility for vCenter Server](/docs/vmwaresolutions?topic=vmwaresolutions-vc_compl_info#vc_compl_info-responsibility) .

Day 2 responsibilities include the following items:

* Service Support - IBM provides support for {{site.data.keyword.vmwaresolutions_short}} issues that you report. Customers should raise support tickets as directed in [Contacting IBM Support](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_support).
* Service Provisioning - Customers can provision and resize their vCenter Server instances on demand, using the [VMware Solutions console](https://cloud.ibm.com/infrastructure/vmware-solutions/console).
* IBM Management component updates - For instances deployed in (or upgraded to) V2.5 or later, updates and patches for the IBM management components are applied automatically, as needed.
* Incident and Problem Management - Customers are responsible for incident and problem management of their vCenter Server instances after deployment. Customer's should have tools and processes to; detect incidents, record the issues, classify their severity, escalate, and return the failing component to service.
* Capacity Management - Customers are responsible for capacity management of their vCenter Server instances, adding or removing additional capacity to match business demands. For more information, see [Expanding and contracting capacity for vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_addingremovingservers) and [Adding, viewing, and deleting clusters for vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_addingviewingclusters).
* Data Recovery - Customers are responsible for all backup and restoration of the components in the vCenter Server instance, including vCenter Server Appliance, NSX Manager, virtual appliances, virtual machines, and content libraries. Customers can leverage optional services, such as [Veeam](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_considerations) or [IBM Spectrum Protect Plus](/docs/vmwaresolutions?topic=vmwaresolutions-spp_considerations) or implement their own enterprise systems.
* Bare metal server firmware - Customers are responsible for updating [out of date firmware](/docs/bare-metal?topic=bare-metal-bm-faq#what-if-my-bare-metal-server-has-out-of-date-firmware-).
* VMware software updates - Updates to the VMware software, such as hot fixes and service packs, are necessary to maintain the health and availability of your vCenter Server instance, and customers should review these updates and apply them periodically. For m ore information, see [VMware update manager introduction](/docs/vmwaresolutions?topic=vmwaresolutions-vum-intro).
* VMware software upgrades - Customers must upgrade existing vCenter Server instances to continue to benefit from the automation. For more information, see [Upgrading vCenter Server vSphere software from VMware vSphere 6.5 to 6.7](/docs/vmwaresolutions?topic=vmwaresolutions-vc_vsphere_upgrade).
* IBM provides notification of scheduled maintenance at [Planned maintenance](https://cloud.ibm.com/status?selected=maintenance).
* Security - Customers are responsible for ensuring adequate protection of their deployed content including, but not limited to; virtual machine operating system patching, application security fixes, data encryption, access controls, roles and permissions granted to users, security of the virtual network components including distributive or gateway firewall rules. Customers are responsible for the detection, classification, and remediation of all security events within your deployed vCenter Server instances and the associated virtual machines, operating systems, applications, data, or content. In addition, customers are responsible for any compliance or certification program in which you are required to participate. Customers should review [Services](/docs/vmwaresolutions?topic=vmwaresolutions-getting-started#getting-started-add-on-services) for the automated deployment of services to assist them with security responsibilities.
* Encryption - Customers are responsible for determining encryption requirements and implementing these requirements. Customers can leverage encryption options such as [HyTrust DataControl](/docs/vmwaresolutions?topic=vmwaresolutions-htdc_considerations), and vSphere or vSAN encryption with [KMIP for VMware](/docs/vmwaresolutions?topic=vmwaresolutions-kmip_standalone_considerations).
* High availability - Customers are responsible to design and deploy your workload in a way that achieves your high availability requirements including anti-affinity rules and load balancing.
* The [Cloud status page](/docs/get-support?topic=get-support-viewing-cloud-status) is the central place to find unplanned incidents, planned maintenance, announcements, and security bulletin notifications about key events that affect the {{site.data.keyword.cloud_notm}} platform, infrastructure, and major services.

**Next topic**: [Configuration tasks](/docs/vmwaresolutions?topic=vmwaresolutions-opsprocs-configure)

## Related links
{: #opsprocs-responsibilities-links}

* [Managing system notifications](/docs/vmwaresolutions?topic=vmwaresolutions-notifications)
* [Compliance information for vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_compl_info)
* [Using the Support Center](/docs/get-support?topic=get-support-using-avatar)
* [Getting support FAQs](/docs/get-support?topic=get-support-get-supportfaq)
* [Cloud Event Management documentation](https://www.ibm.com/support/knowledgecenter/en/SSURRN/com.ibm.cem.doc/index.html)
