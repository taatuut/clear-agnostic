Exporter json
---

Able to receive data in JSON format now...

Using version 0.96.0

`UnicodeDecodeError: 'utf-8' codec can't decode byte 0x8b in position 1: invalid start byte`

gzip's magic number is 0x1f 0x8b, consistent with above UnicodeDecodeError.

Fooled myself for quite a while assuming data was not compressed while it was gzipped (and this with default set to none according to docs...). Will wrap up and feed back internally, maybe useful for Heineken.

I spent a bit of time testing with the OTLP Exporter using JSON (regarding Heineken request). Looks like it is not there yet in the 0.92.0 version of the collector I'm using. There is an approved request committed (https://github.com/open-telemetry/opentelemetry-collector/commit/e8748663d34dd0c2af1878eee45395c35c9b58c5), will check if/when that becomes available in a (next) version. I did manage to display the protobuf content in my own poor man's metrics receiver (Python script) so that comes in handy and will extend that to display json too when it is there.


Run scripts
---

```
python3 myhttpserver.py

GET

http://localhost:3317/?test=ok

POST

curl -X POST -d "this works too" http://localhost:3317/
```

Links
---

https://piehost.com/blog/python-websocket