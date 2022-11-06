# Splunk Cheatsheet
- [Splunk Quick Reference Guide](https://www.innovato.com/splunk/RefCard.pdf)
- [Detection Searches](https://docs.splunksecurityessentials.com/content-detail/)

## Reconnaissance
### [SPL for Network Scanning](https://docs.splunksecurityessentials.com/content-detail/basic_scanning/)
Looks for hosts that reach out to more than 500 hosts, or more than 500 ports in a short period of time, indicating scanning.
```
| bucket _time span=1h 
| stats dc(dest_port) as num_dest_port dc(dest_ip) as num_dest_ip by src_ip, _time 
| where num_dest_port > 1000 OR num_dest_ip > 1000
```

## Initial Accesss / Credential Access
### [SPL for Bruteforcing](https://docs.splunksecurityessentials.com/content-detail/basic_brute_force/)
Uses a simple threshold for Windows Security Logs to alert if there are a large number of failed logins, and at least one successful login from the same source.
```
| bucket _time span=1h 
| stats count(eval(action="success")) as successes count(eval(action="failure")) as failures by src _time 
| where successes>0 AND failures>100
```

## Exfiltration / C2
### [SPL for Basic Dynamic DNS Detection](https://docs.splunksecurityessentials.com/content-detail/sse_dyndns/)
Detect outbound communication to Dynamic DNS servers, which are frequently leveraged for command and control by all types of attackers.
```
| eval list="mozilla" | `ut_parse_extended(uri,list)`
| lookup dynamic_dns_lookup domain as ut_domain OUTPUT inlist
| search inlist=true 
| table _time ut_domain inlist bytes* uri
```

### [Basic TOR Traffic Detection](https://docs.splunksecurityessentials.com/content-detail/sser_tor_traffic/)
The anonymity of TOR makes it the perfect place to hide C&C, exfiltration, or ransomware payment via bitcoin. This example looks for ransomware activity based on FW logs.
```
| search app=tor src_ip=* 
| table _time src_ip src_port dest_ip dest_port bytes app
```