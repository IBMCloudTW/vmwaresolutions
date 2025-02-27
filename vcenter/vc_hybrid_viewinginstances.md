---

copyright:

  years:  2016, 2021

lastupdated: "2021-01-29"

keywords: view vCenter Server Hybridity, view instance, view instance details

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Viewing vCenter Server with Hybridity Bundle instances
{: #vc_hybrid_viewinginstances}

View the summary and detailed information of the VMware vCenter Server® with Hybridity Bundle instances that are provisioned for different user accounts.

## Procedure to view vCenter Server with Hybridity Bundle instances summary
{: #vc_hybrid_viewinginstances-procedure-view-inst-summary}

To view a summary of all the vCenter Server with Hybridity Bundle instances that are provisioned for a user account, complete the following steps:
1. In the {{site.data.keyword.vmwaresolutions_full}} console, click **Resources** from the left navigation pane.
2. From the console banner, click your user account icon, and then click the **Account** field to select the user account that you want to check instances for.
3. In the **vCenter Server instances** table, view the list of instances that are provisioned in the selected user account.

| Item        | Description   |
|:----------- |:------------- |
| Name | The name of the instance. |
| Type | The type of vCenter Server instance. |
| Version | The release version that the instance was deployed in, or upgraded to. |  
| Location | The {{site.data.keyword.cloud_notm}} data center where the instance is hosted. |  
| Creation time | The date and time when the instance was created. |  
| Status | The status of the instance. |
{: caption="Table 1. vCenter Server with Hybridity Bundle instance items" caption-side="top"}

The instance can have different statuses.

| Status        | Description |
|:------------- |:----------- |
| Creating | The instance is being created. |
| Building | The instance is being configured. |
| Ready to use | The instance is ready to use. |
| Modifying | The instance is being modified. |
| Failed | The creation, configuration, or modification process failed. |
| Deleting | The instance is being deleted. |
| Deletion Error | An error occurred when the instance was being deleted. |
| Deleted | The instance is deleted. |
{: caption="Table 2. vCenter Server with Hybridity Bundle instances status descriptions" caption-side="top"}

## Procedure to view vCenter Server with Hybridity Bundle instances property details
{: #vc_hybrid_viewinginstances-procedure-view-inst-property}

To view the property details of a vCenter Server with Hybridity Bundle instance:
1. In the **vCenter Server instances** table, click an instance name.
2. Under **Properties**, view the details for the instance.

| Property        | Description       |
|:------------- |:------------- |
| Name | The name of the instance. |
| ID | The ID of the instance. |
| Location | The {{site.data.keyword.cloud_notm}} data center where the instance is hosted. |
| Current version | The current version of {{site.data.keyword.vmwaresolutions_short}}. |
| vCenter version | The VMware vCenter Server with Hybridity Bundle version.<br><br>**Note:** There is a slight variation between the vCenter Server version displayed on the {{site.data.keyword.vmwaresolutions_short}} console and the VMware vSphere® Web Client. Both are correct. |
| NSX for vSphere | The VMware NSX® for vSphere product version. |
| NSX license edition | The version and edition of the VMware NSX license. |
| DNS, Root domain | The root domain name is the DNS (Domain Name System) domain name and the Microsoft® Active Directory™ (AD) forest root name. |
| DNS, SSO domain | The SSO domain is the vSphere Single Sign-On domain. The SSO domain name is fixed for all deployed vCenter Server with Hybridity Bundle instances with a value of <samp class="ph codeph">vsphere.local</samp>. |
| DNS, Subdomain | The subdomain is the DNS subdomain name of the root domain name where the local vCenter Server with Hybridity Bundle instance host names reside. The subdomain name is in the format <samp class="ph codeph"><var class="keyword varname">vcenter_server_instance_name</var>.<var class="keyword varname">root.domain_name</var></samp>. |
| Hybridity Bundle | Indicates if the vCenter Server with Hybridity Bundle is installed. |
| Status  | The status of the instance.<br><br>The information that is displayed provides an update on the progress of the deployment or the action that is taken on the instance. If there are issues, a message might be displayed to help you investigate and resolve the problem. |
{: caption="Table 3. vCenter Server with Hybridity Bundle instances properties" caption-side="top"}

## Procedure to view access information for vCenter Server with Hybridity Bundle instances
{: #vc_hybrid_viewinginstances-procedure-view-access-info}

Under **Access information**, view the access information for the instance-related components. The passwords displayed are initial passwords that are generated by the system. If you change them outside of the {{site.data.keyword.vmwaresolutions_short}} console, they are not updated on the instance summary page.

| Component        | Description       |
|:------------- |:------------- |
| AD/DNS IPs | The IP addresses of the two AD servers. |
| AD/DNS FQDNs | The AD/DNS server fully qualified domain names.<br><br>**Note:** The same administrator password can be used to connect to all the AD/DNS servers by using a remote desktop connection. |
| AD/DNS ADMIN (Remote Desktop)  | For primary instances, it displays the user name and password to access the AD server via a remote desktop connection.<br><br>For secondary instances, click the **View on primary instance** link to be directed to the user name and password information on the primary instance.<br><br>**Note:** After the secondary instance is added to the primary DNS domain and replication occurs, the local administrator password on the primary instance might overwrite the local administrator password on the secondary instance. By clicking the **View on primary instance** link, you will get access to the correct administrator password.  
| NSX Manager IP  | The IP address of the NSX Manager.  |
| NSX Manager FQDN  | The NSX Manager fully qualified domain name (FQDN).  |
| NSX Manager HTTP  | The user name and password used to access the NSX Manager web console. |
| vCenter IP  | The IP address of the vCenter Server.  |
| vCenter FQDN  | The vCenter Server fully qualified domain name (FQDN).  |
| vCenter ADMIN  | The VMware vCenter Single Sign-On user name and password that you can use to log in to the vCenter Server by using the vSphere Web Client.  |
| vCenter SSH  | The user name and password that you can use to access the vCenter Server VM via SSH connection.  |
{: caption="Table 4. vCenter Server with Hybridity Bundle access information for instance-related components" caption-side="top"}

## Procedure to view the deployment history for vCenter Server with Hybridity Bundle instances
{: #vc_hybrid_viewinginstances-procedure-view-deploy-history}

Click **Deployment History** from the left navigation pane to view the deployment history for the instance.

| Item        | Description       |  
|:------------- |:------------- |
| Date | The date and time when the instance status is changed. |
| Summary | The details of the change. |
{: caption="Table 5. vCenter Server with Hybridity Bundle instance deployment history" caption-side="top"}

## What to do if errors occur
{: #vc_hybrid_viewinginstances-if-errors-occur}

If errors occur during instance deployment or instance deletion, the {{site.data.keyword.cloud_notm}} Support team is automatically notified. To inquire about the status of your ticket, you can [contact IBM Support](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_support).

## What to do next
{: #vc_hybrid_viewinginstances-next}

Manage your instances from the {{site.data.keyword.vmwaresolutions_short}} console or the VMware vSphere Web Client.

Before you click **vCenter console** on the instance summary page to go to the vSphere Web Client and start managing your VMware ESXi™ servers, you must log in to the VPN portal of the {{site.data.keyword.cloud_notm}} data center. Hover over the **vCenter console** button and follow the instructions to ensure that you meet all requirements and you completed the necessary steps before you access the vSphere Web Client.
{:important}

Review the following topics for information to help you complete the login instructions:
*  For the requirements and necessary steps before you access the vSphere Web Client, see [Timeout reached while connecting to the vSphere Web Client](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_timeout_vc_console).
*  For a list of access points to log in to the {{site.data.keyword.cloud_notm}} infrastructure Private Network using VPN, see [VPN Access](https://www.ibm.com/cloud/vpn-access){:external}.
*  If you have problems when you deploy an OVF (Open Virtualization Format) file using the vSphere Web Client, see [Deploying an OVF file using the vSphere Web Client](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_deploy_ovf).

## Related links
{: #vc_hybrid_viewinginstances-related}

* [Adding, viewing, and deleting clusters for vCenter Server with Hybridity Bundle instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_hybrid_addingviewingclusters)
* [Expanding and contracting capacity for vCenter Server with Hybridity Bundle instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_hybrid_addingremovingservers)
* [Deleting vCenter Server with Hybridity Bundle instances](/docs/vmwaresolutions?topic=vmwaresolutions-vc_hybrid_deletinginstance)
