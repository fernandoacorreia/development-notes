# Web servers

## Angie

A powerful, scalable, and efficient web server that is a drop-in replacement for Nginx, featuring enhanced monitoring and HTTP/3 support.

- webserver-llc/angie
- angie.software

Highlights:
- A high-performance fork of Nginx created by former core Nginx developers.
- Designed as a drop-in replacement that maintains compatibility with existing Nginx configurations.
- Includes built-in features that are usually locked behind Nginx Plus, such as advanced monitoring and a dynamic HTTP statistics API.
- Provides native support for modern protocols like HTTP/3 out of the box.

## Caddy

Enterprise-ready, extensible, open source, and automatic HTTPS with a configuration API.

- caddyserver/caddy
- caddyserver.com

Highlights:
- A modern web server written in Go, famous for its "automatic HTTPS" feature using Let's Encrypt.
- Uses a simple, human-readable configuration file (Caddyfile) that is significantly shorter than Nginx configs.
- Functions as a single static binary with no external dependencies, making it very portable.
- Includes built-in support for HTTP/3, Zstandard compression, and on-demand TLS.

## Envoy

A cloud-native, high-performance edge/middle/service proxy designed for large microservice service mesh architectures.

- envoyproxy/envoy
- envoyproxy.io

Highlights:
- An L7 proxy and communication bus designed for large-scale microservice "service mesh" architectures.
- Focused on deep observability, providing advanced metrics, distributed tracing, and wire-level insights.
- Optimized for dynamic configuration via APIs (xDS) rather than static files, allowing updates without restarts.
- Commonly used as the "data plane" for service mesh controllers like Istio.

## HAProxy

The world's fastest and most widely used software load balancer, offering high availability and proxying for TCP and HTTP-based applications.

- haproxy/haproxy
- haproxy.org

Highlights:
- A dedicated, high-performance load balancer and proxy known for extreme reliability and speed.
- Handles millions of concurrent connections with very low CPU and memory overhead.
- Provides the most granular health checking and traffic-shaping features among the list.
- Unlike Nginx or Caddy, it does not serve static files; it is purely a traffic manager.

## Nginx

High-performance HTTP server, reverse proxy, and IMAP/POP3 proxy server known for its stability and low resource consumption.

- nginx/nginx
- nginx.org

Highlights:
- The industry standard for high-performance web serving, reverse proxying, and caching.
- Uses an asynchronous, event-driven architecture that handles high volume with a small resource footprint.
- Features a massive ecosystem of modules and decades of community documentation.
- Requires manual configuration for SSL/TLS certificates and lacks a built-in dashboard in the free version.

## OpenResty

- https://openresty.org/en/
- https://github.com/openresty/openresty

Highlights:
- A full-fledged web platform that bundles Nginx with LuaJIT to allow developers to build high-performance web apps inside the server itself.
- It is technically Nginx "on steroids" and is used by massive companies like Cloudflare to handle global traffic.
- Best for: Complex request routing, dynamic security rules, and high-performance APIs.

## Traefik

The cloud-native application proxy: a modern HTTP reverse proxy and load balancer made to deploy microservices with ease.

- traefik/traefik
- traefik.io

Highlights:
- A cloud-native edge router designed specifically for containerized environments like Docker and Kubernetes.
- Features "auto-discovery," where it automatically detects new services and creates routing rules based on container labels.
- Includes a built-in visual dashboard for monitoring real-time traffic and routing health.
- Native support for Let's Encrypt, automatically managing SSL certificates for all discovered services.
