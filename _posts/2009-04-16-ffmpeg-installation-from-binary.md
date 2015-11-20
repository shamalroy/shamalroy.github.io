---
layout: post
title: FFMPEG installation from binary
date: 2009-04-16 00:00:00
---

FFMPEG installation on RedHat EL 4 – i386.
This installation will run probably for all i386 systems.
1. login to your remote server using this command:

```
# ssh username[at]ip_address
```

2. Then install the following RPMs using the commands.
   
```
# rpm -Uvh --nodeps ftp://ftp.univie.ac.at/systems/linux/dag/redhat/el4/en/i386/RPMS.dag/ffmpeg-0.4.9-0.9.20070530.el4.rf.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-i386/atrpms/stable/libogg0-1.1.3-7.el4.at.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.univie.ac.at/systems/linux/dag/redhat/el4/en/i386/dag/RPMS/gsm-1.0.10-6.el4.rf.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-i386/atrpms/stable/libmp3lame0-3.97-16.el4.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-i386/atrpms/stable/libvorbis0-1.1.2-5.el4.at.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-x86_64/atrpms/stable/libvorbisenc2-1.1.2-5.el4.at.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.univie.ac.at/systems/linux/dag/redhat/el4/en/i386/RPMS.dag/xvidcore-1.1.3-1.el4.rf.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.univie.ac.at/systems/linux/dag/redhat/el4/en/i386/dag/RPMS/x264-0.0.0-0.4.20070529.el4.rf.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-i386/atrpms/stable/libfaac0-1.25-2.el4.at.i386.rpm
# rpm -Uvh --nodeps ftp://ftp.pbone.net/mirror/atrpms.net/el4-x86_64/atrpms/stable/faad2-2.5-7.el4.at.i386.rpm
```

3. Testing:
   Make a test directory.

```
# mkdir /root/ffmpeg_test
# cd /root/ffmpeg_test/
```

Get sample MPEG file and test the following. Then convert MPEG from FLV:

```
# ffmpeg -i FORM.MPG -ar 22050 -ab 32 -f flv -s 320x240 -aspect 4:3 -y
```

Convert IMAGE From FLV:

```
# ffmpeg -i FORM.flv -deinterlace -an -ss 1 -t 00:00:01 -r 1 -y -s 320?240 -vcodec mjpeg -f mjpeg form.jpg
```