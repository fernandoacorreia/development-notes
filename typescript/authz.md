# TypeScript Authorization

## cerbos-sdk-javascript

JavaScript SDK for authorization via Cerbos.

Cerbos is the open core, language-agnostic, scalable authorization solution that makes user permissions and authorization simple to implement and manage by writing context-aware access control policies for your application resources.

At runtime, a TypeScript backend typically does **authentication itself** first, then builds a Cerbos authorization request from the authenticated principal, the target resource, the action, and any extra context; it sends that to a **Cerbos PDP** using the JS/TS SDK (`@cerbos/grpc` is the recommended transport for Node backends, while Cerbos also exposes HTTP). The PDP evaluates the request against your versioned YAML policies and returns an allow/deny decision plus optional metadata, and your backend remains the **policy enforcement point** that decides whether to proceed with the business operation. ([Cerbos][1])

The common deployment topology is to run Cerbos as a **separate stateless service** alongside your app—often as a **sidecar per service instance** to minimize latency, though it can also be a shared service—and because the PDP is stateless it scales horizontally while policies can change independently of application redeploys. Cerbos also has an **embedded ePDP** option for in-process local evaluation via WebAssembly, but that is a distinct architecture from the usual backend-to-PDP network call model. ([Cerbos Docs][2])

A minimal mental model is: **client → TS backend authenticates user → backend calls Cerbos PDP for `can this principal do this action on this resource in this context?` → PDP returns decision → backend enforces it before touching the protected data or operation.** ([Cerbos][1])

[1]: https://www.cerbos.dev/ecosystem/javascript?utm_source=chatgpt.com "Authorization for JavaScript and TypeScript applications - Cerbos"
[2]: https://docs.cerbos.dev/cerbos/latest/what-is-cerbos.html?utm_source=chatgpt.com "What is Cerbos? :: Cerbos // Documentation"

- https://github.com/cerbos/cerbos-sdk-javascript
- https://cerbos.dev/
- https://github.com/cerbos/cerbos
