---
layout: post
title: Installing Maven on OS X
date: 2014-10-02 00:00:00
---

Prior versions of OSX Mavericks (10.9) comes with Maven 3 pre-installed. If you are using OSX 10.9 or higher, your options are to install maven yourself or use Homebrew or MacPorts. I prefer to install it myself. The steps are fairly simple and straight forward.

First check you have Java installed on your machine, by “java -version” command. If you don’t have Java installed, download it from Oracle website and install it first. Now you are ready for maven installation.

1. Download the latest binary zip version of maven from http://maven.apache.org/ site.

2. Mac OSX will unzip .zip or .tar.gz files automatically when you double click. Otherwise extract it using the command below and move the extracted folder somewhere under our development tools directory. I prefer to keep all my tools in my user folder.

```
$ gunzip -c apache-maven-3.2.3-bin.tar.gz | tar xopf -
$ mv apache-maven-3.2.3 ~/Documents/Dev/Tools/
```

3. Open your ~/.bash_profile or ~/.profile file in vim or nano or TextEdit to add the environment variables. If you have both ~/.bash_profile and ~/.profile, use ~/.bash_profile because ~/.bash_profile will take precedence over ~/.profile.
By default, this file does not exist in any version of OS X. If you did not create it in the past, create one now.

```
$ nano ~/.bash_profile
```

4. Add or append the below lines in the .bash_profile file. Here you are adding JAVA_HOME, M2_HOME, and M2 environment variables and appending those new variables with PATH variable.

```
export M2_HOME=/Users/shamalroy/Documents/Dev/Tools/apache-maven-3.2.3
export M2=$M2_HOME/bin
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$M2:$JAVA_HOME/bin:$PATH
```

5. After you are done, save the file by pressing Control+Z keys.

6. Quit open Terminal and reopen it.

7. Type “mvn -version” command to check the installation.

```
$ mvn -version
Apache Maven 3.2.3 (33f8c3e1027c3ddde99d3cdebad2656a31e8fdf4; 2014-08-11T16:58:10-04:00)
Maven home: /Users//Documents/Dev/Tools/apache-maven-3.2.3
Java version: 1.7.0_67, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.7.0_67.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.9.5", arch: "x86_64", family: "mac"
```

You have successfully configured maven on your Mac.