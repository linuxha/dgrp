
           EtherLite(R) Firmware Update Instructions for UNIX

 
The EtherLite Units use TFTP in conjunction with BOOTP to update their
firmware.  In order to update your units, you will need to enable the
TFTP Server on the same host that provides BOOTP service.

We recommend that you disable this service as soon as you are done
updating the firmware, as TFTP does no user authentication and allows
file transfers.  As a result, it can be a security risk.

The BOOTP service is typically launched at boot time by the system, and is
configured according to an entry in /etc/inetd.conf, even though current
versions of bootp server software no longer uses inetd.  The bootp server
line in that file may look much like this:


bootps  dgram  udp  wait  root  /etc/bootpd  bootpd


TFTP service is typically enabled by "inetd".  An entry in the
"/etc/inetd.conf" file specifies how to start the service.  Most
systems will already have an entry for TFTP, but it will be commented
out, which disables it for security reasons mentioned above.  It may
look similar or identical to this:


tftp  dgram  udp  wait  root  /usr/etc/in.tftpd  in.tftpd -s /tftpboot


There should also be entries in your "/etc/services" file, specifying
BOOTP server service on UDP port 67 and the TFTP service on UDP port
69.  The entries in your /etc/services file should appear as shown
below.  If your system differs from the example given below, refer to
the man pages on your system for "tftpd" and "inetd" for details on how
to enable TFTP service.


bootps          67/udp          # bootp server
bootpc          68/udp          # bootp client
tftp            69/udp          # tftp server


To get the EtherLite Unit to download the new firmware, you need to
specify the bootfile option in the BOOTP entry for the unit.  To do
so, edit the "/etc/bootptab" file.  Add the field ":bf=filename" to
the entry for the EtherLite Unit, where "filename" is the full path
to the new firmware.  Please note that most TFTP implementations
perform a "chroot" (change root directory) when started.  If your TFTP
server does this, you will need to place the firmware image in its
directory tree and specify the path relative to TFTP's root directory.

For example, most TFTP servers have a "root" directory of /tftpboot on the
host, which means that a file whose path to TFTP is /files/bootfile, is
actually /tftpboot/files/bootfile to the UNIX system.  Your bootptab entry
may look like:


el16_0:\
    sm=255.255.255.0:\
    gw=192.9.200.1:\
    ht=ethernet:\
    ha=00A0E7000004:\
    ip=192.9.200.2:\
    bf=/bootp/sts/el16.prm:


In this case, the full path to the file on the UNIX side would be:


    /tftpboot/bootp/sts/el16.prm


To actually update the EtherLite Unit, you simply power cycle it.  Each
time it boots, it downloads the bootfile specified by BOOTP.  If the
bootfile differs from the firmware it is currently running, it will
update its Flash EPROM with the new firmware.

Once you have updated your EtherLite Unit, it is not necessary to leave
the TFTP server enabled.  As mentioned earlier it can be a security
risk.  Unless you are sure that your configuration is secure, we
recommend that you disable TFTP service by commenting out its entry in
"/etc/inetd.conf".  It is ok to leave in the bootfile entry in the
"/etc/bootptab".

