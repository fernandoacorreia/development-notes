# AI Agents

## Agent Protocols

- [A2A Protocol](https://a2a-protocol.org/latest/)
- [ACP - Agent Communication Protocol](https://agentcommunicationprotocol.dev/introduction/welcome) (ACP is now part of A2A under the Linux Foundation)
- [ACP - Agentic Commerce Protocol](https://developers.openai.com/commerce/)
- [MCP, A2A, ACP: What does it all mean?](https://akka.io/blog/mcp-a2a-acp-what-does-it-all-mean)

## Model alloys

Model alloys are a technique for improving the performance of agentic AI systems by mixing different large language models within a single task loop. Instead of relying on one model to handle all reasoning steps, an alloy alternates between two or more models — for example, Anthropic’s Sonnet and Google’s Gemini — allowing each to contribute its unique strengths at different stages of problem solving. The models remain unaware that they are collaborating, creating a seamless fusion of perspectives that yields better results than any model alone. Model alloys are especially effective for complex, multi-step tasks that require diverse insights or creative leaps, such as vulnerability detection, research, or iterative reasoning, where combining different cognitive “styles” can lead to stronger, more consistent performance.

Source: [Agents Built From Alloys](https://xbow.com/blog/alloy-agents/)

## Deep Agents

[https://www.philschmid.de/agents-2.0-deep-agents](https://www.philschmid.de/agents-2.0-deep-agents)

The architecture of AI agents is evolving from "Shallow Agents" (Agent 1.0) to "Deep Agents" (Agent 2.0). The initial Agent 1.0 model relies on a simple, reactive loop where the LLM's context window serves as its entire state. While this design is effective for short, transactional tasks requiring a small number of steps, it quickly fails when faced with complex problems demanding hours or days of work. These systems are prone to context overflow, where tool outputs displace original instructions, leading to a loss of the main goal and an inability to recover from errors or infinite loops.

The shift to Agents 2.0, or Deep Agents, represents an architectural move from reactive loops to proactive, robust systems capable of tackling complex, multi-step problems. Deep Agents achieve this by decoupling planning from execution, managing memory external to the LLM’s context window, and utilizing a layered system of specialized components. This approach is fundamentally about better engineering around the model to control context and, by extension, complexity, enabling the system to sustain hours- or days-long tasks.

The Deep Agent architecture is built on four core pillars:

- Explicit Planning: The agent uses tools to create and maintain an explicit To-Do list plan, which is reviewed and updated after every step or failure.

- Hierarchical Delegation: An Orchestrator delegates specialized tasks to focused Sub-Agents, which execute their own tool-call loops and return only the synthesized final answer.

- Persistent Memory: External sources like filesystems or vector databases are used as the source of truth for intermediate results, shifting the paradigm from "remembering everything" to "knowing where to find information."

- Extreme Context Engineering: Agents rely on thousands of tokens of detailed instructions defining protocols for planning, delegation, tool use, and collaboration standards.
