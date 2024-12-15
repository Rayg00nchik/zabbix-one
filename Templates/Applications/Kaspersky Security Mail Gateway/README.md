# Kaspersky Security Mail Gateway  

## Overview

<p>
A template to monitor status of Kaspersky Security Mail Gateway 2.0+ instance via SNMP agent v2.
SNMP-Traps included.
</p>
<p>
Kaspersky Mail Security Gateway OID MIB used:
https://support.kaspersky.com/KSMG/2.0/en-US/181254.htm
</p>

## Author 
Rayg00nchik

## Requirements

Zabbix version: 
- 6.4 and higher 

## Tested versions

The template has been tested on:
- Kaspersky Security Mail Gateway 2.0 
- Kaspersky Security Mail Gateway 2.1+

## Configuration

- Zabbix snmp-trap configuration is required to gather snmp traps. See the [link](https://www.zabbix.com/documentation/6.4/en/manual/config/items/itemtypes/snmptrap)


## Setup

1. Connect to your KSMG server via SSH and add the following strings to the file /etc/snmp/snmpd.conf to make connections through the UNIX socket:
    
    master agentx
    agentXSocket unix:/var/run/agentx-master.socket
    agentXPerms 770 770 kluser klusers

2. Check the settings in /etc/snmp/snmpd.conf:
        
    #com2sec notConfigUser  default       public
    com2sec notConfigUser  default       <ksmg-community-name>

    view    systemview    included   .1.3.6.1.2.1.1
    view    systemview    included   .1.3.6.1.2.1.25.1.1
    view    systemview    included   .1.3.6.1.4.1.

    If there are no such strings, add it!

    Change <ksmg-community-name> to any name you want to use.

3. Restart the snmpd service. To do it, run the following command:
    
    systemctl restart snmpd

4. Add the snmpd service to autostart. To do it, run the following command:
    
    systemctl enable snmpd
    
    The snmpd service will be configured. 
    To enable the application to operate over the SNMP protocol, enable use of SNMP in the application web interface:
    https://support.kaspersky.com/KSMG/2.0/en-US/91248.htm.

5. Check the availability of snmptd from your zabbix server\proxy like:
        
    snmpwalk -v2c -c <ksmg-community-name> <you-ksmg-server-ip-address> .1.3.6.1.4.1.23668.1735.2.1.2.0
    
    If you don't have "snmpwalk" - just install it according to your operating system.

## Macros used

No macroses.

## Template links

There are no template links in this template.

## Discovery rules

There are no discovery rules in this template.

## Items collected

|Name|Description|Type|Key and additional info|
|----|-----------|----|-----------------------|
|Antifishing DB obsoleted|<p>Anti-Phishing databases are severely out of date.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.160]</p>|
|Antifishing DB outdated|<p>Anti-Phishing databases are out of date.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.150]</p>|
|Antispam DB obsoleted|<p>Anti-Spam databases are obsolete.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.140]</p>|
|Antispam DB outdated|<p>Antispam DB outdated</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.130]</p>|
|Antispam module error|<p>Anti-Spam module error.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.530]</p>|
|Antivir DB obsoleted|<p>Anti-Virus databases are severely out of date.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.120]</p>|
|Antivir DB outdated|<p>Anti-Virus databases are out of date.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.100]</p>|
|Antivir module error|<p>Anti-Virus module error.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.520]</p>|
|Cluster consistency error|<p>Server status error.</p><p>For example, there is no server with the Control node role.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.1600]</p>|
|Cluster emergency state|<p>Application switched to emergency mode.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.1610]</p>|
|Cluster sync error|<p>Error synchronizing settings between the node with role Control and nodes with role Secondary.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.1620]</p>|
|Compilation antispam DB error|<p>Compilation of Anti-Spam databases ended with an error.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.30]</p>|
|Daemon crashed|<p>Application process crashed.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.400]</p>|
|Daemon restarted|<p>Application process restarted.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.410]</p>|
|Error adding backup|<p>Error adding message to Backup.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.200]</p>|
|Error deleting backup|<p>Error deleting messages from Backup.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.210]</p>|
|KSN connection status changed|<p>The status of the connection with the KSN server has changed.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.700]</p>|
|Ldap sync|<p>Data synchronization with Active Directory is complete.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.910]</p>|
|License blacklisted|<p>Activation code or key file has been added to the denylist.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.350]</p>|
|License deleted|<p>Activation code or key file has been removed.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.310]</p>|
|License expired|<p>License expired.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.330]</p>|
|License installed|<p>Activation code or key file has been added.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.300]</p>|
|License soon expired|<p>License expires soon.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.320]</p>|
|License status|<p>Current status of the license key.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.8.5.0 </p><p>Update Interval: 1m</p>|
|License updated|<p>License key status changed.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.360]</p>|
|Number of all messages|<p>Total number of processed messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.11.0</p><p>Update Interval: 1m</p>|
|Number of clean messages|<p>Number of scanned messages in which no threats were detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.1.0 </p><p>Update Interval: 1m</p>|
|Number of content filter rejected size|<p>Number of messages that were rejected based on the Content Filtering settings.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.5.0</p><p>Update Interval: 1m</p>|
|Number of deleted messages|<p>Number of deleted messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.4.0</p><p>Update Interval: 1m</p>|
|Number of disinfected messages|<p>Number of disinfected messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.2.0</p><p>Update Interval: 1m</p>|
|Number of encrypted messages|<p>Number of messages whose encrypted (password-protected) attachments could not be scanned.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.4.0</p><p>Update Interval: 1m</p>|
|Number of fishing messages|<p>Number of messages with phishing content.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.13.0</p><p>Update Interval: 1m</p>|
|Number of infected messages|<p>Number of messages in which threats were detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.2.0</p><p>Update Interval: 1m</p>|
|Number of mass mail messages|<p>Number of messages identified as mass mail.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.9.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which content errors occurred with bases or license|<p>Number of messages that were excluded from Content Filtering scans due to licensing issues or problems with the application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.6.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which errors occurred|<p>Number of messages whose processing resulted in errors.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.12.4.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which fishing errors occurred|<p>Number of messages whose processing resulted in errors.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.10.4.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which spam errors occurred|<p>Number of messages whose processing resulted in errors.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.6.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which spam errors occurred with bases or license|<p>Number of messages that were excluded from Anti-Spam scans due to licensing issues or problems with the application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.8.0</p><p>Update Interval: 1m</p>|
|Number of messages during the processing of which spam errors occurred with fishing|<p>Number of messages that were excluded from Anti-Phishing scans due to licensing issues or problems with the application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.10.6.0</p><p>Update Interval: 1m</p>|
|Number of messages in storage|<p>Number of objects currently in Backup</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.1.1.0</p><p>Update Interval: 1m</p>|
|Number of messages non checked spam|<p>Number of messages that were excluded from Anti-Spam scans based on the defined settings of the Anti-Spam module.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.7.0</p><p>Update Interval: 1m</p>|
|Number of messages non cheked fishing|<p>Number of messages that were excluded from phishing scans based on the defined settings of the Anti-Phishing module.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.10.5.0</p><p>Update Interval: 1m</p>|
|Number of messages with deleted attachments|<p>Number of messages whose infected attachments were deleted.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.3.0</p><p>Update Interval: 1m</p>|
|Number of messages with denied attachments|<p>Number of messages containing prohibited types of attachments.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.3.0</p><p>Update Interval: 1m</p>|
|Number of messages with denied attachments names|<p>Number of messages containing attachments with prohibited names.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.4.0</p><p>Update Interval: 1m</p>|
|Number of messages with fishing|<p>Number of messages in which phishing content was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.10.2.0</p><p>Update Interval: 1m</p>|
|Number of messages with macroses|<p>Number of messages containing attachments with macros.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.5.0</p><p>Update Interval: 1m</p>|
|Number of messages with malicious links|<p>Number of messages in which the program detected malicious advertising links or links associated with legitimate applications that could be exploited by hackers.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.12.3.0</p><p>Update Interval: 1m</p>|
|Number of messages without any check|<p>Number of messages for which no action was taken based on the scan results by all enabled application modules.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.1.0</p><p>Update Interval: 1m</p>|
|Number of messages without contenf filter check|<p>Number of messages that were excluded from Content Filtering scans based on the defined settings.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.5.0</p><p>Update Interval: 1m</p>|
|Number of messages without fishing|<p>Number of scanned messages in which no phishing content was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.10.1.0</p><p>Update Interval: 1m</p>|
|Number of messages without malicious links|<p>Number of scanned messages in which no links were detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.12.1.0</p><p>Update Interval: 1m</p>|
|Number of messages without spam|<p>Number of scanned messages in which no spam was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.1.0</p><p>Update Interval: 1m</p>|
|Number of messages without spam content|<p>Number of scanned objects for which no action was taken.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.1.0 </p><p>Update Interval: 1m</p>|
|Number of messages with probable spam|<p>Number of messages in which probable spam was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.3.0</p><p>Update Interval: 1m</p>|
|Number of messages with size exceeded|<p>Number of objects that were larger than the maximum allowed size defined in the Content Filtering settings.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.4.2.0</p><p>Update Interval: 1m</p>|
|Number of messages with spam|<p>Number of messages in which spam was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.2.0</p><p>Update Interval: 1m</p>|
|Number of messages with spam sent to quarantined|<p>Number of messages put in Anti-Spam Quarantine.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.3.5.0</p><p>Update Interval: 1m</p>|
|Number of messages with threats|<p>Number of messages in which threats were detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.1.0</p><p>Update Interval: 1m</p>|
|Number of non checked messages whith any modules error|<p>Number of messages in which at least one scan module detected a threat or generated a scan error and for which the Skip action was performed.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.7.0</p><p>Update Interval: 1m</p>|
|Number of non check messages|<p>Number of messages that were excluded from threat scans based on the defined settings of the Anti-Virus module.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.7.0</p><p>Update Interval: 1m</p>|
|Number of non detected messages|<p>Number of scanned messages in which nothing was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.9.0</p><p>Update Interval: 1m</p>|
|Number of non scanтed messages|<p>Number of unscanned messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.7.0</p><p>Update Interval: 1m</p>|
|Number of non scanned messages (violations)|<p>Number of messages that were excluded from threat scans due to licensing issues or problems with the application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.2.8.0</p><p>Update Interval: 1m</p>|
|Number of quarantined messages with processing delay|<p>Number of messages put in Quarantine because their processing was postponed.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.6.0</p><p>Update Interval: 1m</p>|
|Number of rejected messages|<p>Number of rejected messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.5.0</p><p>Update Interval: 1m</p>|
|Number of smap messages|<p>Number of messages in which spam was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.3.0</p><p>Update Interval: 1m</p>|
|Number of unverified messages with links due to issues|<p>Number of messages that were excluded from malicious link scans due to licensing issues or problems with the application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.12.6.0</p><p>Update Interval: 1m</p>|
|Size all messages|<p>Total size of all processed messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.12.0</p><p>Update Interval: 1m</p>|
|Size fishing messages|<p>Total size of messages with phishing content.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.14.0</p><p>Update Interval: 1m</p>|
|Size non detected messages|<p>Total size of scanned messages in which nothing was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.10.0</p><p>Update Interval: 1m</p>|
|Size non scanned messages|<p>Total size of unscanned messages.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.8.0</p><p>Update Interval: 1m</p>|
|Size rejected content filter messages|<p>Total size of messages that were rejected based on the Content Filtering settings.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.6.0</p><p>Update Interval: 1m</p>|
|Spam messages size|<p>Total size of messages in which spam was detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.4.0</p><p>Update Interval: 1m</p>|
|Storage is full|<p>Maximum allowable size of Backup reached.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.220]</p>|
|Storage size|<p>Disk space occupied by Backup.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.1.2.0</p><p>Update Interval: 1m</p>|
|The number of messages not checked by any module due to unavailability of application databases|<p>Number of messages that were skipped by all modules due to inaccessible application databases.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.5.8.0</p><p>Update Interval: 1m</p>|
|Threat detected|<p>Threat detected.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.510]</p>|
|Threat message size|<p>Total size of messages in which threats were detected.</p>|`SNMP agent`|<p>SNMP OID: 1.3.6.1.4.1.23668.1735.2.9.2.0</p><p>Update Interval: 1m</p>|
|Trial license expired|<p>Trial license expired.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.340]</p>|
|Update error|<p>Application database update ended with an error.</p>|`SNMP trap`|<p>snmptrap[23668.1735.1.10]</p>|




## Triggers

|Name|Descriptionion|Expression|Severity|Dependencies and additional info|
|----|-----------|----------|--------|--------------------------------|
|KSMG antifishing DB obsoleted|<p>-</p>|<p>**Expression**:nodata(/zbx_ksmg_snmp/snmptrap[23668.1735.1.160],1s)=0</p><p>**Recovery expression**:nodata(/zbx_ksmg_snmp/snmptrap[23668.1735.1.160],120s)=1</p>|High|Allow manual close: true|
|KSMG Antifishing DB outdated|<p>-</p>|<p>**Expression**:nodata(/zbx_ksmg_snmp/snmptrap[23668.1735.1.150],1s)=0</p><p>**Recovery expression**:nodata(/zbx_ksmg_snmp/snmptrap[23668.1735.1.150],120s)=1</p>|Warning|Allow manual close: true|

<p>**Expression**: minSAD[{$URL}</p><p>**Recovery expression**: min</p>

## Feedback

Please report any issues with the template [here](https://github.com/Rayg00nchik/zabbix-one/issues/new?assignees=&labels=bug&template=BUG_REPORT.md&title=Issues%3A+Bug+Report)

You can give feedback [here](mailto:rayg00nchik@gmail.com)
