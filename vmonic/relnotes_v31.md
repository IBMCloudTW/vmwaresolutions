---

copyright:

  years:  2016, 2019

lastupdated: "2019-07-16"

keywords: release notes, what's new, version 3.1

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Release notes for V3.1
{: #relnotes_v31}

This release includes new features, component updates, usability enhancements, and bug fixes.

## Single-node Trial for Data Protection and Disaster Recovery instances
{: #relnotes_v31-dr-bundle}

The Single-node Trial for Data Protection and Disaster Recovery is a quick way to test drive {{site.data.keyword.cloud_notm}} to migrate and recover VMware workloads into the {{site.data.keyword.cloud_notm}}. This trial is a version of VMware vCenter Server, a single-tenant VMware platform that can be managed using the same familiar tools as on-premises environments. This trial includes the Veeam on {{site.data.keyword.cloud_notm}}, VMware HCX on {{site.data.keyword.cloud_notm}}, and Zerto on {{site.data.keyword.cloud_notm}} services.

## IBM Cloud Expert Services
{: #relnotes_v31-expert-services}

You can now engage IBM Expert Services to work together with your internal team to deploy, migrate, and maintain your own {{site.data.keyword.cloud_notm}} solution from planning to modernization, or any stage in between.

You can add {{site.data.keyword.cloud_notm}} Expert Services to your instance order at any time by requesting a consultation from the **Getting Started** page.

## Integration with the IBM Cloud Cost Estimator

You can now estimate the cost of your {{site.data.keyword.vmwaresolutions_short}} resources in the unified estimate tool provided by the {{site.data.keyword.cloud_notm}} framework. With this function, you can save a snapshot of resources across the {{site.data.keyword.cloud_notm}} Catalog and store them under a single PDF file that you can download to refer to at a later time. The cost estimator is not a contract or a firm quote, but only an estimate of your total cost.

## Updates for VMware vCenter Server instances
{: #relnotes_v31-vcs}

This release applies the following upgrades and improvements for newly deployed instances, clusters, and hosts:

* vSphere ESXi 6.5 Update 2 (build 6.5.0-13635690)
* vCenter Server Appliance 6.5 Update 2g (build 6.5.0-13638625)

### Alternative ESXi server configuration for clusters
{: #relnotes_v31-esxi-config}

You can now add new ESXi servers to an existing cluster by either selecting an existing configuration or an alternative configuration than the existing hosts in the cluster. When you order new servers from the **Add Server** window, you can instantly select an existing configuration in the cluster or choose a new 	{{site.data.keyword.cloud_notm}} bare metal server configuration. This applies to all vCenter Server, vCenter Server with Hybridity, and vCenter Server with NSX-T instances.

To avoid performance or stability issues, it is recommended that clusters use the same or similar configuration with regards to CPU, RAM, and storage. This functionality is useful for hardware updates within the same cluster.

### Updates for policy configurations
{: #relnotes_v31-policy-config}

The generated vCenter password for vCenter Server primary instances is now 15 characters in length and has the following password and lockout policy configurations:

* The minimum length for the vCenter password policy is now 15 characters.
* The vCenter lockout policy now allows for a maximum of 3 failed login attempts.
* The vCenter lockout policy now allows for a 900 second time interval between failures.

The generated NSX Manager password for vCenter Server primary instances is now 15 characters in length. Previously, the generated password was eight characters in length.

## Updates for add-on services
{: #relnotes_v31-services}

### Caveonix RiskForesight on IBM Cloud
{: #relnotes_v31-services-caveonix}

The current release installs Caveonix RiskForesight 2.2.1 on all newly deployed instances.

### Automatic renewal for HyTrust licenses
{: #relnotes_v31-services-ht}

{{site.data.keyword.vmwaresolutions_short}} now provides automatic renewal support for HyTrust licenses that have the Call Home feature enabled. This support is provided starting with:

* HyTrust CloudControl 5.5.1 and later
* HyTrust DataControl 4.3.2 and later
* HyTrust KeyControl 4.3.2 and later

You must complete the procedure to enable internet access for your HyTrust virtual machines (VMs) to ensure the automatic renewal of your license. If you don't complete this procedure, your license continues to expire annually.
{:important}

### Veeam on IBM Cloud
{: #relnotes_v31-services-veeam}

In the current release, Veeam Availability Suite 9.5 is installed on all newly deployed instances. In addition, a number of updates are made to the Veeam VSIs and Veeam configuration repositories.

### Zerto on IBM Cloud
{: #relnotes_v31-services-zerto}

For new instances deployed in V3.1 and later, Zerto on {{site.data.keyword.cloud_notm}} installations require a billable  {{site.data.keyword.cloud_notm}} account. The cost of VM replication now uses an {{site.data.keyword.cloud_notm}} billable plan instead of {{site.data.keyword.cloud_notm}} infrastructure billing.

To order Zerto on {{site.data.keyword.cloud_notm}}, your {{site.data.keyword.cloud_notm}} account must meet specific requirements. If you have an existing Zerto on {{site.data.keyword.cloud_notm}} installation, you can convert your Zerto VM billing type to billable.

## New and updated documentation
{: #relnotes_v31-updated-doc}

* The Activity Tracker instance management events and events for the KMIP for VMware on IBM Cloud service are updated.
* User ID reference documentation is updated with user IDs used for installation and configuration of services by {{site.data.keyword.cloud_notm}} services automation.
* Reference documentation is available for the new ``Retrieve the detailed network interface of a cluster`` API.
* The following technical documents are new in the *Reference* section of the user documentation:
  * Operations management
  * Day 2 operational procedures

## User interface updates and enhancements
{: #relnotes_v31-ui}

The user interface is updated and provides the following enhancements:
* The **Clusters** details page now displays the type of storage that the cluster uses.
* Various error messages and tooltip enhancements are available to assist you in selecting the appropriate setting on the user interface.
