# Offensive Security / PenTesting Cheatsheets
A collection of cheatsheets, convenience functions, reminders and other useful snippets to aid your during your pentest engagement.
Disclaimer: I did not claim ownership of netcat and linux privilege escalation or reverse shell scripts.
The below is heavily inspired and based on https://github.com/dostoevskylabs/dostoevsky-pentest-notes.


## Reconnaissance / Enumeration

##### DNS lookups, Zone Transfers & Brute-Force
```bash
whois domain.com
dig {a|txt|ns|mx} domain.com
dig {a|txt|ns|mx} domain.com @ns1.domain.com
host -t {a|txt|ns|mx} megacorpone.com
host -a megacorpone.com
host -l megacorpone.com ns1.megacorpone.com
dnsrecon -d megacorpone.com -t axfr @ns2.megacorpone.com
dnsenum domain.com
nslookup -> set type=any -> ls -d domain.com
for sub in $(cat subdomains.txt);do host $sub.domain.com|grep "has.address";done
```

##### Banner Grabbing
```bash
nc -v $TARGET 80
telnet $TARGET 80
curl -vX $TARGET
```

##### Port Scanning with NetCat
```bash
nc -nvv -w 1 -z host 1000-2000
nc -nv -u -z -w 1 host 160-162
```

##### HTTP Brute-Force & Vulnerability Scanning
```bash
target=10.0.0.1; gobuster -u http://$target -r -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt -t 150 -l | tee /root/tools/$target/$target-gobuster
target=10.0.0.1; nikto -h http://$target:80 | tee $target/$target-nikto
target=10.0.0.1; wpscan --url http://$target:80 --enumerate u,t,p | tee $target/$target-wpscan-enum
```

##### RPC / NetBios / SMB
```bash
rpcinfo -p $TARGET
nbtscan $TARGET

#list shares
smbclient -L //$TARGET -U ""

# null session
rpcclient -v "" $TARGET
smbclient -L //$TARGET
```

##### SNMP
```bash

# Windows User Accounts
snmpwalk -c public -v1 $TARGET 1.3.6.1.4.1.77.1.2.25

# Windows Running Programs
snmpwalk -c public -v1 $TARGET 1.3.6.1.2.1.25.4.2.1.2

# Windows TCP Ports
snmpwalk -c public -v1 $TARGET4 1.3.6.1.2.1.6.13.1.3

# Software Name
snmpwalk -c public -v1 $TARGET 1.3.6.1.2.1.25.6.3.1.2

# brute-force community strings
onesixtyone -i snmp-ips.txt -c community.txt

snmp-check $TARGET
```

##### SMTP
```bash
smtp-user-enum -U /usr/share/wordlists/names.txt -t $TARGET -m 150
```

## Gaining Access



## Local Enumeration & Privilege Escalation

##### General File Search

```bash
# query the local db for a quick file find. Run updatedb before executing locate.
locate passwd 

# show which file would be executed in the current environment, depending on $PATH environment variable;
which nc wget curl php perl python netcat tftp telnet ftp

# search for *.conf (case-insensitive) files recursively starting with /etc;
find /etc -iname *.conf
```

