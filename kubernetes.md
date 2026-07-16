# Kubernetes

## kubeshark

Deep Network Observability for kubernetes

Inspect all internal and external cluster communications, API transactions, and data in transit with cluster-wide network visibility, monitoring all traffic going in, out, and across containers, pods, namespaces, nodes, and clusters.

https://www.kubeshark.co/

## Envoy Gateway

- https://gateway.envoyproxy.io/
- https://github.com/envoyproxy/gateway
- https://kubernetes.io/docs/concepts/services-networking/gateway/

## Kubernetes log viewers

### Stern

https://github.com/stern/stern

```
stern my-service -n my-namespace
stern -l app=my-service -n my-namespace
stern deployment/my-service -n my-namespace
stern my-service --since 10m
stern my-service --all-namespaces
```

### K9s

https://github.com/derailed/k9s

### Kubetail

https://github.com/kubetail-org/kubetail

### Kail

https://github.com/boz/kail
