---
title: 'Envoy Getting started'
date: 2018-11-21T16:54:08+00:00
author: sumit
layout: post
#permalink: /2018/11/21/envoy-hello-world/
vantage_panels_no_legacy:
  - 'true'
siteorigin_page_settings:
  - 'a:6:{s:6:"layout";s:7:"default";s:10:"page_title";b:1;s:15:"masthead_margin";b:1;s:13:"footer_margin";b:1;s:13:"hide_masthead";b:0;s:19:"hide_footer_widgets";b:0;}'
categories:
  - containers
  - devops
tags:
  - envoy
  - geek
image: envoy-series/1/sidecar-image.jpg
description: Envoy getting started, getting familar with config.
published: false
---

![sidecar]({{site.baseurl}}/images/blogs/envoy-series/1/sidecar-image.jpg)

# Envoy's hello-world

Here is a series of posts in which we will get from basic of envoy to see how envoy is used in projects like istio. All the configs and task as tested using envoy version [v1.8.0](https://www.envoyproxy.io/docs/envoy/latest/intro/version_history#oct-4-2018)

Posts will be in following order

* Envoy hello-world: Get familiarity with envoy, basic static config and base features like front proxy, admin panel, stats.

* Basic service-mesh using envoy: Service to service communication using envoy.

* Envoy dynamic configurations: Next post will extend out configuration to get how we can work with dynamic configuration.

* Envoy in Istio: Finally that we will see how envoy fits in project like istio.


## Why envoy:

If you're into microservices architecture, one needs to implements the following many features like **rate-limiting** where we limit the requests coming to the services. It helps in scenarios like DDOS or if one of services has gone rogue in our services clusters, it should not take down all the services.

Retry Logic is only of features that one should use quite carefully while working with microservices, it is quite useful and dangerous features too, so quite good implementation of it is readily available to use, if we are using envoy proxy. There are many more features where we can use envoy to give various nicely implemented features to our services without messing with codebase.
 
One can implement all of those in application by code, there are multitude of libraries, blog-posts on how to do that. If organisation has multiple languages in use, then you need to implement for all of them, it's a lot of task to do.

Learning by screwing up everytime isn't a so-good way to go, if one can learn from others mistake and their solutions. All these features can be handled by envoy, so now we need to just focus on our business logic and let these features be handled by envoy.


## What's envoy:

Envoy is L4(TCP) proxy at its core, but has L7 filters (as HTTP is like omnipresent). Intention behind envoy is to give your application superpowers without writing much of code to do that. It acts like an armor for the application, one can think of it's as a **batman suite** to the **application**, which gives it power like

* Rate limiting.
* Filtering request.
* Health checking.
* Observability.
* Service discovery.
* Circuit Breaking
* Retry logic


Envoy is usually used as sidecar, it means your application is accompanied by a envoy, which gives your application features mentioned above with your additional business logic. Envoy is pretty optimised C++ codebase, so there is very low overhead, compared to functionality we get with it.

## Hello-world

Lets get started with hello-world for envoy from documentation itself.

In this hello-world program we will be

* Creating a docker container running envoy
* Envoy config will listen on port **9002** as main endpoint and **9001** for admin site.
* When 9002 port get some requests, it will just route it to "duckduckgo.com", nothing fancy.

```yaml
---
admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 0.0.0.0, port_value: 9001 }

static_resources:
  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 9002 }
    filter_chains:
    - filters:
      - name: envoy.http_connection_manager
        config:
          stat_prefix: ingress_http
          codec_type: HTTP1
          route_config:
            name: local_route
            virtual_hosts:
            - name: local_service
              domains: ["*"]
              routes:
              - match: { prefix: "/" }
                route: { host_rewrite: www.duckduckgo.com, cluster: service_ddg }
          server_name: my-test-envoy
          http_filters:
          - name: envoy.router

  clusters:
  - name: service_ddg
    connect_timeout: 2s
    type: LOGICAL_DNS
    hosts: [{ socket_address: { address: duckduckgo.com, port_value: 443 }}]
    tls_context: { sni: duckduckgo.com }```
```
Save the above yaml as `envoy.yaml`.

Below is Dockerfile for our hello-world example.
```yaml
FROM envoyproxy/envoy-alpine:v1.8.0
COPY envoy.yaml /etc/envoy/envoy.yaml
```

Directory structure should be like

```
 $ > ls
Dockerfile  envoy.yaml
```

To build the image:
* `docker build -t username/envoy-posts:v1 .`

To run it :
* `docker run --rm -p 9001:9001 -p 9002:9002  username/envoy-posts:v1`


Now to check if everything:  

* Envoy proxy hosts: <a href="http://localhost:9002/"  target="_blank">http://localhost:9002/</a>. It should redirect to <a href="https://duckduckgo.com/" target="_blank">duckduckgo.com</a>
* For viewing admin panel, go to this : <a href="http://localhost:90021/" target="_blank">http://localhost:9001/</a>

![stats-image](/images/blogs/envoy-series/1/envoy-stats.png)


Now  to understand the configuration:
--

We have 3 main section are:
* Admin section
* Static resources
* Listeners


### Admin section
`admin` section refers to administration panel of envoy, things like cluster config, listener, stats.

### static_resources

These are config that are done statically when envoy starts, we can do things dynamically  like dynamically fetching listeners config, but few posts will be dealing with static configuration of envoy until we are comfortable with it.

#### Listeners.

Listener is a L3/L4 endpoint where envoy listens to, after requests come to listeners we have `filter_chains` which contains the list of filters applied to request, here we are using `http_connection_manager` filter.


```
##### Extra info

Below  are built-in filters available.

Link: https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/api/v2/listener/listener.proto#envoy-api-msg-listener-filter


    * envoy.client_ssl_auth
    * envoy.echo
    * envoy.http_connection_manager
    * envoy.mongo_proxy
    * envoy.ratelimit
    * envoy.redis_proxy
    * envoy.tcp_proxy

`http_connection_manager` filter provides us with following http related configuration, it in-turn has multiple filters which handle various parts of request.

    * envoy.buffer
    * envoy.cors
    * envoy.fault
    * envoy.gzip
    * envoy.http_dynamo_filter
    * envoy.grpc_http1_bridge
    * envoy.grpc_json_transcoder
    * envoy.grpc_web
    * envoy.health_check
    * envoy.header_to_metadata
    * envoy.ip_tagging
    * envoy.lua
    * envoy.rate_limit
    * envoy.router
    * envoy.squash

Some of filters are quite useful like `health__check`, `rate_limit`.
```

* `route_config` : It has all the route configuration, It manages it by using `virtual_hosts`.
* `virtual_hosts` : Here we specify how the requests has to be moved, depending on host-headers.
    * In our config we say, any domain (*) which has prefix(/) move to `cluster_ddg`.

#### clusters

Cluster are endpoint where request will be routed to after **virtual_hosts** matches the requests.


So here we have seen the basic envoy configuration, and how envoy talks to requests and processes it. Next post we will try service to service communication using envoy in kubernetes.

---
Reference:

* Envoy docs url: [https://www.envoyproxy.io/docs/envoy/](https://www.envoyproxy.io/docs/envoy/)
* What is envoy: [https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy"> (https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy")
* http-connection-manager : [https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/config/filter/network/http_connection_manager/v2/http_connection_manager.proto#envoy-api-msg-config-filter-network-http-connection-manager-v2-httpconnectionmanager](https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/config/filter/network/http_connection_manager/v2/http_connection_manager.proto#envoy-api-msg-config-filter-network-http-connection-manager-v2-httpconnectionmanager)

* Listeners overview: [https://www.envoyproxy.io/docs/envoy/v1.8.0/intro/arch_overview/listeners](https://www.envoyproxy.io/docs/envoy/v1.8.0/intro/arch_overview/listeners
)
* virtual_hosts : [https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/api/v2/route/route.proto#envoy-api-msg-route-virtualhost](https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/api/v2/route/route.proto#envoy-api-msg-route-virtualhost)

* http_connection_manager filters: [https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/config/filter/network/http_connection_manager/v2/http_connection_manager.proto#envoy-api-msg-config-filter-network-http-connection-manager-v2-httpfilter](https://www.envoyproxy.io/docs/envoy/v1.8.0/api-v2/config/filter/network/http_connection_manager/v2/http_connection_manager.proto#envoy-api-msg-config-filter-network-http-connection-manager-v2-httpfilter)

&nbsp;
