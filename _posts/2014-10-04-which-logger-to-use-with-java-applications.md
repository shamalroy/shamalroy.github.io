---
layout: post
title: Which Logger to use with Java applications?
date: 2014-10-04 00:00:00
---

Which Logger to use with Java applications?

Logging is one of the most significant and necessary parts of any application design. It helps developers and support teams to debug and gather enough information on the logic flow of the application. A well-designed logging system is helpful for maintaining applications for longer life cycle and saves thousands of hours.

Sometimes it is confusing among new developers when it comes to choosing a logging framework for their applications. I always used to get questions like what is the difference between slf4j and log4j, etc. In this blog, I will try to give a basic overview of Logging utilities available for Java and how they work. I hope it will help someone who just started developing in Java or trying to choose the right logging framework.

## Logging facade vs Logging Implementations

Example logging facades are SLF4J or Apache Commons Logging. It is the abstraction layer of the logging system; it’s not an implementation. That means if you write an application that uses a logging facade you can use any logging implementations like log4J or Logback latter on based on your need. It helps to project being independent of too much dependency of logging implementation layers.

On the other hand, Logging implementation framework examples are LOG4J, Logback, Java Logging API. Most of them are plug-able with well-known Logging Facades. In general, most Logging implementations include three pieces, Logger, Formatter, and Appender. Logger captures the messages from the Application or Facades with necessary metadata. Formatter formats the messages based on configuration details. Appender gets those formatted data and appends to predefined output. It might be appended to a database, or disk file system or console.

Logging implementations can be used directly without a facade in an application.

![]({{site.baseurl}}/images/2014-10-04-logging.png "How Logger frameworks work with the Application")
How Logger frameworks work with the Application

## Which Facade to choose?

Most popular Java Logging Facades are SLF4J and Apache Commons Logging. Apache Commons Logging uses a runtime discovery algorithm that looks for other logging frameworks in different common places in the classpath and chose the right one or you can configure it which one you want to use. This approach uses dynamic binding/class loading that poses to numerous problems, including unexpected behavior, complex to debug class loading problems.
You can check out Ceki’s Article [Think again before adopting the commons-logging API][cekis-article]. Ceki is the inventor of SLJ4J, LOG4J, and Logback.

SLF4J uses static binding for avoiding the issues introduced by JCL. It also has a native implementation layer for other logging frameworks. It also provides a JCL to SLF4J bridge for the applications/ Frameworks which still uses JCL for Logging.

![]({{site.baseurl}}/images/2014-10-04-slf4jvsacl.png "SLF4J vs Apache Commons Logging")
SLF4J vs Apache Commons Logging

[cekis-article]: http://articles.qos.ch/thinkAgain.html