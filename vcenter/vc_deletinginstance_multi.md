---

copyright:

  years:  2016, 2021

lastupdated: "2021-03-18"

keywords: vCenter Server delete instance, delete vCenter Server, delete multi-site

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Deleting vCenter Server instances in a multi-site configuration
{: #vc_deletinginstance_multi}

Be aware of the following special considerations before you plan to delete VMware vCenter Server® instances that are part of a multi-site configuration.

When you delete a vCenter Server instance, the following components are released sequentially:
1. All deployed services
2. Support and Services fee
3. VMware® product licenses
4. VMware ESXi™ servers
5. Subnets
6. VLANs

Any VLANs that have your own resources that are added to them, such as gateways, Veeam servers, and other resources, are not deleted. In addition, any existing VLANs that you own, outside of {{site.data.keyword.vmwaresolutions_full}}, are not deleted.
{:note}

Because of resource dependencies, the components in your instance are not released immediately when you delete the instance. For example, the subnets and VLANs cannot be deleted until the ESXi servers are fully reclaimed by the {{site.data.keyword.cloud_notm}} infrastructure, which happens at the end of the {{site.data.keyword.cloud_notm}} infrastructure billing cycle. At the end of the billing cycle, which is typically 30 days, the subnets and VLANs are deleted and the instance deletion is completed.

You are billed until the end of the {{site.data.keyword.cloud_notm}} infrastructure billing cycle for the deleted instance.
{:important}

## Procedure to delete vCenter Server instances in a multi-site configuration
{: #vc_deletinginstance_multi-procedure}
{: help}
{: support}

1. Delete all services from the secondary vCenter Server instance.
2. Ensure that no NSX objects are expanded into the secondary instance that you want to delete.
3. Delete the secondary vCenter Server from the primary SSO (Single Sign-On) domain. For more information, see [Unregister vCenter Server from Single Sign-On](https://kb.vmware.com/s/article/2106736){:external}.
4. Demote the local domain controller VSI (Virtual Service Instance). For more information, see [Demoting domain controllers and domains](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/demoting-domain-controllers-and-domains--level-200-){:external}.
5. Delete the secondary vCenter Server instance from the {{site.data.keyword.vmwaresolutions_short}} console.
6. Repeat steps 1 - 5 for all secondary vCenter Server instances in your multi-site configuration.
7. After deleting all secondary instances, you can also delete the primary instance from the {{site.data.keyword.vmwaresolutions_short}} console.

## Related links
{: #vc_deletinginstance_multi-related}

* [Deleting vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_deletinginstance)
* [Ordering, viewing, and deleting services from vCenter Server instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_addingremovingservices)
* [Managing virtual servers](/docs/virtual-servers?topic=virtual-servers-managing-virtual-servers)
