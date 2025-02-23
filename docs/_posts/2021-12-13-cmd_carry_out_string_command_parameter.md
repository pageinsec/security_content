---
title: "CMD Carry Out String Command Parameter"
excerpt: "Windows Command Shell, Command and Scripting Interpreter"
categories:
  - Endpoint
last_modified_at: 2021-12-13
toc: true
toc_label: ""
tags:
  - Windows Command Shell
  - Execution
  - Command and Scripting Interpreter
  - Execution
  - Splunk Enterprise
  - Splunk Enterprise Security
  - Splunk Cloud
  - CVE-2021-44228
  - Endpoint
---



[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

The following analytic identifies command-line arguments where `cmd.exe /c` is used to execute a program. `cmd /c` is used to run commands in MS-DOS and terminate after command or process completion. This technique is commonly seen in adversaries and malware to execute batch command using different shell like PowerShell or different process other than `cmd.exe`. This is a good hunting query for suspicious command-line made by a script or relative process execute it.

- **Type**: Hunting
- **Product**: Splunk Enterprise, Splunk Enterprise Security, Splunk Cloud
- **Datamodel**: [Endpoint](https://docs.splunk.com/Documentation/CIM/latest/User/Endpoint)
- **Last Updated**: 2021-12-13
- **Author**: Teoderick Contreras, Bhavin Patel, Splunk
- **ID**: 54a6ed00-3256-11ec-b031-acde48001122


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1059.003](https://attack.mitre.org/techniques/T1059/003/) | Windows Command Shell | Execution |

| [T1059](https://attack.mitre.org/techniques/T1059/) | Command and Scripting Interpreter | Execution |

#### Search

```

| tstats `security_content_summariesonly` min(_time) as firstTime max(_time) as lastTime from datamodel=Endpoint.Processes where `process_cmd` AND Processes.process="* /c *" by Processes.dest Processes.user Processes.parent_process Processes.process_name Processes.original_file_name Processes.process Processes.process_id Processes.parent_process_id 
| `drop_dm_object_name(Processes)` 
| `security_content_ctime(firstTime)` 
| `security_content_ctime(lastTime)` 
| `cmd_carry_out_string_command_parameter_filter`
```

#### Associated Analytic Story
* [IcedID](/stories/icedid)
* [Log4Shell CVE-2021-44228](/stories/log4shell_cve-2021-44228)


#### How To Implement
To successfully implement this search you need to be ingesting information on process that include the name of the process responsible for the changes from your endpoints into the `Endpoint` datamodel in the `Processes` node. In addition, confirm the latest CIM App 4.20 or higher is installed and the latest TA for the endpoint product.

#### Required field
* _time
* Processes.parent_process_name
* Processes.parent_process
* Processes.process_name
* Processes.process_id
* Processes.process
* Processes.dest
* Processes.user
* Processes.process_id
* Processes.parent_process_id


#### Kill Chain Phase
* Exploitation


#### Known False Positives
False positives may be high based on legitimate scripted code in any environment. Filter as needed.


#### RBA

| Risk Score  | Impact      | Confidence   | Message      |
| ----------- | ----------- |--------------|--------------|
| 30.0 | 60 | 50 | An instance of $parent_process_name$ spawning $process_name$ was identified on endpoint $dest$ by user $user$ attempting spawn a new process. |



#### CVE

| ID          | Summary | [CVSS](https://nvd.nist.gov/vuln-metrics/cvss) |
| ----------- | ----------- | -------------- |
| [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228) | Apache Log4j2 &lt;=2.14.1 JNDI features used in configuration, log messages, and parameters do not protect against attacker controlled LDAP and other JNDI related endpoints. An attacker who can control log messages or log message parameters can execute arbitrary code loaded from LDAP servers when message lookup substitution is enabled. From log4j 2.15.0, this behavior has been disabled by default. In previous releases (&gt;2.10) this behavior can be mitigated by setting system property &#34;log4j2.formatMsgNoLookups&#34; to “true” or it can be mitigated in prior releases (&lt;2.10) by removing the JndiLookup class from the classpath (example: zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class). | None |



#### Reference

* [https://thedfirreport.com/2021/10/18/icedid-to-xinglocker-ransomware-in-24-hours/](https://thedfirreport.com/2021/10/18/icedid-to-xinglocker-ransomware-in-24-hours/)



#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)

* [https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/malware/icedid/cmd_carry_str_param/sysmon.log](https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/malware/icedid/cmd_carry_str_param/sysmon.log)



[*source*](https://github.com/splunk/security_content/tree/develop/detections/endpoint/cmd_carry_out_string_command_parameter.yml) \| *version*: **2**