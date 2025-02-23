---
title: "Outbound Network Connection from Java Using Default Ports"
excerpt: "Exploit Public-Facing Application"
categories:
  - Endpoint
last_modified_at: 2021-12-13
toc: true
toc_label: ""
tags:
  - Exploit Public-Facing Application
  - Initial Access
  - Splunk Enterprise
  - Splunk Enterprise Security
  - Splunk Cloud
  - CVE-2021-44228
---



[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

A required step while exploiting the CVE-2021-44228-Log4j vulnerability is that the victim server will perform outbound connections to attacker-controlled infrastructure. This is required as part of the JNDI lookup as well as for retrieving the second stage .class payload. The following analytic identifies the Java process reaching out to default ports used by the LDAP and RMI protocols. This behavior could represent successfull exploitation. Note that adversaries can easily decide to use arbitrary ports for these protocols and potentially bypass this detection.

- **Type**: TTP
- **Product**: Splunk Enterprise, Splunk Enterprise Security, Splunk Cloud
- **Datamodel**: 
- **Last Updated**: 2021-12-13
- **Author**: Mauricio Velazco, Splunk
- **ID**: d2c14d28-5c47-11ec-9892-acde48001122


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access |

#### Search

```
 `sysmon` EventCode=3  (process_name=java OR process_name=java.exe) (DestinationPort=389 OR DestinationPort=1389 OR DestinationPort = 1099 ) 
| rename Computer as dest 
|  stats count min(_time) as firstTime max(_time) as lastTime by dest, process_name, DestinationPort 
| `security_content_ctime(firstTime)` 
| `outbound_network_connection_from_java_using_default_ports_filter`
```

#### Associated Analytic Story
* [Log4Shell CVE-2021-44228](/stories/log4shell_cve-2021-44228)


#### How To Implement
To successfully implement this search you need to be ingesting information on process that include the name of the process responsible for the changes from your endpoints into the `Endpoint` datamodel in the `Processes` node.

#### Required field
* _time
* process_name
* EventID
* CommandLine
* Computer
* DestinationPort
* DestinationIp


#### Kill Chain Phase
* Exploitation


#### Known False Positives
Legitimate Java applications may use perform outbound connections to these ports. Filter as needed


#### RBA

| Risk Score  | Impact      | Confidence   | Message      |
| ----------- | ----------- |--------------|--------------|
| 54.0 | 90 | 60 | Java performed outbound connections to default ports of LDAP or RMI on $dest$ |



#### CVE

| ID          | Summary | [CVSS](https://nvd.nist.gov/vuln-metrics/cvss) |
| ----------- | ----------- | -------------- |
| [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228) | Apache Log4j2 &lt;=2.14.1 JNDI features used in configuration, log messages, and parameters do not protect against attacker controlled LDAP and other JNDI related endpoints. An attacker who can control log messages or log message parameters can execute arbitrary code loaded from LDAP servers when message lookup substitution is enabled. From log4j 2.15.0, this behavior has been disabled by default. In previous releases (&gt;2.10) this behavior can be mitigated by setting system property &#34;log4j2.formatMsgNoLookups&#34; to “true” or it can be mitigated in prior releases (&lt;2.10) by removing the JndiLookup class from the classpath (example: zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class). | None |



#### Reference

* [https://www.lunasec.io/docs/blog/log4j-zero-day/](https://www.lunasec.io/docs/blog/log4j-zero-day/)
* [https://www.govcert.admin.ch/blog/zero-day-exploit-targeting-popular-java-library-log4j/](https://www.govcert.admin.ch/blog/zero-day-exploit-targeting-popular-java-library-log4j/)



#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)

* [https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1190/outbound_java/linux-sysmon.log](https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1190/outbound_java/linux-sysmon.log)



[*source*](https://github.com/splunk/security_content/tree/develop/detections/endpoint/outbound_network_connection_from_java_using_default_ports.yml) \| *version*: **1**