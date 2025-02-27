---

copyright:

  years:  2016, 2021

lastupdated: "2021-02-22"

subcollection: vmwaresolutions


---

{:external: target="_blank" .external}

# Proactive tasks
{: #opsprocs-proactive}

The goal of these proactive checks is to enable system administrators to maintain the vCenter Server environment in a health state. When carried out daily, it should prevent many common issues that are related to usage, capacity and performance issues, from impacting your workloads. These proactive checks can be classified into the following structure:

* Health - Checks that indicate issues that are currently affecting the health of your environment and require immediate attention to minimize impact that is, hardware failures.
* Risk - Checks that indicate issues that are not immediate threats but should be addressed soon. For example, capacity issues.
* Efficiency - Checks that indicate areas to improve performance or reclaim resources. For example, right-sizing virtual machines (VMs) and clusters.

Many of these proactive tasks can be made much simpler with the Operations Management on {{site.data.keyword.cloud}} and reduce the management overhead.

With utilization, it is important to understand your "baseline". This baseline reflects normal operations in your environment. All clients have a different baseline based on their standard practices, workloads running on the vCenter Server instance etc. These proactive checks are then comparing the recent capacity/performance/utilization against this baseline. In summary these proactive usage checks are answering four questions:

1. Am I over-utilized now?
2. Will I run out of capacity soon?
3. Is my cluster under-sized for expected peak usage periods?
4. Can I reclaim unused resources from my virtual infrastructure?

## Guidelines
{: #opsprocs-proactive-guidelines}

The following guidelines assist you in being proactive and maintaining a stable environment:
* Plan your VM deployments by allocating enough resources for all the VMs you plan to run. Don't forget to allow resources for vSphere ESXi itself.
* Size your VMs correctly and allocate to each VM only as much virtual resources as that VM needs. Provisioning a VM with more resources than it needs might reduce the performance of that VM and of others on the same host.
* In general, 80% host CPU usage is a reasonable ceiling and 90% should be a warning that the CPUs are approaching an overloaded condition. If your hosts are approaching 80% CPU usage, provision additional hosts.
* In general, 80% host RAM usage is a reasonable ceiling and 90% should be a warning that the hosts are fully used. If your hosts are approaching 80% RAM usage provision additional hosts.
* It is important not to under-allocate memory, so allocate enough memory to hold the working set of the applications you plan to run in the VMs, to minimize thrashing. Thrashing can dramatically impact application performance. However, you should avoid substantially over-allocating memory as well.
* Use resource settings, reservation, shares, and limits, only if needed in your environment. If you expect frequent changes to the total available resources, use
shares, not reservation, to allocate resources fairly across VMs. Shares take effect only when there is resource contention.
* If you do need to use reservations, configure them to specify the minimum acceptable amount of CPU or memory, not the amount you would like to have available.
* Use resource pools for delegated resource management and to fully isolate a resource pool, make the resource pool type Fixed, and use Reservation and Limit.
* Group VMs for a multitier service into a resource pool. This allows resources to be assigned for the service as a whole. Carefully select the resource settings (that is, reservations, shares, and limits) for your VMs. Setting reservations too high can leave few unreserved resources in the cluster, thus limiting the options that DRS has to balance load.
* Setting limits too low might keep VMs from using extra resources available in the cluster to improve their performance.
* Run your clusters at no more than 80% usage to allow remediation of hosts by using VMware Update Manager. Cluster remediation is most likely to succeed when the cluster is running at maximum of 80% usage.
* There are many reasons to use thin provisioning of storage. However, thin provisioning increases the management workload of maintaining your environment as you need to be careful in your capacity management process.
*  Analyze your VM growth and understand how your infrastructure is getting used up to support this growth, order more capacity to facilitate this growth.
* When planning, use usable capacity, which takes into account HA and buffers, and not total available capacity.
* Ensure that you are running the latest version of the BIOS available for your system.
* Ensure that you are running the latest VMware updates to your installed products, this includes VM hardware and VM Tools.

## List of tasks
{: #opsprocs-proactive-tasks}

| Title | Description |
|:----- |:----------- |
| Health | Using vCenter, check the health of all hosts and VM objects. Methodically step through; clusters, hosts, datastores, VMs, and networks looking for Alarms.  |
| Alarm and notification check | Are there any active alarms in vCenter that you have not been notified of? By default, vCenter has a number of alarms that are defined when installed. These alarms might alert you to issues, however, by default, they take no action other than a warning or alert in the VMware vSphere Web Client. They can be configured to a notification such as an email. As you check the health of the hosts and VM objects, review the alarms, decide if a notification is needed, and configure as required. You only need to be notified for the most important or critical issues. Consider the following case: do I want to be notified of this issue at 2 AM or can it wait until normal working hours? Too many notifications are considered as "noise" and human nature is to ignore all alarms. |
| Cluster CPU and memory capacity/utilization check | Review the capacity/utilization metrics for the cluster's CPU and memory by using vCenter by navigating to each cluster in turn and selecting  Monitor, Performance. Review the graphs and statistics to ensure that your cluster has enough resources to satisfy demand. Demand should be based on maintaining enough capacity for VMs to burst when needed, a vSphere ESXi host to fail, and for VMs to be added according to your known service requests. |
| Datastore capacity/utilization check | Review the capacity/utilization metrics for the datastores by using vCenter by navigating to each datastore in turn and selecting Monitor, Performance, Space. Review the graphs and statistics to ensure that your datastore has enough space to satisfy demand. Demand should be based on maintaining enough capacity a vSphere ESXi host to fail (for a vSAN cluster) and for VMs to be added according to your known service requests. |
| Datastore performance check | Review the performance metrics for the datastores by using vCenter by navigating to each datastore in turn and selecting: Monitor, Performance, Performance, Real-time. Review the graphs and statistics to ensure that your datastore performance baseline is expected and that any changes can be accounted for. Investigate any abnormalities. |
| Bare metal server firmware | Best practices recommend to install the latest firmware updates for your bare metal server hosts. Check for firmware updates on the bare metal server hosts by using the Update Firmware option in the {{site.data.keyword.cloud_notm}} portal. On the page that is displayed, review the current and update version for system board and hard disk drives and see whether updates are available. If they are, plan to update the firmware in your next maintenance window. For more information, see [FAQs: Bare metal servers](/docs/bare-metal?topic=bare-metal-bm-faq#what-if-my-bare-metal-server-has-out-of-date-firmware-). |
| vSphere ESXi patching | For information about checking for the availability of vSphere ESXi patches, see [Creating baselines and attaching to inventory objects](/docs/vmwaresolutions?topic=vmwaresolutions-vum-baselines#vum-baselines). |
| VM hardware updates | For information about checking for the availability of VM hardware updates, see [Creating baselines and attaching to inventory objects](/docs/vmwaresolutions?topic=vmwaresolutions-vum-baselines#vum-baselines). |
| VM Tools updates | For more information, see [Creating baselines and attaching to inventory objects](/docs/vmwaresolutions?topic=vmwaresolutions-vum-baselines#vum-baselines). |
| vSphere vSAN patching | For information about checking for the availability of vSphere vSAN patches and the patch process, see [Updating vSAN clusters](/docs/vmwaresolutions?topic=vmwaresolutions-vum-updating-vsan#vum-updating-vsan). |
| vCenter patching | For more information about checking for the availability of VCSA patches and applying the update, see [VCSA update and SSO-linked vCenters](/docs/vmwaresolutions?topic=vmwaresolutions-vum-updating-vcsa#vum-updating-vcsa). |
| Updating NSX | For more information about checking for the availability of NSX patches and applying the upgrades, see [NSX patching](/docs/vmwaresolutions?topic=vmwaresolutions-vum-updating-nsx#vum-updating-nsx). |
| Check for VMs without VM Tools | It is good practice to install VM Tools as this allows greater interaction with the OS, that is, graceful power down of the VM. You can use vCenter to check which VMs do not have VM Tools installed. Go to the cluster, select **Related Objects**, **VMs**, and in the table enable the columns for **VM Tools running** and **VM Tools version**. Review the list and install VM Tools if needed. |
| VMs with snapshots | For information about best practices when working with snapshots, see [Best practices for using snapshots in the vSphere environment (1025279)](https://kb.vmware.com/s/article/1025279){:external}. It is important to identify the existence of VMs with snapshots as using a single snapshot for more than 72 hours creates a snapshot file that continues to grow in size and can cause the snapshot storage location to run out of space and impact the system performance. To review VMs with snapshots, connect to vCenter by using the Web Client, select the vCenter Server and go to Related Objects tab. Right-click on the column titles and go to Show/Hide Columns list. From the list of columns chose Needs Consolidation option. This column shows a summary of all the VMs that are currently running. |
| AD/DNS OS patching | The Microsoft Active Directory (AD) / Domain Name Server (DNS) is automatically set up to download the updates only. For more information, see [More limitations and considerations](/docs/vmwaresolutions?topic=vmwaresolutions-trbl_limitations#trbl_limitations) for further updating advice. |
| Check storage latency | Check storage latency to understand any changes for the vSphere ESXi hosts to access the datastores. Too high a latency causes applications that are hosted in the VMs to slow down. In vCenter, go to each of the datastores and, on the Performance tab, review the average write latency per VM. |
| Review VMs with virtual devices | Virtual devices such as CD or diskette drives create an overhead, therefore, remove any devices that are not needed for a VM. |
| vSAN capacity advice | When any capacity device in your cluster reaches 80% full, vSAN automatically rebalances the cluster until the space available on all capacity devices is below the threshold. The following operations can cause disk capacity to reach 80% and initiate cluster rebalancing: hardware failures, vSAN hosts are placed in maintenance mode with the Evacuate all data option, or vSAN hosts are placed in maintenance mode with Ensure data accessibility when objects assigned PFTT=0 reside on the host. To provide enough space for maintenance and reprotection, and to minimize automatic rebalancing of events in the vSAN cluster, consider keeping 30% capacity available always. |
| Cluster utilization check | Using vCenter, review each cluster and identify which clusters are at 50% or greater utilization for CPU and RAM. 50% is chosen as a warning level to focus attention on potential expansion of this cluster with additional hosts or clusters. The difference between 50% utilization and a maximum of 80 to 90%, is your room for additional VMs due to service requests. At the 50% limit, you should be looking at the near-future requests and forecasting when more resources need to be added. |
| Cluster consolidation review | Using vCenter, review each cluster and identify which clusters are at 30% or less utilization for CPU and RAM. 30% is chosen as a warning level to focus attention on potential right-sizing of this cluster by removing hosts or removing this cluster and moving VMs to another cluster. |
| Right-size over-sized VMs | Use a simple approach to right-size oversized VMs: identify, profile and tune, and monitor demand trends. Using vCenter identify large VMs that have the potential for right-sizing. Navigate to Monitor, Performance, and profile the average CPU and RAM demand profile of the workload over time and adjust the virtual resources. Finally, continue to monitor the workloads to see that the performance is acceptable. Ideally, consumed memory for the VM should be close to the memory used by the guest OS, plus overhead for running the VM.|
| Right-size under-sized VMs | Use a simple approach to right-size under-sized VMs; a. identify, b. profile and tune, and c. monitor demand trends. Identify small VMs that need right-sizing. Profile the average CPU and RAM demand profile of the workload over time and adjust the virtual resources. Finally, continue to monitor the workloads to see that the performance is acceptable. Ideally, consumed memory for the VM should be close to the memory used by the guest OS, plus overhead for running the VM.|
| Check VM hardware device compatibility | Using the VMware hardware compatibility online resource, check that your VMs' hardware resources; network, storage devices etc. are supported for that OS. If they are not supported change to a supported device to improve reliability and performance.  |
{: caption="Table 1. Proactive tasks" caption-side="top"}

**Next topic**: [Troubleshooting](/docs/vmwaresolutions?topic=vmwaresolutions-opsprocs-trouble)

## Related links
{: #opsprocs-proactive-links}

* [Operations Management on {{site.data.keyword.cloud_notm}}](/docs/vmwaresolutions?topic=vmwaresolutions-opsmgmt-intro)
