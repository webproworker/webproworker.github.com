---
layout: post
title: "Log4net configuration"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I always forget how to configure log4net to enable it’s powerful logging capabilities. So here’s how:

For web applications: in your global.aspx file, when the application starts, add this line: XmlConfiguration.Configure();

For desktop applications add the same line in the main function.

ex:

{% highlight ruby %}
  protected void Application_Start()
  {
  XmlConfigurator.Configure();
  }
{% endhighlight %}

In  web.config / app.config add this configuration:

Declare the section in the section configuration

{% highlight ruby %}
  <configSections>
  <section name=”log4net” type=”log4net.Config.Log4NetConfigurationSectionHandler, log4net” />
  </configSections>
{% endhighlight %}

Configure the logger for file logging:

{% highlight ruby %}
  <log4net>
  <appender name=”LogFileAppender” type=”log4net.Appender.FileAppender”>
  <param name=”File” value=”Logs\\Log4Net.log” />
  <layout type=”log4net.Layout.PatternLayout”>
  <param name=”ConversionPattern” value=”%d [%t] %-5p %c %m%n” />
  </layout>
  </appender>
  <logger name=”File”>
  <level value=”All” />
  <appender-ref ref=”LogFileAppender” />
  </logger>
  </log4net>
{% endhighlight %}

That’s it, hope this helps!
