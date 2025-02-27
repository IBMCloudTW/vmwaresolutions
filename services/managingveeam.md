---

copyright:

  years:  2016, 2021

lastupdated: "2021-03-16"

keywords: Veeam console, Veeam backup restore, update Veeam license

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}

# Managing Veeam
{: #managingveeam}

After the service is deployed into your instance, you can access the Veeam® console by using Remote Desktop Protocol. With Remote Desktop Protocol, you can manage the backup and restore of all the virtual machines in your environment, including the management components. You can also upgrade the service by downloading and installing the Veeam updates from the Veeam website.

Veeam v11 is installed on newly deployed instances. If you have Veeam v9.5u4b, you can continue to use it. However, you cannot install Veeam v9.5u4b on a new instance.

## Tasks that you can complete with Veeam v11
{: #managingveeam-fivetasks_v10}

* Order a new vCenter Server instance with Veeam

   A Veeam stand-alone license is automatically deployed. The Veeam service installation starts only after the stand-alone license was successfully ordered.

   Veeam v11 is installed on a bare metal server, VM, or VSI, depending on what you selected.

* Install Veeam on an existing VMware vCenter Server® instance

   A Veeam stand-alone license is automatically deployed. The Veeam service installation starts only after the stand-alone license was successfully ordered.

   Veeam v11 is installed on a bare metal server, VM, or VSI depending on what you selected.

* Uninstall Veeam on an existing vCenter Server instance

   If you uninstall an older version of Veeam, for example Veeam v9.5u4b, the license charge for that version is canceled because the license is attached to the older version.

   However, if you uninstall Veeam v11, the stand-alone license instance is not deleted. To cancel the license charge, you must delete the license separately.

   For more information, see [Managing Veeam licenses](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_managing_licenses).

* Order a new Veeam stand-alone license

   A SKU in IMS is ordered with the number of VMs to license. A license key is not displayed because no unique keys are generated.

   For more information, see [Managing Veeam licenses](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_managing_licenses).

* Delete a Veeam stand-alone license

   When you delete a Veeam stand-alone license, the license charge in IMS is canceled. There is no effect on the Veeam installation.

   When you want to increase your usage, delete your old stand-alone instance after you order a new instance to avoid unnecessary charges.

   For more information, see [Managing Veeam licenses](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_managing_licenses).

## Considerations for installing and deleting Veeam licenses
{: #managingveeam-install-delete-consid}

The following information about licenses applies to both Veeam v9.5u4b and Veeam v11.

Starting with Veeam v10 and continuing with Veeam v11, Veeam licenses are installed and deleted in a different way than previous Veeam versions.

After you place an order for Veeam v11, a Veeam stand-alone license is automatically deployed. The Veeam service installation begins only after the stand-alone license order completed successfully. After that, depending on what you ordered, Veeam is installed on the bare metal server, VM, or VSI.

When you uninstall Veeam v11, the stand-alone license instance is not deleted. You must delete it separately to avoid being charged for the stand-alone license.

You might want to consider this when you plan to upgrade your Veeam usage. You might be charged for licenses for Veeam 9.5, which is deprecated, and for Veeam 11. Therefore, you might decide to order a new license for your Veeam installation toward the end of a month, so you aren’t charged for both licenses for most of the month.

If you have an existing Veeam v9.5u4b installation that comes with a license and you want more coverage, you can keep your license that came with the installation and order a new license to upgrade usage. Later, if you delete Veeam v9.5u4b, you must delete separately any stand-alone licenses that you ordered. Otherwise, you continue to be charged for them.

Although you can order a new stand-alone license, stacking licenses is not recommended. For example, if you initially ordered Veeam with 20 VMs and now want coverage for 40 VMs, do not keep your old license and order a new license for 20 VMs. Instead, order a new license for 40 VMs and delete your original 20 VM license.

When licenses are ordered, the system does not generate a key, but applies a primary key.

## Accessing the Veeam console by using Remote Desktop Protocol
{: #managingveeam-accessing}

To manage the Veeam service, access the Veeam console by completing the following steps:
1. Use Remote Desktop Protocol to connect to the Windows® IP address.
2. Log in to the Windows console by using the Administrator credentials.
3. Access the Veeam console from the Windows console by using the Administrator credentials.

You can find the Windows IP address and the Administrator credentials on the Veeam service details page.

For more information, see [Ordering, viewing, and deleting services for vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_addingremovingservices).

## Backing up and restoring management components for instances with Veeam installations
{: #managing-veeam-backup-and-replication}

The Veeam service can be configured to back up the management components by using the Veeam console. For more information, see [Backing up components](/docs/vmwaresolutions?topic=vmwaresolutions-solution_backingup).

For instances deployed in (or upgraded to) V1.8 or later releases, the configuration changes to your environment are not automatically backed up. Therefore, before you change the configuration of your environment, it is recommended that you back up the management components manually by running the management backup job in the Veeam console. For more information about backing up manually, see the [Veeam technical instructions](https://helpcenter.veeam.com/backup/vsphere/scheduing_manual.html){:external}.

When failures occur on the management components, you can restore the management components to a previous backup by using the Veeam console. For more information about restoring manually, see the [Veeam technical instructions]( https://helpcenter.veeam.com/backup/vsphere/performing_full_recovery.html){:external}.

## Applying updates to Veeam
{: #managingveeam-updates}

You are responsible for maintaining the Veeam software to keep it updated to the most recent version.

### Applying updates to instances deployed with public and private network
{: #managingveeam-updates-public-private}

If the Veeam service is installed on an instance with public and private network, you can check for and download the updates by using the Veeam software itself.

### Applying updates to instances deployed with private network only
{: #managingveeam-updates-private}

If the Veeam service is installed on an instance with private network only, you cannot check for or download updates by using the Veeam software. The reason is because the Veeam VSI or VM is configured with no public network access. Instead, you must download updates from the Veeam website, transfer them to the Veeam VM, and then install them.

### Updating Veeam licenses for instances that are deployed with public and private network
{: #managingveeam-update-license-public-private}

If the Veeam service is installed on an instance with public and private network, you can update your Veeam v9.5u4b license either automatically or manually. For Veeam v11, you must update your Veeam license automatically. You cannot update it manually. Follow the Veeam instructions at [Updating license](https://helpcenter.veeam.com/docs/backup/vsphere/license_update.html){:external}.

### Updating Veeam licenses for instances that are deployed with private network only
{: #managingveeam-update-license-private}

If the Veeam service is installed on an instance with private network only, take note of the expiration date for your license. When renewal is needed, [contact {{site.data.keyword.IBM}} Support](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_support) to get assistance with updating the license key.

### Known issue about license key message for automatic updating
{: #managingveeam-known-issue-message}

On the Veeam v11 console, when you’re automatically updating a license before it expires, you might see a `License key is up-to-date` message that looks like an error. There is no error and you can ignore the message.

## Related links
{: #managingveeam-related}

* [Veeam v9.5 overview](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_considerations)
* [Veeam v11 overview](/docs/vmwaresolutions?topic=vmwaresolutions-veeamvm_overview)
* [Ordering Veeam](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_ordering)
* [Ordering Veeam licenses](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_ordering_licenses)
* [Managing Veeam licenses](/docs/vmwaresolutions?topic=vmwaresolutions-veeam_managing_licenses)
* [Ordering and configuring IBM Cloud Object Storage with Veeam](/docs/vmwaresolutions?topic=vmwaresolutions-icos_ordering)
* [VMware Solutions FAQ](/docs/vmwaresolutions?topic=vmwaresolutions-faq-vmwaresolutions)
* [Veeam backup and replication](https://www.ibm.com/cloud/architecture/architectures/virtualization_backup_veeam){:external}
* [Veeam backup and replication FAQ](https://www.veeam.com/availability-suite-faq.html){:external}
* [Veeam technical documentation](https://www.veeam.com/documentation-guides-datasheets.html){:external}
