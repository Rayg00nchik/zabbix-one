
# vCenter Appliance

## Overview

A simple template to monitor status of vCenter Appliance Components by Zabbix that works without any external scripts.
<p>
The template uses vCenter REST API instead of SNMP to obtain data.
</p>

## Requirements

Zabbix version: 6.4 and higher (it's not tested on Zabbix 5.0+, but it may work)

## Tested versions

This template has been tested on:
- vCenter 7.0.3

## Configuration

> No additional configuration is needed

## Setup

1. Create a vCenter read-only user to obtain specific information:
- Open you vCenter via `http(s)://<vcenter_fqdn>/ui/`
- Create a new role in vCenter and new group or user (optionaly) under "Administration" section
- Assign read priviliegs to the role (optionaly)
- Assign read permisiions to the new group  (vStat, read only etc.)
- In vCenter "Administration" -> "single sign-on" -> "user and groups" click on  "groups" and find "SystemConfiguration.ReadOnly" group. 
- Add the previously created group there.
2. Import the template.
3. Set the host macroses (on host or template level) before using:
```
{$VCENTER_URL_SESSION}
{$VMWARE.APP.DATABASE.STATUS.URL}
{$VMWARE.APP.HEALTH.SERVICES.URL}
{$VMWARE.APP.LOAD.STATUS.URL}
{$VMWARE.APP.MEMORY.STATUS.URL}
{$VMWARE.APP.SOFTWARE.STATUS.URL}
{$VMWARE.APP.STORAGE.STATUS.URL}
{$VMWARE.APP.SWAP.STATUS.URL}
{$VMWARE.APP.SYSTEM.LASTCHECK.STATUS.URL}
{$VMWARE.APP.SYSTEM.STATUS.URL}
{$VMWARE.PASSWORD}
{$VMWARE.URL}
{$VMWARE.USERNAME}
```
4. Link the template to host created early.

## Macros used

|Name|Descriptionion|Default|
|----|-----------|-------|
|{$VCENTER_URL_SESSION}|<p>vCenter Url to obtain authentification token (value)</p>|https://<vcenter_fqdn>/rest/com/vmware/cis/session|
|{$VMWARE.APP.DATABASE.STATUS.URL}|<p>vCenter Appliance database storage status url</p>|https://<vcenter_fqdn>/rest/appliance/health/database-storage|
|{$VMWARE.APP.HEALTH.SERVICES.URL}|<p>vCenter Appliance services status url (health)</p>|https://<vcenter_fqdn>/rest/appliance/health/applmgmt|
|{$VMWARE.APP.LOAD.STATUS.URL}|<p>vCenter Appliance load status url</p>|https://<vcenter_fqdn>/rest/appliance/health/load|
|{$VMWARE.APP.MEMORY.STATUS.URL}|<p>vCenter Appliance memory status url</p>|https://<vcenter_fqdn>/rest/appliance/health/mem|
|{$VMWARE.APP.SOFTWARE.STATUS.URL}|<p>vCenter Appliance software status url</p>|https://<vcenter_fqdn>/rest/appliance/health/software-packages|
|{$VMWARE.APP.STORAGE.STATUS.URL}|<p>vCenter Appliance storage status url</p>|https://<vcenter_fqdn>/rest/appliance/health/storage|
|{$VMWARE.APP.SWAP.STATUS.URL}|<p>vCenter Appliance swap status url</p>|https://<vcenter_fqdn>/rest/appliance/health/swap|
|{$VMWARE.APP.SYSTEM.LASTCHECK.STATUS.URL}|<p>vCenter Appliance system last check url</p>|https://<vcenter_fqdn>/rest/appliance/health/system/lastcheck|
|{$VMWARE.APP.SYSTEM.STATUS.URL}|<p>vCenter Appliance system status url</p>|https://<vcenter_fqdn>/rest/appliance/health/system|
|{$VMWARE.PASSWORD}|<p>VMware service user password</p>|password|
|{$VMWARE.URL}|<p>VMware service - vCenter SDK URL</p>|https://<vcenter_fqdn>/sdk|
|{$VMWARE.USERNAME}|<p>VMware service user name</p>|username|

## Template links

There are no template links in this template.

## Items

|Name|Descriptionion|Type|Key and additional info|
|----|-----------|----|-----------------------|
|vCenter Appliance Database Storage Status|<p>Getting appliance database storage status</p>|`SCRIPT`|<p>vcenter.appliance.database.storage.status</p>|
|vCenter Appliance Load Status|<p>Getting appliance load status</p>|`SCRIPT`|<p>vcenter.appliance.load.status</p>|
|vCenter Appliance Memory Status|<p>Getting appliance memory health status</p>|`SCRIPT`|<p>vcenter.appliance.memory.status</p>|
|vCenter Appliance Services Status|<p>Getting appliance services status</p>|`SCRIPT`|<p>vcenter.appliance.services.status</p>|
|vCenter Appliance Software Packages Updates Status|<p>Getting appliance software packages updates health status</p>|`SCRIPT`|<p>vcenter.appliance.software.packages.updates.status</p>|
|vCenter Appliance Storage Status|<p>Getting appliance storage health status</p>|`SCRIPT`|<p>vcenter.appliance.storage.status</p>|
|vCenter Appliance Swap Status|<p>Getting appliance swap health status</p>|`SCRIPT`|<p>vcenter.appliance.swap.status</p>|
|vCenter Appliance System Last Check|<p>Getting appliance system last check</p>|`SCRIPT`|<p>vcenter.appliance.system.last.check</p>|
|vCenter Appliance System Status|<p>Getting appliance system health status</p>|`SCRIPT`|<p>vcenter.appliance.system.status</p>|

## Triggers

|Name|Descriptionion|Expression|Severity|Dependencies and additional info|
|----|-----------|----------|--------|--------------------------------|
|vCenter Appliance Database Storage is not Healthy|<p>Check the vCenter Appliance Database Storage please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.database.storage.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Load is not Healthy|<p>Check the vCenter Appliance Load please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.load.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Memory Status is not Healthy|<pCheck the vCenter Appliance Memory please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.memory.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Services Status is not Healthy|<p>Check the vCenter Appliance Services please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.services.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Software Packages Updates Status is not Healthy|<p>Check the vCenter Appliance Software Packages please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.software.packages.updates.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Storage Status is not Healthy|<p>Check the vCenter Appliance Storage please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.storage.status,#3,"eq","green")=0'`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|vCenter Appliance Swap Status is not Healthy|<p>Check the vCenter Appliance Swap please</p>|`find(/vmware_vcenter_appliance/vcenter.appliance.swap.status,#3,"eq","green")=0`|High|**Depends on**:<br><ul><li><p>Unable connect to vCenter Appliance</p></li></ul>|
|Unable to connect to vCenter|<p>Check the connection to vCenter Appliance please</p>|`nodata(/vmware_vcenter_appliance/vcenter.appliance.system.status,120)=1`|Disaster||


##Feedback

Please report any issues with the template [`here`](https://github.com/Rayg00nchik/zabbix-one/issues/new?assignees=&labels=bug&template=BUG_REPORT.md&title=Issues%3A+Bug+Report)

You can give feedback [`here`](mailto:rayg00nchik@gmail.com)
