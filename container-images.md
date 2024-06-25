# Container Images

## Building container images

### Chainguard static image

This image contains certificates, timezone data, public keys, etc.

Base image:
- https://edu.chainguard.dev/chainguard/chainguard-images/reference/static/
- static-dev variant for an image with shell, package manger and utilities

Videos:
- https://edu.chainguard.dev/chainguard/chainguard-images/videos/static-base-image/

## Debugging container images

### docker debug

Docker Debug requires a Pro, Teams, or Business Subcription.

https://docs.docker.com/reference/cli/docker/debug/

```
docker debug ${container-name}
```

### nsenter1

https://github.com/justincormack/nsenter1

This image allows you to run a privileged container that runs nsenter for the process space running as pid 1.

### cdebug

- https://github.com/iximiuz/cdebug
- https://iximiuz.com/en/posts/docker-debug-slim-containers/
- https://edu.chainguard.dev/chainguard/chainguard-images/videos/kubectl_cdebug/

```
cdebug exec -it <target-container-name-or-id>
```

or

```
cdebug exec --privileged -it --image nixery.dev/shell/ps/vim/tshark <target>
```

### kubectl debug

https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/#ephemeral-container

You can use the kubectl debug command to add ephemeral containers to a running Pod.

## Container images scanning and SBOM (Software Bill of Materials)

### grype

https://github.com/anchore/grype

A vulnerability scanner for container images and filesystems.

### syft

https://github.com/anchore/syft

CLI tool and library for generating a Software Bill of Materials from container images and filesystems.

### Docker Scout

https://docs.docker.com/scout/

Docker Scout is a solution for proactively enhancing your software supply chain security. By analyzing your images, Docker Scout compiles an inventory of components, also known as a Software Bill of Materials (SBOM). The SBOM is matched against a continuously updated vulnerability database to pinpoint security weaknesses.
