---
layout: post
title: ATS(Apache Traffic Server) Quick start
description:
date: 2024-01-08 17:02:30 +0800
tags: ATS
---

# ATS(Apache Traffic Server) Quick start

(Zhang Zhao in Hangzhou)

## Abstract

The article is to guide newbies to learn ATS. A completed demo was given
to demonstrate how to configure a reverse proxy by ATS quickly.

## 1 Preface

### 1.1 Nginx

Nginx was used to build an origin server.

### 1.2 Curl

`curl` was used to send client requests.

## 2 The beginning

### 2.1 Build ATS from source code and then launch

It appeared "the autotools build is broken"<sup>1</sup> on the website, so the built version was the tag v9.2.3.
The building steps were referred from the official document<sup>2</sup>.

The following part was the main built steps:

```
cmake -B build
cmake --build build
sudo cmake --install build
```

It failed to launch ATS server by the method<sup>3</sup> in document,
and the following command worked in the article's environment:

```
/opt/ts/bin/traffic_server
```

### 2.2 Launch origin server

The origin server was a static Nginx server, which needed a configured file named `origin_server.conf`, whose details are below:

```
events {
    worker_connections 1024;
}

http {
    server {
        listen 8888;
        location / {
            root ./;
        }
    }
}
```

The command for launching Nginx server was:

```
/usr/bin/nginx -p <prefix> -c <your-config-path>/origin_server.conf
```

## 3 Reverse Proxy By ATS

### 3.1 The config file `remap.config`

`remap.config` is about **mapping rules** that ATS uses to perform the following actions:
- Map URL requests
- Reverse-map server location headers
- Redirect HTTP requests

**Effective**

After you modify the remap.config run the `traffic_ctl config reload` to apply the changes.

**Format**

Three space-delimited fields: `type`, `target`, and `replacement`.

`type`

Enter one of the following:
- `map`-translates a client request URL to the origin server URL.

`target`

```
scheme://host:port/path_prefix
```

`replacement`

```
scheme://host:port/path_prefix
```

**Examples: Reverse Proxy Mapping Rules**

```
# The two directives do the same work
map http://www.x.com/ http://server.hoster.com/
reverse_map http://server.hoster.com/ http://www.x.com/
```

This rule results in the following translations:

|Client Request|Translated Request|
|------------|------------|
|http://www.x.com/Widgets/index.html | http://server.hoster.com/Widgets/index.html |
|http://www.x.com/cgi/form/submit.sh?arg=true | http://server.hoster.com/cgi/form/submit.sh?arg=true |

### 3.2

ATS server `remap.config`:

```
map http://eric.coco.jack/ http://localhost:8888/
```

## 4 Testing

Send a request to verify the config working.

```
#note: ATS server default uses 8080 port.
# URI neville/ats/origin_server.conf is valid in the article environment.
curl -x 127.0.0.1:8080  http://eric.coco.jack/neville/ats/origin_server.conf
```

## References

1. [the autotools build is broken](https://github.com/apache/trafficserver), "ATS is transitioning to cmake as its build system. At the moment, the autotools build is broken and will soon be removed from the repository"
2. Document: [Build Dependencies](https://docs.trafficserver.apache.org/en/latest/admin-guide/installation/index.en.html)
3. [Start Traffic Server](https://docs.trafficserver.apache.org/en/latest/admin-guide/installation/index.en.html)