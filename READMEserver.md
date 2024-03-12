Exporter json
---

Able to receive data in JSON format now...

Using version 0.96.0. It was not in the 0.92.0 version of the collector.

`UnicodeDecodeError: 'utf-8' codec can't decode byte 0x8b in position 1: invalid start byte`

gzip's magic number is 0x1f 0x8b, consistent with above UnicodeDecodeError.

Fooled myself for quite a while assuming data was not compressed while it was gzipped (and this with default set to none according to docs...).

Testing with the OTLP exporter json.

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

https://github.com/open-telemetry/opentelemetry-collector/commit/e8748663d34dd0c2af1878eee45395c35c9b58c5
https://piehost.com/blog/python-websocket