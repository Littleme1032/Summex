# Splunk Configuration

## [Sysmon (Splunk Universal Forwarder)](https://docs.splunk.com/Documentation/AddOns/released/MSSysmon/ConfigureSysmon)
```
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = false
renderXml = true
source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```

## [Event Logs](https://docs.splunk.com/Documentation/Splunk/9.0.1/Data/MonitorWindowseventlogdata)
```
[default]
_meta = hf_proxy::meta_test

[WinEventLog] 
_meta = hf_proxy::meta_test
host = WIN2K16_DC
index = wineventlog

[WinEventLog://Application]
disabled = 0
```

> input.config directory - ```$SPLUNK_HOME/etc/system/default/inputs```