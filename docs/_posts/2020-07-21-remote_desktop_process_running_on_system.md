---
title: "Remote Desktop Process Running On System"
excerpt: "Remote Desktop Protocol, Remote Services"
categories:
  - Endpoint
last_modified_at: 2020-07-21
toc: true
toc_label: ""
tags:
  - Remote Desktop Protocol
  - Lateral Movement
  - Remote Services
  - Lateral Movement
  - Splunk Enterprise
  - Splunk Enterprise Security
  - Splunk Cloud
  - Endpoint
---

### ⚠️ WARNING THIS IS A EXPERIMENTAL DETECTION
We have not been able to test, simulate or build datasets for it, use at your own risk!


[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

This search looks for the remote desktop process mstsc.exe running on systems upon which it doesn&#39;t typically run. This is accomplished by filtering out all systems that are noted in the `common_rdp_source category` in the Assets and Identity framework.

- **Type**: Hunting
- **Product**: Splunk Enterprise, Splunk Enterprise Security, Splunk Cloud
- **Datamodel**: [Endpoint](https://docs.splunk.com/Documentation/CIM/latest/User/Endpoint)
- **Last Updated**: 2020-07-21
- **Author**: David Dorsey, Splunk
- **ID**: f5939373-8054-40ad-8c64-cec478a22a4a


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Desktop Protocol | Lateral Movement |

| [T1021](https://attack.mitre.org/techniques/T1021/) | Remote Services | Lateral Movement |

#### Search

```

| tstats `security_content_summariesonly` count min(_time) as firstTime max(_time) as lastTime from datamodel=Endpoint.Processes where Processes.process=*mstsc.exe AND Processes.dest_category!=common_rdp_source by Processes.dest Processes.user Processes.process 
| `security_content_ctime(firstTime)`
| `security_content_ctime(lastTime)` 
| `drop_dm_object_name(Processes)` 
| `remote_desktop_process_running_on_system_filter` 
```

#### Associated Analytic Story
* [Hidden Cobra Malware](/stories/hidden_cobra_malware)
* [Active Directory Lateral Movement](/stories/active_directory_lateral_movement)


#### How To Implement
To successfully implement this search, you must be ingesting data that records process activity from your hosts to populate the endpoint data model in the processes node. The search requires you to identify systems that do not commonly use remote desktop. You can use the included support search &#34;Identify Systems Using Remote Desktop&#34; to identify these systems. After identifying them, you will need to add the &#34;common_rdp_source&#34; category to that system using the Enterprise Security Assets and Identities framework. This can be done by adding an entry in the assets.csv file located in `SA-IdentityManagement/lookups`.

#### Required field
* _time
* Processes.process
* Processes.dest_category
* Processes.dest
* Processes.user


#### Kill Chain Phase
* Actions on Objectives


#### Known False Positives
Remote Desktop may be used legitimately by users on the network.





#### Reference


#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)




[*source*](https://github.com/splunk/security_content/tree/develop/detections/experimental/endpoint/remote_desktop_process_running_on_system.yml) \| *version*: **5**