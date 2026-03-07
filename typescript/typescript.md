# TypeScript

## Task workers

### Graphile Worker

High performance Node.js/PostgreSQL job queue.

Highlights: cleaner task abstraction, faster job wake-up, fewer tuning knobs.

Worker requires a direct DB connection, without pgbouncer.

- https://github.com/graphile/worker
- http://worker.graphile.org/

### pg-boss

Queueing jobs in Postgres from Node.js.

pg-boss is a job queue built in Node.js on top of PostgreSQL in order to provide background processing and reliable asynchronous execution to Node.js applications.

complex queue management: many queues, rate limits, throttling, distributed job orchestration.

- https://github.com/timgit/pg-boss
- https://timgit.github.io/pg-boss/

### BullMQ

Message Queue and Batch processing for NodeJS, Python, Elixir and PHP based on Redis.

- https://bullmq.io/
- https://github.com/taskforcesh/bullmq
