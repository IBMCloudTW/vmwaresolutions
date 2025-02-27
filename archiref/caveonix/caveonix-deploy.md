---

copyright:

  years:  2016, 2021

lastupdated: "2021-02-19"

subcollection: vmwaresolutions


---

{:tip: .tip}
{:note: .note}
{:important: .important}

# Deployment models for Caveonix RiskForesight
{: #caveonix-deploy}

This section describes the deployment models for Caveonix RiskForesight along with the installation process for installing the solution.

When you select the {{site.data.keyword.vmwaresolutions_full}} RiskForesight option, you do not have to follow all the steps in the deployment as the initial ones are automated. However, for an understanding of the complete deployment and architecture, and for those who want to scale out the solution after the initial deployment, a detailed understanding is required.

The RiskForesight installation consists of the following high-level steps:

1. [Initial planning and prerequisites](/docs/vmwaresolutions?topic=vmwaresolutions-caveonix-step1) – Understanding and selecting a deployment option, configuring DNS to provide FQDN/IP resolution for the application components.
2. [Virtual machine deployment](/docs/vmwaresolutions?topic=vmwaresolutions-caveonix-step2) – Deploying the VMs from an OVF template. The VM has installed on it all the application components.
3. [Application configuration](/docs/vmwaresolutions?topic=vmwaresolutions-caveonix-step3) – Running the Caveonix configuration script that configures the application component on each of the VMs.
4. [Application setup](/docs/vmwaresolutions?topic=vmwaresolutions-caveonix-step4) – Setting up the Service Provider and a Tenant or Organization so that the application becomes accessible for the users.

The automated installation provisions one VM and configures all the application components on that VM.
{:note}

## Deployment sizing
{: #caveonix-deploy-sizing}

The deployment sizing is calculated by using the following volumes.

| Data type | Volume |
|---|---|
| Scans per day | 1 |
| Scan data (MB) |  20 |
| Log data (MB) | 500 |
| Flow data (MB) | 200 |
| Asset data (MB) |46 |
| Total storage per asset per day (MB) | 766 |
| Data replication multiplier | 2 |
| Total index storage / Asset / Day (MB) | 1,532 |
{: caption="Table 1. Volumes" caption-side="top"}

The Data Replication Multiplier is set to 2 as the Index store (Elastic Search) uses by default n+1 replication of the indexes.
{:note}

The number of Scaling VMs is calculated from the number of Assets and the number of days of data to index.

| Number of assets | 100 | 500 | 5000 |
|---|---|---|---|
| Days of data | 30 | 30 | 30 |
| Total index storage / Asset / day (MB) | 1532 | 1532 | 1532 |
| Total index storage / Asset / 30 days (TB) | 4 | 22 | 219 |
| Data supported per scaling node (TB) | 0 | 8 | 8 |
|Scaling VMs required | 0 | 3 |27 |
{: caption="Table 2. Scaling VM parameters" caption-side="top"}

The amount of storage that is required is calculated as follows:

| Number of assets |100 | 500 | 5,000 |
|---|---|---|---|
| Long Term Data Retention (Months) |8 | 8 | 8 |
| Total Storage per Asset per Day (MB) | 766 | 766 | 766 |
| Days of Data | 30 | 30 | 30 |
| Near Term Data Retention (TB) | 7 | 33 | 329 |
| Long Term Data Retention (TB) | 18 | 88 | 877 |
{: caption="Table 3. Storage parameters" caption-side="top"}

From a data perspective, data is used as follows:
- Scan data is used in compliance management.
- Log data is used in forensic management.
- Policy and flow data is used in risk management. Flow data is available from NSX Manager only.

Data storage has three tiers:
- Replicated
- Near term
- Long term

The following table provides a summary of the deployments:

|Deployment model | All-in-one | Partially distributed | Fully distributed |
|---|---|---|---|
| Number of assets | 100 |500 | 5,000 |
| Online data generated in 30 days (TB) | 4 | 22 | 219 |
| Nearline data retention (90 days) (TB) | 7 | 33 | 329 |
| Offline data retention (8 months) (TB) | 18 | 88 | 877 |
| Total data storage retention (1 year) (TB) | 28 |142 | 1,425 |
| Base VMs | 1 | 1 | 20 |
| Scaling VMs | 0 | 3 | 28 |
| Total VMs | 1 | 4 | 48 |
{: caption="Table 4. Summary" caption-side="top"}

**Notes:**
When you remove the Caveonix RiskForesight service, the {{site.data.keyword.vmwaresolutions_short}} automation deletes only the single all-in-one Caveonix VM that was deployed and the dedicated private subnet that was ordered for it. Therefore,
* If you scaled out the Caveonix VM into multiple VMs, those additional VMs are not removed.
* If you used the IP addresses of the dedicated private subnet on additional VMs, those VMs must be assigned new IP addresses to continue to function.
* If you delete vCenter Server instance A with the Caveonix RiskForesight service installed, and you used the IP addresses of the dedicated private subnet ordered for the service in vCenter Server instance B, the dedicated private subnet is canceled upon deletion of vCenter Server instance A.

**Next topic:** [All-in-one deployment](/docs/vmwaresolutions?topic=vmwaresolutions-caveonix-allinone)
