## Avahi (mDNS) discoverable

To be able to find the rPI add a file 
/etc/avahi/services/*filename*.service

with the following content

```xml
<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">

<service-group>

  <name replace-wildcards="yes">%h</name>

  <service>
    <type>_tcp._tcp</type>
    <port>8080</port>
  </service>

</service-group>
```
