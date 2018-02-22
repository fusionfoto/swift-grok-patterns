# Idea
We leverage rsyslog to send, convert swift log to json fomat and forward to logstash.

## Implementation

### For Rsyslog Receiver ( elk or logstash server )
 * Update elk5 ( rsyslog receiver ) in rsyslog.conf
```
sudo vi /etc/rsyslog.conf
```

 * Change from
```
# provides UDP syslog reception
#$ModLoad imudp
#$UDPServerRun 514
# provides TCP syslog reception
#$ModLoad imtcp
#$InputTCPServerRun 514
```
 * To
```
# provides UDP syslog reception
$ModLoad imudp
$UDPServerRun 514
# provides TCP syslog reception
$ModLoad imtcp
$InputTCPServerRun 514
```

 * copy three * conf to /etc/rsyslog.d/
```
-rw-r--r-- 1 swiftstack swiftstack  706 Feb 27 21:06 01-json-template.conf
-rw-r--r-- 1 swiftstack swiftstack 1655 Dec  2  2015 50-default.conf
-rw-r--r-- 1 swiftstack swiftstack  167 Feb 27 16:22 60-output.conf
```

 * restart rsyslog
```
service rsyslog restar
```

### For Rsyslog Sender ( Swift Node )
 * Add /etc/rsyslog.d/0-swift.conf
```
# NOTE: we used to enable UDP logging here via @,
#       if you prefer TCP logging, then use @@

$imjournalRatelimitInterval 0
$imjournalRatelimitBurst 0

*.*                         @<your elk host or ip address>:514

# Turn off an rsyslog misfeature; this seems to override any value in
# /etc/rsyslog.conf, which is good.
$RepeatedMsgReduction off


# Log all Swift proxy-server access log lines (local2) to
# /var/log/swift/proxy_access.log
local2.* /var/log/swift/proxy_access.log;RSYSLOG_FileFormat

# Log all Swift lines to /var/log/swift/all.log
# AND PREVENT FURTHER LOGGING OF THEM (eg. to /var/log/syslog)
local0.*;local2.* /var/log/swift/all.log;RSYSLOG_TraditionalFileFormat
& ~
```

 * restart rsyslog
```
service rsyslog restart
```
