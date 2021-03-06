# Log4NetAdoNetAppender
.Net Standard 1.3 AdoNetAppender

A late night fix since it do not exists for .net core for some odd reason. This project will only exists until the official [logging.apache.org](https://logging.apache.org/) project is releasing their own AdoNetAppender within the log4net package.

## How to setup

You log4net.config AdoNetAppender configuration

```
<log4net>
  <appender name="AdoNetAppender" type="MicroKnights.Logging.AdoNetAppender, MicroKnights.Log4NetAdoNetAppender">
    <bufferSize value="1" />
    <connectionType value="System.Data.SqlClient.SqlConnection,System.Data,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b77a5c561934e089" />
    <connectionStringName value="log4net" />
    <connectionStringFile value="connectionstrings.json" />
    <commandText value="INSERT INTO Log ([Date],[Thread],[Level],[Logger],[Message],[Exception]) VALUES (@log_date, @thread, @log_level, @logger, @message, @exception)" />
    <parameter>
        <parameterName value="@log_date" />
        <dbType value="DateTime" />
        <layout type="log4net.Layout.RawTimeStampLayout" />
    </parameter>
    <parameter>
        <parameterName value="@thread" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
            <conversionPattern value="%thread" />
        </layout>
    </parameter>
    <parameter>
        <parameterName value="@log_level" />
        <dbType value="String" />
        <size value="50" />
        <layout type="log4net.Layout.PatternLayout">
            <conversionPattern value="%level" />
        </layout>
    </parameter>
    <parameter>
        <parameterName value="@logger" />
        <dbType value="String" />
        <size value="255" />
        <layout type="log4net.Layout.PatternLayout">
            <conversionPattern value="%logger" />
        </layout>
    </parameter>
    <parameter>
        <parameterName value="@message" />
        <dbType value="String" />
        <size value="4000" />
        <layout type="log4net.Layout.PatternLayout">
            <conversionPattern value="%message" />
        </layout>
    </parameter>
    <parameter>
        <parameterName value="@exception" />
        <dbType value="String" />
        <size value="2000" />
        <layout type="log4net.Layout.ExceptionLayout" />
    </parameter>
  </appender>
```

With 2 changes, you are up and running in your .net core project.

First you must change/replace the type and assembly name, from this:
```
<appender name="AdoNetAppender" type="log4net.Appender.AdoNetAppender">
```
to this:
```
<appender name="AdoNetAppender" type="MicroKnights.Logging.AdoNetAppender, MicroKnights.Log4NetAdoNetAppender">
```

Secondly you must specify the configurationfile where your connectionstrings are defined. A new property in the configuration is added - called `ConnectionStringFile`. It will then locate the connectionstring by using `ConnectionStringName`.

# NuGet Package
```
PM> Install-Package MicroKnights.Log4NetAdoNetAppender
```

