# Mock or Echo Servers

## MockServer

https://www.mock-server.com/

```
docker run --rm -it -p 1080:1080 mockserver/mockserver
```

There is an API to set expectations and responses.

## httpbin

https://github.com/postmanlabs/httpbin

https://httpbin.org/

```
docker run --rm -it -p 80:80 kennethreitz/httpbin
```

HTTP request and response service. Endpoints echo back request data and simulate
status codes, redirects, auth, headers, and delays.
