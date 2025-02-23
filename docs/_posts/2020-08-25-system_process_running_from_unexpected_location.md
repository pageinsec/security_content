---
title: "System Process Running from Unexpected Location"
excerpt: "Masquerading"
categories:
  - Endpoint
last_modified_at: 2020-08-25
toc: true
toc_label: ""
tags:
  - Masquerading
  - Defense Evasion
  - Splunk Behavioral Analytics
  - Endpoint_Processes
---



[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

An attacker tries might try to use different version of a system command without overriding original, or they might try to avoid some detection running the process from a different folder. This detection checks that a list of system processes run inside C:\\Windows\System32 or C:\\Windows\SysWOW64 The list of system processes has been extracted from https://github.com/splunk/security_content/blob/develop/lookups/is_windows_system_file.csv and the original detection https://github.com/splunk/security_content/blob/develop/detections/system_processes_run_from_unexpected_locations.yml

- **Type**: Anomaly
- **Product**: Splunk Behavioral Analytics
- **Datamodel**: [Endpoint_Processes](https://docs.splunk.com/Documentation/CIM/latest/User/EndpointProcesses)
- **Last Updated**: 2020-08-25
- **Author**: Ignacio Bermudez Corrales, Splunk
- **ID**: 28179107-099a-464a-94d3-08301e6c055f


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1036](https://attack.mitre.org/techniques/T1036/) | Masquerading | Defense Evasion |

#### Search

```
 $ssa_input = 
| from read_ssa_enriched_events() 
| eval device=ucast(map_get(input_event, "dest_device_id"), "string", null), user=ucast(map_get(input_event, "dest_user_id"), "string", null), timestamp=parse_long(ucast(map_get(input_event, "_time"), "string", null)), process_name=lower(ucast(map_get(input_event, "process_name"), "string", null)), process_path=lower(ucast(map_get(input_event, "process_path"), "string", null)), event_id=ucast(map_get(input_event, "event_id"), "string", null);
$cond_1 = 
| from $ssa_input 
| where process_name="arp.exe" OR process_name="adaptertroubleshooter.exe" OR process_name="applicationframehost.exe" OR process_name="atbroker.exe" OR process_name="authhost.exe" OR process_name="autoworkplace.exe" OR process_name="axinstui.exe" OR process_name="backgroundtransferhost.exe" OR process_name="bdehdcfg.exe" OR process_name="bdeuisrv.exe" OR process_name="bdeunlockwizard.exe" OR process_name="bitlockerdeviceencryption.exe" OR process_name="bitlockerwizard.exe" OR process_name="bitlockerwizardelev.exe" OR process_name="bytecodegenerator.exe" OR process_name="camerasettingsuihost.exe" OR process_name="castsrv.exe" OR process_name="certenrollctrl.exe" OR process_name="checknetisolation.exe" OR process_name="clipup.exe" OR process_name="cloudexperiencehostbroker.exe" OR process_name="cloudnotifications.exe" OR process_name="cloudstoragewizard.exe" OR process_name="compmgmtlauncher.exe" OR process_name="compattelrunner.exe" OR process_name="computerdefaults.exe" OR process_name="credentialuibroker.exe" OR process_name="dfdwiz.exe" OR process_name="dwwin.exe" OR process_name="dataexchangehost.exe" OR process_name="defrag.exe" OR process_name="devicedisplayobjectprovider.exe" OR process_name="deviceeject.exe" OR process_name="deviceenroller.exe" OR process_name="devicepairingwizard.exe" OR process_name="deviceproperties.exe" OR process_name="disksnapshot.exe" OR process_name="dism.exe" OR process_name="displayswitch.exe" OR process_name="dmnotificationbroker.exe" OR process_name="dmomacpmo.exe" OR process_name="dpiscaling.exe" OR process_name="dsmusertask.exe" OR process_name="dxpserver.exe" OR process_name="edpcleanup.exe" OR process_name="eosnotify.exe" OR process_name="eap3host.exe" OR process_name="easpoliciesbrokerhost.exe" OR process_name="easeofaccessdialog.exe" OR process_name="ehstorauthn.exe" OR process_name="fxscover.exe" OR process_name="fxssvc.exe" OR process_name="fxsunatd.exe" OR process_name="filehistory.exe" OR process_name="fondue.exe" OR process_name="gamepanel.exe" OR process_name="genvalobj.exe" OR process_name="gettingstarted.exe" OR process_name="hostname.exe" OR process_name="icsentitlementhost.exe" OR process_name="infdefaultinstall.exe" OR process_name="installagent.exe" OR process_name="languagecomponentsinstallercomhandler.exe" OR process_name="launchtm.exe" OR process_name="launchwinapp.exe" OR process_name="legacynetuxhost.exe" OR process_name="licensemanagershellext.exe" OR process_name="licensingui.exe" OR process_name="locationnotificationwindows.exe" OR process_name="locationnotifications.exe" OR process_name="locator.exe" OR process_name="lockapphost.exe" OR process_name="lockscreencontentserver.exe" OR process_name="logonui.exe" OR process_name="lsaiso.exe" OR process_name="mdeserver.exe" OR process_name="mdmagent.exe" OR process_name="mdmappinstaller.exe" OR process_name="mrinfo.exe" OR process_name="mrt.exe" OR process_name="mschedexe.exe" OR process_name="magnify.exe" OR process_name="mbaeparsertask.exe" OR process_name="mdres.exe" OR process_name="mdsched.exe" OR process_name="migautoplay.exe" OR process_name="mpsigstub.exe" OR process_name="msspellcheckinghost.exe" OR process_name="muiunattend.exe" OR process_name="multidigimon.exe" OR process_name="musnotification.exe" OR process_name="musnotificationux.exe" OR process_name="napstat.exe" OR process_name="netstat.exe" OR process_name="narrator.exe" OR process_name="netcfgnotifyobjecthost.exe" OR process_name="netevtfwdr.exe" OR process_name="netproj.exe" OR process_name="netplwiz.exe" OR process_name="networkuxbroker.exe";
$cond_2 = 
| from $ssa_input 
| where process_name="openwith.exe" OR process_name="optionalfeatures.exe" OR process_name="pathping.exe" OR process_name="ping.exe" OR process_name="passwordonwakesettingflyout.exe" OR process_name="pickerhost.exe" OR process_name="pkgmgr.exe" OR process_name="pnpunattend.exe" OR process_name="pnputil.exe" OR process_name="presentationhost.exe" OR process_name="presentationsettings.exe" OR process_name="printbrmui.exe" OR process_name="printdialoghost.exe" OR process_name="printdialoghost3d.exe" OR process_name="printisolationhost.exe" OR process_name="proximityuxhost.exe" OR process_name="rdspnf.exe" OR process_name="rmactivate.exe" OR process_name="rmactivate_isv.exe" OR process_name="rmactivate_ssp.exe" OR process_name="rmactivate_ssp_isv.exe" OR process_name="route.exe" OR process_name="rdpsa.exe" OR process_name="rdpsaproxy.exe" OR process_name="rdpsauachelper.exe" OR process_name="reagentc.exe" OR process_name="recoverydrive.exe" OR process_name="register-cimprovider.exe" OR process_name="registeriepkeys.exe" OR process_name="relpost.exe" OR process_name="remoteposworker.exe" OR process_name="rmclient.exe" OR process_name="robocopy.exe" OR process_name="rpcping.exe" OR process_name="runlegacycplelevated.exe" OR process_name="runtimebroker.exe" OR process_name="sihclient.exe" OR process_name="searchfilterhost.exe" OR process_name="searchindexer.exe" OR process_name="searchprotocolhost.exe" OR process_name="secedit.exe" OR process_name="sensordataservice.exe" OR process_name="setieinstalleddate.exe" OR process_name="settingsynchost.exe" OR process_name="slidetoshutdown.exe" OR process_name="smartscreensettings.exe" OR process_name="sndvol.exe" OR process_name="snippingtool.exe" OR process_name="soundrecorder.exe" OR process_name="spaceagent.exe" OR process_name="sppextcomobj.exe" OR process_name="srtasks.exe" OR process_name="stikynot.exe" OR process_name="synchost.exe" OR process_name="sysreseterr.exe" OR process_name="systempropertiesadvanced.exe" OR process_name="systempropertiescomputername.exe" OR process_name="systempropertiesdataexecutionprevention.exe" OR process_name="systempropertieshardware.exe" OR process_name="systempropertiesperformance.exe" OR process_name="systempropertiesprotection.exe" OR process_name="systempropertiesremote.exe" OR process_name="systemsettingsadminflows.exe" OR process_name="systemsettingsbroker.exe" OR process_name="systemsettingsremovedevice.exe" OR process_name="tcpsvcs.exe" OR process_name="tracert.exe" OR process_name="tstheme.exe" OR process_name="tswbprxy.exe" OR process_name="tapiunattend.exe" OR process_name="taskmgr.exe" OR process_name="thumbnailextractionhost.exe" OR process_name="tokenbrokercookies.exe" OR process_name="tpminit.exe" OR process_name="tswpfwrp.exe" OR process_name="ui0detect.exe" OR process_name="upgraderesultsui.exe" OR process_name="useraccountbroker.exe" OR process_name="useraccountcontrolsettings.exe" OR process_name="usoclient.exe" OR process_name="utilman.exe" OR process_name="vssvc.exe" OR process_name="vaultcmd.exe" OR process_name="vaultsysui.exe" OR process_name="wfs.exe" OR process_name="wmpdmc.exe" OR process_name="wpdshextautoplay.exe" OR process_name="wscollect.exe" OR process_name="wsmanhttpconfig.exe" OR process_name="wsreset.exe" OR process_name="wudfhost.exe" OR process_name="wwahost.exe" OR process_name="wallpaperhost.exe" OR process_name="webcache.exe" OR process_name="werfault.exe" OR process_name="werfaultsecure.exe" OR process_name="winsat.exe" OR process_name="windows.media.backgroundplayback.exe" OR process_name="windowsactiondialog.exe" OR process_name="windowsanytimeupgrade.exe" OR process_name="windowsanytimeupgraderesults.exe";
$cond_3 = 
| from $ssa_input 
| where process_name="windowsanytimeupgradeui.exe" OR process_name="windowsupdateelevatedinstaller.exe" OR process_name="workfolders.exe" OR process_name="wpcmon.exe" OR process_name="acu.exe" OR process_name="aitagent.exe" OR process_name="aitstatic.exe" OR process_name="alg.exe" OR process_name="appidcertstorecheck.exe" OR process_name="appidpolicyconverter.exe" OR process_name="at.exe" OR process_name="attrib.exe" OR process_name="audiodg.exe" OR process_name="auditpol.exe" OR process_name="autochk.exe" OR process_name="autoconv.exe" OR process_name="autofmt.exe" OR process_name="baaupdate.exe" OR process_name="backgroundtaskhost.exe" OR process_name="bcastdvr.exe" OR process_name="bcdboot.exe" OR process_name="bcdedit.exe" OR process_name="bdechangepin.exe" OR process_name="bdeunlock.exe" OR process_name="bitsadmin.exe" OR process_name="bootcfg.exe" OR process_name="bootim.exe" OR process_name="bootsect.exe" OR process_name="bridgeunattend.exe" OR process_name="browser_broker.exe" OR process_name="bthudtask.exe" OR process_name="cacls.exe" OR process_name="calc.exe" OR process_name="cdpreference.exe" OR process_name="certreq.exe" OR process_name="certutil.exe" OR process_name="change.exe" OR process_name="changepk.exe" OR process_name="charmap.exe" OR process_name="chglogon.exe" OR process_name="chgport.exe" OR process_name="chgusr.exe" OR process_name="chkdsk.exe" OR process_name="chkntfs.exe" OR process_name="choice.exe" OR process_name="cipher.exe" OR process_name="cleanmgr.exe" OR process_name="cliconfg.exe" OR process_name="clip.exe" OR process_name="cmd.exe" OR process_name="cmdkey.exe" OR process_name="cmdl32.exe" OR process_name="cmmon32.exe" OR process_name="cmstp.exe" OR process_name="cofire.exe" OR process_name="colorcpl.exe" OR process_name="comp.exe" OR process_name="compact.exe" OR process_name="conhost.exe" OR process_name="consent.exe" OR process_name="control.exe" OR process_name="convert.exe" OR process_name="credwiz.exe" OR process_name="cscript.exe" OR process_name="csrss.exe" OR process_name="ctfmon.exe" OR process_name="cttune.exe" OR process_name="cttunesvr.exe" OR process_name="dashost.exe" OR process_name="dccw.exe" OR process_name="dcomcnfg.exe" OR process_name="ddodiag.exe" OR process_name="dfrgui.exe" OR process_name="dialer.exe" OR process_name="diantz.exe" OR process_name="dinotify.exe" OR process_name="diskpart.exe" OR process_name="diskperf.exe" OR process_name="diskraid.exe" OR process_name="dispdiag.exe" OR process_name="djoin.exe" OR process_name="dllhost.exe" OR process_name="dllhst3g.exe" OR process_name="dmcertinst.exe" OR process_name="dmcfghost.exe" OR process_name="dmclient.exe" OR process_name="dnscacheugc.exe" OR process_name="doskey.exe" OR process_name="dpapimig.exe" OR process_name="dpnsvr.exe" OR process_name="driverquery.exe" OR process_name="drvcfg.exe" OR process_name="drvinst.exe" OR process_name="dsregcmd.exe" OR process_name="dstokenclean.exe" OR process_name="dvdplay.exe" OR process_name="dvdupgrd.exe" OR process_name="dwm.exe" OR process_name="dxdiag.exe" OR process_name="easinvoker.exe" OR process_name="efsui.exe";
$cond_4 = 
| from $ssa_input 
| where process_name="embeddedapplauncher.exe" OR process_name="esentutl.exe" OR process_name="eudcedit.exe" OR process_name="eventcreate.exe" OR process_name="eventvwr.exe" OR process_name="expand.exe" OR process_name="extrac32.exe" OR process_name="fc.exe" OR process_name="fhmanagew.exe" OR process_name="find.exe" OR process_name="findstr.exe" OR process_name="finger.exe" OR process_name="fixmapi.exe" OR process_name="fltmc.exe" OR process_name="fodhelper.exe" OR process_name="fontdrvhost.exe" OR process_name="fontview.exe" OR process_name="forfiles.exe" OR process_name="fsavailux.exe" OR process_name="fsquirt.exe" OR process_name="fsutil.exe" OR process_name="ftp.exe" OR process_name="fvenotify.exe" OR process_name="fveprompt.exe" OR process_name="getmac.exe" OR process_name="gpresult.exe" OR process_name="gpscript.exe" OR process_name="gpupdate.exe" OR process_name="grpconv.exe" OR process_name="hdwwiz.exe" OR process_name="help.exe" OR process_name="hwrcomp.exe" OR process_name="hwrreg.exe" OR process_name="icacls.exe" OR process_name="icardagt.exe" OR process_name="icsunattend.exe" OR process_name="ie4uinit.exe" OR process_name="ieunatt.exe" OR process_name="ieetwcollector.exe" OR process_name="iexpress.exe" OR process_name="immersivetpmvscmgrsvr.exe" OR process_name="ipconfig.exe" OR process_name="irftp.exe" OR process_name="iscsicli.exe" OR process_name="iscsicpl.exe" OR process_name="isoburn.exe" OR process_name="klist.exe" OR process_name="ksetup.exe" OR process_name="ktmutil.exe" OR process_name="label.exe" OR process_name="licensingdiag.exe" OR process_name="lodctr.exe" OR process_name="logagent.exe" OR process_name="logman.exe" OR process_name="logoff.exe" OR process_name="lpkinstall.exe" OR process_name="lpksetup.exe" OR process_name="lpremove.exe" OR process_name="lsass.exe" OR process_name="lsm.exe" OR process_name="makecab.exe" OR process_name="manage-bde.exe" OR process_name="mblctr.exe" OR process_name="mcbuilder.exe" OR process_name="mctadmin.exe" OR process_name="mfpmp.exe" OR process_name="mmc.exe" OR process_name="mobsync.exe" OR process_name="mountvol.exe" OR process_name="mpnotify.exe" OR process_name="msconfig.exe" OR process_name="msdt.exe" OR process_name="msdtc.exe" OR process_name="msfeedssync.exe" OR process_name="msg.exe" OR process_name="mshta.exe" OR process_name="msiexec.exe" OR process_name="msinfo32.exe" OR process_name="mspaint.exe" OR process_name="msra.exe" OR process_name="mstsc.exe" OR process_name="mtstocom.exe" OR process_name="nbtstat.exe" OR process_name="ndadmin.exe" OR process_name="net.exe" OR process_name="net1.exe" OR process_name="netbtugc.exe" OR process_name="netcfg.exe" OR process_name="netiougc.exe" OR process_name="netsh.exe" OR process_name="newdev.exe" OR process_name="nltest.exe" OR process_name="notepad.exe" OR process_name="nslookup.exe" OR process_name="ntoskrnl.exe" OR process_name="ntprint.exe" OR process_name="ocsetup.exe" OR process_name="odbcad32.exe" OR process_name="odbcconf.exe" OR process_name="omadmclient.exe" OR process_name="omadmprc.exe";
$cond_5 = 
| from $ssa_input 
| where process_name="openfiles.exe" OR process_name="osk.exe" OR process_name="p2phost.exe" OR process_name="pcalua.exe" OR process_name="pcaui.exe" OR process_name="pcawrk.exe" OR process_name="pcwrun.exe" OR process_name="perfmon.exe" OR process_name="phoneactivate.exe" OR process_name="plasrv.exe" OR process_name="poqexec.exe" OR process_name="powercfg.exe" OR process_name="prevhost.exe" OR process_name="print.exe" OR process_name="printfilterpipelinesvc.exe" OR process_name="printui.exe" OR process_name="proquota.exe" OR process_name="provtool.exe" OR process_name="psr.exe" OR process_name="pwlauncher.exe" OR process_name="qappsrv.exe" OR process_name="qprocess.exe" OR process_name="query.exe" OR process_name="quser.exe" OR process_name="qwinsta.exe" OR process_name="rasautou.exe" OR process_name="rasdial.exe" OR process_name="raserver.exe" OR process_name="rasphone.exe" OR process_name="rdpclip.exe" OR process_name="rdpinput.exe" OR process_name="rdrleakdiag.exe" OR process_name="recdisc.exe" OR process_name="recover.exe" OR process_name="reg.exe" OR process_name="regedt32.exe" OR process_name="regini.exe" OR process_name="regsvr32.exe" OR process_name="rekeywiz.exe" OR process_name="relog.exe" OR process_name="repair-bde.exe" OR process_name="replace.exe" OR process_name="reset.exe" OR process_name="resmon.exe" OR process_name="rmttpmvscmgrsvr.exe" OR process_name="rrinstaller.exe" OR process_name="rstrui.exe" OR process_name="runas.exe" OR process_name="rundll32.exe" OR process_name="runonce.exe" OR process_name="rwinsta.exe" OR process_name="sbunattend.exe" OR process_name="sc.exe" OR process_name="schtasks.exe" OR process_name="sdbinst.exe" OR process_name="sdchange.exe" OR process_name="sdclt.exe" OR process_name="sdiagnhost.exe" OR process_name="secinit.exe" OR process_name="services.exe" OR process_name="sessionmsg.exe" OR process_name="sethc.exe" OR process_name="setspn.exe" OR process_name="setupcl.exe" OR process_name="setupugc.exe" OR process_name="setx.exe" OR process_name="sfc.exe" OR process_name="shadow.exe" OR process_name="shrpubw.exe" OR process_name="shutdown.exe" OR process_name="sigverif.exe" OR process_name="sihost.exe" OR process_name="slui.exe" OR process_name="smss.exe" OR process_name="snmptrap.exe" OR process_name="sort.exe" OR process_name="spinstall.exe" OR process_name="spoolsv.exe" OR process_name="sppsvc.exe" OR process_name="spreview.exe" OR process_name="srdelayed.exe" OR process_name="subst.exe" OR process_name="svchost.exe" OR process_name="sxstrace.exe" OR process_name="syskey.exe" OR process_name="systeminfo.exe" OR process_name="systemreset.exe" OR process_name="systray.exe" OR process_name="tabcal.exe" OR process_name="takeown.exe" OR process_name="taskeng.exe" OR process_name="taskhost.exe" OR process_name="taskhostw.exe" OR process_name="taskkill.exe" OR process_name="tasklist.exe" OR process_name="taskmgr.exe" OR process_name="tcmsetup.exe" OR process_name="timeout.exe" OR process_name="tpmvscmgr.exe" OR process_name="tpmvscmgrsvr.exe";
$cond_6 = 
| from $ssa_input 
| where process_name="tracerpt.exe" OR process_name="tscon.exe" OR process_name="tsdiscon.exe" OR process_name="tskill.exe" OR process_name="typeperf.exe" OR process_name="tzsync.exe" OR process_name="tzutil.exe" OR process_name="ucsvc.exe" OR process_name="unlodctr.exe" OR process_name="unregmp2.exe" OR process_name="upnpcont.exe" OR process_name="userinit.exe" OR process_name="vds.exe" OR process_name="vdsldr.exe" OR process_name="verclsid.exe" OR process_name="verifier.exe" OR process_name="verifiergui.exe" OR process_name="vmicsvc.exe" OR process_name="vssadmin.exe" OR process_name="w32tm.exe" OR process_name="waitfor.exe" OR process_name="wbadmin.exe" OR process_name="wbengine.exe" OR process_name="wecutil.exe" OR process_name="wermgr.exe" OR process_name="wevtutil.exe" OR process_name="wextract.exe" OR process_name="where.exe" OR process_name="whoami.exe" OR process_name="wiaacmgr.exe" OR process_name="wiawow64.exe" OR process_name="wifitask.exe" OR process_name="wimserv.exe" OR process_name="wininit.exe" OR process_name="winload.exe" OR process_name="winlogon.exe" OR process_name="winresume.exe" OR process_name="winrs.exe" OR process_name="winrshost.exe" OR process_name="winver.exe" OR process_name="wisptis.exe" OR process_name="wkspbroker.exe" OR process_name="wksprt.exe" OR process_name="wlanext.exe" OR process_name="wlrmdr.exe" OR process_name="wowreg32.exe" OR process_name="wpnpinst.exe" OR process_name="wpr.exe" OR process_name="write.exe" OR process_name="wscript.exe" OR process_name="wsmprovhost.exe" OR process_name="wsqmcons.exe" OR process_name="wuapihost.exe" OR process_name="wuapp.exe" OR process_name="wuauclt.exe" OR process_name="wusa.exe" OR process_name="xcopy.exe" OR process_name="xpsrchvw.exe" OR process_name="xwizard.exe";

| from $cond_1 
| union $cond_2 
| union $cond_3 
| union $cond_4 
| union $cond_5 
| union $cond_6 
| where match_regex(process_path, /(?i)\\windows\\system32/)=false AND match_regex(process_path, /(?i)\\windows\\syswow64/)=false 
| eval start_time=timestamp, end_time=timestamp, entities=mvappend(device, user), body=create_map(["event_id", event_id, "process_path", process_path, "process_name", process_name]) 
| into write_ssa_detected_events();
```

#### Associated Analytic Story
* [Windows Defense Evasion Tactics](/stories/windows_defense_evasion_tactics)
* [Masquerading - Rename System Utilities](/stories/masquerading_-_rename_system_utilities)


#### How To Implement
Collect endpoint data such as sysmon or 4688 events.

#### Required field
* dest_device_id
* process_name
* _time
* dest_user_id
* process_path


#### Kill Chain Phase
* Actions on Objectives


#### Known False Positives
None


#### RBA

| Risk Score  | Impact      | Confidence   | Message      |
| ----------- | ----------- |--------------|--------------|
| 56.0 | 70 | 80 | A system process $process_name$ with commandline $cmd_line$ spawn in non-default folder path in host $dest_device_id$ |




#### Reference


#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)

* [https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1036/system_process_running_unexpected_location/windows-security.log](https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1036/system_process_running_unexpected_location/windows-security.log)



[*source*](https://github.com/splunk/security_content/tree/develop/detections/endpoint/system_process_running_from_unexpected_location.yml) \| *version*: **3**