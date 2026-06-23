# New Feature: Real-time Multi-Agent Collaboration

## Overview
We have introduced **real-time collaboration** between multiple autonomous agents within the MultiAgent framework. This feature enables agents to share state, exchange messages instantly, and coordinate actions without the need for external orchestration.

## Key Benefits
- **Improved Efficiency**: Agents can react to each other's outputs instantly, reducing latency.
- **Scalable Coordination**: Supports dynamic addition/removal of agents during runtime.
- **Simplified Development**: Developers can focus on individual agent logic while the framework handles communication.

## How It Works
1. **Shared Context**: A central `Context` object is introduced, accessible by all agents. It stores shared variables and a message queue.
2. **Message Bus**: Agents publish messages to the bus; subscribers receive them in real‑time.
3. **Synchronization Primitives**: Lightweight locks and events ensure thread‑safe interactions.

## Usage Example
```python
from multiagent import Agent, Context

# Create a shared context
ctx = Context()

# Define two agents that communicate via the context
class Producer(Agent):
    def run(self):
        for i in range(5):
            self.context.publish('data', i)
            self.sleep(1)

class Consumer(Agent):
    def on_message(self, topic, payload):
        if topic == 'data':
            print(f"Received: {payload}")

producer = Producer(context=ctx)
consumer = Consumer(context=ctx)

producer.start()
consumer.start()
```

## Configuration
- `context.max_queue_size` – maximum number of pending messages.
- `agent.sync_interval` – how often agents check the message bus (default: 100 ms).

## Future Work
- Distributed context support across multiple machines.
- Advanced routing rules for selective message delivery.

---
*Documentation generated on $(date).*
