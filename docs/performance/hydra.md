---
id: hydra
title: ORY Hydra
---

In this document you will find benchmark results for different endpoints of ORY
Hydra. All benchmarks are executed using
[rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks
run against the in-memory storage adapter of ORY Hydra. These benchmarks
represent what performance you would get with a zero-overhead database
implementation.

We do not include benchmarks against databases (e.g. MySQL, PostgreSQL or
CockroachDB) as the performance greatly differs between deployments (e.g.
request latency, database configuration) and tweaking individual things may
greatly improve performance. We believe, for that reason, that benchmark results
for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark
data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All
benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using
flows such as the OAuth 2.0 Client Credentials Grant, ORY Hydra validates the
client credentials using BCrypt which causes (by design) CPU load. CPU load and
performance depend on the BCrypt cost which can be set using the environment
variable `BCRYPT_COST`. For these benchmarks, we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	2.3488 secs
  Slowest:	0.1545 secs
  Fastest:	0.0003 secs
  Average:	0.0223 secs
  Requests/sec:	4257.4167

  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.016 [5238]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.031 [1751]	|■■■■■■■■■■■■■
  0.047 [1275]	|■■■■■■■■■■
  0.062 [876]	|■■■■■■■
  0.077 [488]	|■■■■
  0.093 [265]	|■■
  0.108 [77]	|■
  0.124 [19]	|
  0.139 [9]	|
  0.154 [1]	|


Latency distribution:
  10% in 0.0005 secs
  25% in 0.0009 secs
  50% in 0.0140 secs
  75% in 0.0367 secs
  90% in 0.0592 secs
  95% in 0.0727 secs
  99% in 0.0933 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0003 secs, 0.1545 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0081 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0054 secs
  resp wait:	0.0222 secs, 0.0002 secs, 0.1544 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0080 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	19.9343 secs
  Slowest:	1.1668 secs
  Fastest:	0.0171 secs
  Average:	0.1914 secs
  Requests/sec:	501.6473

  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.132 [4405]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.247 [2747]	|■■■■■■■■■■■■■■■■■■■■■■■■■
  0.362 [1600]	|■■■■■■■■■■■■■■■
  0.477 [861]	|■■■■■■■■
  0.592 [295]	|■■■
  0.707 [73]	|■
  0.822 [15]	|
  0.937 [0]	|
  1.052 [1]	|
  1.167 [2]	|


Latency distribution:
  10% in 0.0324 secs
  25% in 0.0924 secs
  50% in 0.1811 secs
  75% in 0.2795 secs
  90% in 0.3855 secs
  95% in 0.4220 secs
  99% in 0.5881 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0171 secs, 1.1668 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0068 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0603 secs
  resp wait:	0.1912 secs, 0.0170 secs, 1.1668 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0709 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading
from which negatively impacts performance. Performance will be better if IDs and
secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.4432 secs
  Slowest:	0.0252 secs
  Fastest:	0.0001 secs
  Average:	0.0042 secs
  Requests/sec:	22564.4850

  Total data:	4820000 bytes
  Size/request:	482 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [4698]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1342]	|■■■■■■■■■■■
  0.008 [1714]	|■■■■■■■■■■■■■■■
  0.010 [1349]	|■■■■■■■■■■■
  0.013 [579]	|■■■■■
  0.015 [187]	|■■
  0.018 [70]	|■
  0.020 [26]	|
  0.023 [9]	|
  0.025 [25]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0033 secs
  75% in 0.0073 secs
  90% in 0.0099 secs
  95% in 0.0116 secs
  99% in 0.0161 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0252 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0090 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0128 secs
  resp wait:	0.0038 secs, 0.0000 secs, 0.0220 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0179 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.3898 secs
  Slowest:	0.0228 secs
  Fastest:	0.0001 secs
  Average:	0.0036 secs
  Requests/sec:	25655.9465

  Total data:	4800000 bytes
  Size/request:	480 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [5295]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1116]	|■■■■■■■■
  0.007 [1532]	|■■■■■■■■■■■■
  0.009 [903]	|■■■■■■■
  0.011 [639]	|■■■■■
  0.014 [320]	|■■
  0.016 [117]	|■
  0.018 [50]	|
  0.020 [21]	|
  0.023 [6]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0016 secs
  75% in 0.0061 secs
  90% in 0.0095 secs
  95% in 0.0115 secs
  99% in 0.0154 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0228 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0058 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0099 secs
  resp wait:	0.0033 secs, 0.0000 secs, 0.0227 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0100 secs

Status code distribution:
  [200]	10000 responses



```
