# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2021-11-02

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2021-11-02)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22blacksheep%22%2C%22muffin%22%2C%22falcon%22%2C%22starlette%22%2C%22baize%22%2C%22emmett%22%2C%22sanic%22%2C%22fastapi%22%2C%22aiohttp%22%2C%22tornado%22%2C%22quart%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B517515%2C466410%2C425130%2C359265%2C344040%2C320055%2C301485%2C267150%2C213045%2C124485%2C110340%2C66900%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2021-11-02)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.0` | 18312 | 2.96 | 4.47 | 3.47
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 16350 | 3.37 | 4.98 | 3.90
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 15212 | 3.43 | 5.50 | 4.18
| [baize](https://pypi.org/project/baize/) `0.12.1` | 13746 | 3.92 | 5.95 | 4.63
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 13505 | 3.84 | 6.12 | 4.71
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 13085 | 3.96 | 6.34 | 4.86
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 9570 | 5.17 | 8.86 | 6.66
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 8684 | 5.77 | 9.57 | 7.41
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.0` | 7292 | 8.57 | 8.71 | 8.78
| [quart](https://pypi.org/project/quart/) `0.15.1` | 3423 | 19.27 | 19.86 | 18.68
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3318 | 18.93 | 19.11 | 19.29
| [django](https://pypi.org/project/django/) `3.2.9` | 1883 | 33.61 | 36.74 | 34.28


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.0` | 10490 | 4.77 | 8.20 | 6.08
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 10346 | 4.83 | 8.20 | 6.15
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 9659 | 7.04 | 8.24 | 6.63
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 8011 | 6.20 | 10.63 | 7.96
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 7433 | 6.59 | 11.35 | 8.63
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 6797 | 9.51 | 11.57 | 9.53
| [baize](https://pypi.org/project/baize/) `0.12.1` | 6488 | 10.07 | 10.32 | 9.86
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 6062 | 10.68 | 11.54 | 10.55
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.0` | 4658 | 13.49 | 13.94 | 13.74
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2860 | 22.26 | 22.41 | 22.38
| [quart](https://pypi.org/project/quart/) `0.15.1` | 2158 | 29.07 | 29.61 | 29.65
| [django](https://pypi.org/project/django/) `3.2.9` | 1581 | 38.48 | 43.17 | 40.44

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.0` | 5699 | 8.80 | 15.31 | 11.27
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 4398 | 11.24 | 20.03 | 14.58
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 3982 | 12.29 | 21.48 | 16.09
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 3471 | 18.04 | 22.77 | 18.59
| [baize](https://pypi.org/project/baize/) `0.12.1` | 2702 | 22.74 | 25.60 | 23.67
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 2435 | 26.58 | 35.24 | 26.27
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.0` | 2253 | 28.21 | 28.34 | 28.49
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 2178 | 30.26 | 39.16 | 29.35
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2121 | 30.05 | 30.16 | 30.17
| [quart](https://pypi.org/project/quart/) `0.15.1` | 1775 | 35.51 | 36.27 | 36.15
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 1455 | 40.99 | 48.11 | 44.10
| [django](https://pypi.org/project/django/) `3.2.9` | 996 | 63.49 | 70.47 | 64.26


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [blacksheep](https://pypi.org/project/blacksheep/) `1.2.0` | 517515 | 5.51 | 9.33 | 6.94
| [muffin](https://pypi.org/project/muffin/) `0.86.0` | 466410 | 6.48 | 11.07 | 8.21
| [falcon](https://pypi.org/project/falcon/) `3.0.1` | 425130 | 9.5 | 12.17 | 9.8
| [starlette](https://pypi.org/project/starlette/) `0.16.0` | 359265 | 12.21 | 17.33 | 12.98
| [baize](https://pypi.org/project/baize/) `0.12.1` | 344040 | 12.24 | 13.96 | 12.72
| [emmett](https://pypi.org/project/emmett/) `2.3.2` | 320055 | 18.15 | 22.01 | 19.5
| [sanic](https://pypi.org/project/sanic/) `21.9.1` | 301485 | 8.22 | 14.13 | 10.71
| [fastapi](https://pypi.org/project/fastapi/) `0.70.0` | 267150 | 15.37 | 19.85 | 15.52
| [aiohttp](https://pypi.org/project/aiohttp/) `3.8.0` | 213045 | 16.76 | 17.0 | 17.0
| [tornado](https://pypi.org/project/tornado/) `6.1` | 124485 | 23.75 | 23.89 | 23.95
| [quart](https://pypi.org/project/quart/) `0.15.1` | 110340 | 27.95 | 28.58 | 28.16
| [django](https://pypi.org/project/django/) `3.2.9` | 66900 | 45.19 | 50.13 | 46.33

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)