A [structured-log](https://github.com/diginet-ab/structured-log) plugin that writes log events to [Seq](https://getseq.net/).

**Requires polyfills for `Promise` and `fetch` if those aren't supported in your target platform/browser.**

### Installation

```
npm i @diginet/structured-log-seq-sink --save
```

### Usage

```js
var structuredLog = require('@diginet/structured-log');
var seqSink = require('@diginet/structured-log-seq-sink');

var logger = structuredLog.configure()
  .writeTo(seqSink({ /* ... options ...  */ }))
  .create();

```

##### Available options

|Parameter|Description|
|---|---|
|`apiKey`|(optional) API key to use|
|`compact`|(optional) If true, events be serialized using [Serilog's compact format](https://github.com/serilog/serilog-formatting-compact)|
|`durable`|(optional) If true, events will be buffered in local storage if available|
|`levelSwitch`|(optional) `DynamicLevelSwitch` which the Seq log level will control and use |
|`suppressErrors`|(optional) If true, errors in the pipeline will be suppressed and logged to the console instead (defaults to true)|
|`url`|(required) URL to the Seq server|

#### Dynamic Level Control

Much like [Serilog's Dynamic Level Control via Seq](http://docs.getseq.net/docs/using-serilog#dynamic-level-control), Seq can be used to dynamically
control the log level of `structured-log`.  To configure, setup a `DynamicLevelSwitch` and pass it to the sink:

```js
var levelSwitch = new structuredLog.DynamicLevelSwitch("info")
var log = structuredLog.configure()
    .minLevel(levelSwitch)
    .writeTo(SeqSink({
        url: "http://localhost:5341",
        apiKey: "API_KEY",
        levelSwitch: levelSwitch
    }))
    .create();
```

This can be used as the log level across the entire pipeline (by using `.minLevel(levelSwitch)`, or just for the 
seq sink (by passing it in the `options` array).


### Building and testing

To build the modules yourself, you need Node 8 or greater. Check out the code, then install dependencies and run the `build` script:

```
npm i
npm run build
```

Then, you can test the bundled module by running:

```
npm test
```
