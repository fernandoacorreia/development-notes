# Remote development on Kubernetes

## Telepresence

https://www.telepresence.io/

Fast, local development for Kubernetes and OpenShift Microservices

Telepresence is a Cloud Native Computing Foundation Sandbox project created by the team at Ambassador Labs

## mirrord

https://mirrord.dev/

Run your local code in the real-time context of your cloud environment, with access to other microservices, databases, queues, and managed services, all without leaving the local setup you know and love.

Comparison with Telepresence: https://mirrord.dev/docs/faq/comparisons/#how-is-mirrord-different-from-telepresence

Besides what is in that list, the fact that it can steal traffic from a container (and not the entire pod or service)
means that it works with istio even if you enforce mTLS in the service mesh.

## watchexec

https://github.com/watchexec/watchexec

Executes commands in response to file modifications

watchexec is a simple, standalone tool that watches a path and runs a command whenever it detects modifications.

Example use cases:

- Automatically run unit tests
- Run linters/syntax checkers
- Rebuild artifacts


## Articles

- [Developing and debugging services locally using telepresence (Kubernetes blog)](https://kubernetes.io/docs/tasks/debug/debug-cluster/local-debugging/)
- [One Year of Remote Kubernetes Development: Lessons Learned](https://thenewstack.io/one-year-of-remote-kubernetes-development-lessons-learned/)
- [Kubernetes Development Environments: From Local to Remote (Ambassador blog)](https://www.getambassador.io/blog/kubernetes-dev-environments-local-remote)
- [Local Development With Remote Clusters (Garden)](https://docs.garden.io/use-cases/local-development-remote-clusters)
