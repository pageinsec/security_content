---
title: "Any Powershell DownloadFile"
excerpt: "Command and Scripting Interpreter, PowerShell"
categories:
  - Endpoint
last_modified_at: 2021-03-01
toc: true
toc_label: ""
tags:
  - Command and Scripting Interpreter
  - Execution
  - PowerShell
  - Execution
  - Splunk Enterprise
  - Splunk Enterprise Security
  - Splunk Cloud
  - CVE-2021-44228
  - Endpoint
---



[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

The following analytic identifies the use of PowerShell downloading a file using `DownloadFile` method. This particular method is utilized in many different PowerShell frameworks to download files and output to disk. Identify the source (IP/domain) and destination file and triage appropriately. If AMSI logging or PowerShell transaction logs are available, review for further details of the implant.

- **Type**: TTP
- **Product**: Splunk Enterprise, Splunk Enterprise Security, Splunk Cloud
- **Datamodel**: [Endpoint](https://docs.splunk.com/Documentation/CIM/latest/User/Endpoint)
- **Last Updated**: 2021-03-01
- **Author**: Michael Haag, Splunk
- **ID**: 1a93b7ea-7af7-11eb-adb5-acde48001122


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1059](https://attack.mitre.org/techniques/T1059/) | Command and Scripting Interpreter | Execution |

| [T1059.001](https://attack.mitre.org/techniques/T1059/001/) | PowerShell | Execution |

#### Search

```

| tstats `security_content_summariesonly` count min(_time) as firstTime max(_time) as lastTime from datamodel=Endpoint.Processes where `process_powershell` Processes.process=*DownloadFile* by Processes.dest Processes.user Processes.parent_process Processes.process_name Processes.original_file_name Processes.process Processes.process_id Processes.parent_process_id 
| `drop_dm_object_name(Processes)` 
| `security_content_ctime(firstTime)`
| `security_content_ctime(lastTime)`
| `any_powershell_downloadfile_filter`
```

#### Associated Analytic Story
* [Malicious PowerShell](/stories/malicious_powershell)
* [Ingress Tool Transfer](/stories/ingress_tool_transfer)
* [Log4Shell CVE-2021-44228](/stories/log4shell_cve-2021-44228)


#### How To Implement
To successfully implement this search you need to be ingesting information on process that include the name of the process responsible for the changes from your endpoints into the `Endpoint` datamodel in the `Processes` node. In addition, confirm the latest CIM App 4.20 or higher is installed and the latest TA for the endpoint product.

#### Required field
* _time
* Processes.dest
* Processes.user
* Processes.parent_process_name
* Processes.parent_process
* Processes.original_file_name
* Processes.process_name
* Processes.process
* Processes.process_id
* Processes.parent_process_path
* Processes.process_path
* Processes.parent_process_id


#### Kill Chain Phase
* Exploitation


#### Known False Positives
False positives may be present and filtering will need to occur by parent process or command line argument. It may be required to modify this query to an EDR product for more granular coverage.


#### RBA

| Risk Score  | Impact      | Confidence   | Message      |
| ----------- | ----------- |--------------|--------------|
| 56.0 | 80 | 70 | An instance of $parent_process_name$ spawning $process_name$ was identified on endpoint $dest$ by user $user$. This behavior identifies the use of DownloadFile within PowerShell. |



#### CVE

| ID          | Summary | [CVSS](https://nvd.nist.gov/vuln-metrics/cvss) |
| ----------- | ----------- | -------------- |
| [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228) | Apache Log4j2 &lt;=2.14.1 JNDI features used in configuration, log messages, and parameters do not protect against attacker controlled LDAP and other JNDI related endpoints. An attacker who can control log messages or log message parameters can execute arbitrary code loaded from LDAP servers when message lookup substitution is enabled. From log4j 2.15.0, this behavior has been disabled by default. In previous releases (&gt;2.10) this behavior can be mitigated by setting system property &#34;log4j2.formatMsgNoLookups&#34; to “true” or it can be mitigated in prior releases (&lt;2.10) by removing the JndiLookup class from the classpath (example: zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class). | None |



#### Reference

* [https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfile?view=net-5.0](https://docs.microsoft.com/en-us/dotnet/api/system.net.webclient.downloadfile?view=net-5.0)
* [https://blog.malwarebytes.com/malwarebytes-news/2021/02/lazyscripter-from-empire-to-double-rat/](https://blog.malwarebytes.com/malwarebytes-news/2021/02/lazyscripter-from-empire-to-double-rat/)
* [https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md)



#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)

* [https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1059.001/atomic_red_team/windows-sysmon.log](https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1059.001/atomic_red_team/windows-sysmon.log)



[*source*](https://github.com/splunk/security_content/tree/develop/detections/endpoint/any_powershell_downloadfile.yml) \| *version*: **2**