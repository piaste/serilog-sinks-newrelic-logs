
A Serilog sink that writes events to the [NewRelic Logs](https://docs.newrelic.com/docs/logs/new-relic-logs/get-started/introduction-new-relic-logs).

## Getting started

```csharp
Log.Logger = new LoggerConfiguration()
    .WriteTo.NewRelicLogs(
        endpointUrl: "https://log-api.newrelic.com/log/v1", 
        applicationName: "Serilog.Sinks.NewRelic.Sample", 
        licenseKey: "[Your API key]")
    .CreateLogger();
```

The available parameters are:
* `applicationName` of the current application in NewRelic If the parameter is omitted, then the value of the "NewRelic.AppName" appSetting will be used.
* `endpointUrl` is the ingestion URL of NewRelic Logs. The US endpoint is used by default if this value is omitted.
* `licenseKey` is the NewRelic License key, which is also used with the NewRelic Agent.
* `insertKey` is New Relic Insert API key. Either `licenseKey` or `insertKey` must be supplied.

The events are submitted to NewRelic Logs in batches, and the sink is derived from [PeriodicBatchingSink](https://github.com/serilog/serilog-sinks-periodicbatching). It therefore supports the following parameter:
* `batchSizeLimit` is the maximum number of events to include in a single batch. Default is 1000 entries
* `period` is the time to wait between checking for event batches. It is TimeSpan with a default value of 2 seconds. If provided from [AppSettings](https://github.com/serilog/serilog/wiki/AppSettings),
the value should be given as an absolute time span, i.e.: "0.00:00:05" standing for 5 seconds.

The batches are formatted using NewRelic Logs [detailed JSON body](https://docs.newrelic.com/docs/logs/new-relic-logs/log-api/introduction-log-api#json-content) and are transmitted GZip-compressed.

All properties along with the rendered message will be emitted to NewRelic Logs.
This sink adds four additional properties:
* `timestamp` in milliseconds since epoch
* `application` holds the value from `applicationName`
* `level` is the actual log level of the event.
* `stack_trace` holds the stack trace portion of an exception.

If `newrelic.linkingmetadata` property is present in an event, it will be unrolled into individual NewRelic properties used for "logs in context".

## Install via NuGet

```
PM> Install-Package Serilog.Sinks.NewRelic.Logs
```

## How can I contribute?

Please, refer to [CONTRIBUTING](.github/CONTRIBUTING.md)

## Found something strange or need a new feature?

Open a new Issue following our issue template [ISSUE_TEMPLATE](.github/ISSUE_TEMPLATE.md)

## Changelog

See in [nuget version history](https://www.nuget.org/packages/JsonMasking)

## Did you like it? Please, make a donate :)

if you liked this project, please make a contribution and help to keep this and other initiatives, send me some Satochis.

BTC Wallet: `1G535x1rYdMo9CNdTGK3eG6XJddBHdaqfX`

![1G535x1rYdMo9CNdTGK3eG6XJddBHdaqfX](https://i.imgur.com/mN7ueoE.png)