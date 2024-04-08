# clear-agnostic
Solace PubSub+ Distributed Tracing

Introduction
---

This Solace PubSub+ Distributed Tracing demo is based on work from Daniel Brunold which drew its inspiration from the Solace Codelabs. 

The name of this repo is an anagram for Solace Tracing and this time the anagram actually makes sense.

Background
---

In folder `docs` you find file `SolacePubSub+DistributedTracingDemo.pdf` describing the setup, and file `SolacePubSub+DistributedTracingDemo.pdf` with an introduction on Solace PubSub+ Distributec Tracing.

Setup
---

Assumes MacOS, some hints are given for Windows where commands differ.

Get the `clear-agnostic` repo, e.g. using a code editor or in a terminal run:

`git clone https://github.com/taatuut/clear-agnostic.git`

Open the code in your favorite code editor (using Visual Studio Code myself).

Run
---

NOTE: this README provides a short summary to get everything running. More info can be found in the documentation.

Open four terminals (using Terminal or something like iTerm):

_Start Jaeger_

```
cd ~/jaeger/jaeger-1.53.0-darwin-amd64/
./jaeger-all-in-one
```

Or detached `#nohup ./jaeger-all-in-one > /dev/null 2>&1 &`

To stop kill the process with `Control-C`.

In case you need to kill a detached process get pid for port then kill:

```
sudo lsof -i tcp:4317
Password:
COMMAND     PID       USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
jaeger-al 52983 emilzegers   13u  IPv6 0x7abfc551d38db5d9      0t0  TCP *:4317 (LISTEN)
jaeger-al 52983 emilzegers   26u  IPv6 0x7abfc551d38dc5d9      0t0  TCP localhost:53137->localhost:4317 (ESTABLISHED)
jaeger-al 52983 emilzegers   27u  IPv6 0x7abfc551d38dcdd9      0t0  TCP localhost:4317->localhost:53137 (ESTABLISHED)
kill -15 52983
sudo lsof -i tcp:4317

```

Navigate to http://localhost:16686 to access the Jaeger UI.

_Start OTEL collector_

Create required environment variables for yaml file, if any

`source /path/to/.env`

```
cd ~/otelcol/otelcol-contrib_0.96.0_darwin_arm64
./otelcol-contrib --config=../otel-collector-config-single.yaml
```

To stop kill the process with `Control-C`.

_Start 3.1	Solace SDKPerf_

SDKPerf acts as both publisher and consumer in this sertup. Topics and queues must be generated upfront, see documentation.

Run repeatedly every 10 seconds.

```
cd ~/sdkperf/sdkperf-jcsmp-8.4.14.10
while true; do ./sdkperf_java.sh -cip=tcps://mr-connection-5uta8l8extu.messaging.solace.cloud:55443 -cu=solace-cloud-client@ez-aws-fra -cp=deun1l905ashrflooldf1qhrfg -ptl='demo/trace' -sql='queue-trace1,queue-trace2' -mt=persistent -mn=1 -mr=1 -msa=32768 -q -tcc -tcrc -tecip="http://localhost:4317"; sleep 10; done
```

To stop kill the process with `Control-C`.

_Start PMOTEL_

Poor Man’s OTEL endpoint, a Python script that processes POST requests from otel collector exporter with metrics in protobuf, (gzipped) JSON or something else and just displays the data received.

`python3 myhttpserver.py`

To stop kill the process with `Control-C`.

Navigate to http://localhost:3317 to view PMOTEL output.

Links
---

https://opentelemetry.io/docs/collector/configuration/

