# Node.js

## Modern Node.js Patterns for 2025

Source: [kashw1n.com/blog/nodejs-2025](https://kashw1n.com/blog/nodejs-2025/)

### Module System: ESM as the New Standard
- Move away from CommonJS toward ES Modules.
- Use `node:` prefix for built-in modules to avoid confusion.
- Top-level `await` simplifies initialization.

### Built-in Web APIs
- Native Fetch API replaces Axios / node-fetch.
- Built-in timeout and cancellation via `AbortSignal.timeout`.
- Use `AbortController` for graceful cancellation across APIs.

### Built-in Testing
- Node.js Test Runner: `node --test`, watch mode, coverage support.

### Advanced Async Patterns
- Stronger error handling with async/await.
- Use `Promise.all()` for parallel operations.
- AsyncIterators with event emitters for stream/event processing.

### Streams & Web Standards Interoperability
- Modern stream pipelines using `node:stream/promises` and `pipeline`.
- Interoperability between Node.js Streams and Web Streams (`Readable.fromWeb` / `toWeb`).

### Worker Threads for Parallelism
- Use Worker Threads for CPU-intensive tasks without blocking the main thread.

### Enhanced Developer Experience
- Built-in `--watch` mode.
- `.env` support via `--env-file`.
- Reduces need for tools like nodemon or dotenv.

### Security & Performance Monitoring
- Experimental permission model (restrict fs, network access, etc.).
- Built-in performance monitoring via `node:perf_hooks` and `PerformanceObserver`.
- Diagnostics channel for custom diagnostic events.

### Application Distribution & Packaging
- Bundle Node.js apps into a single executable (experimental SEA configuration).

### Modern Package Management & Module Resolution
- Import maps for internal module resolution (e.g., aliases like `#utils/*`).
- Dynamic imports for conditional loading and code splitting.

### Key Takeaways for 2025
- Embrace web standards (fetch, AbortController, Web Streams).
- Leverage built-in tools (testing, watch mode, env support).
- Use modern async patterns (top-level await, structured errors, async iterators).
- Use worker threads strategically.
- Adopt progressive enhancement (permissions, diagnostics, performance monitoring).
- Optimize for developer experience.
- Plan for distribution (single executable, modern packaging).
