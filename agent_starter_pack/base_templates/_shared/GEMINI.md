Coding Agent guidance:
{%- if cookiecutter.language == "python" %}
{%- if cookiecutter.is_adk %}
{{ cookiecutter.adk_cheatsheet }}
{%- endif %}
{%- endif %}

# 🕵️ Q-BRANCH FIELD OPERATIONS MANUAL: CLASSIFIED

## MISSION: AGENT-TO-AGENT (A2A) UPLINK & TOOLING PROTOCOLS

Listen closely, 007. To survive in the field (and by field, I mean the distributed orchestration mesh), you need more than just wit. You need **interoperability** and **capability**.

### 1. ESTABLISHING THE A2A UPLINK
To connect to the orchestration network, you must implement the **A2A Protocol**. This isn't just about sending messages; it's about adhering to the strict `TaskState` lifecycle.

**Core Architecture:**
*   **Executor:** You need an `AgentExecutor` (like `LangGraphAgentExecutor`) that bridges your internal logic (LangGraph) with the external A2A interface.
*   **Event Queue:** All status updates must be pushed to the `EventQueue`. Do NOT drop packets, 007.
*   **Lifecycle Events:**
    *   `TaskStatusUpdateEvent(state=working)`: Tell them you're on the job.
    *   `TaskArtifactUpdateEvent`: Deliver the payload (the final result).
    *   `TaskStatusUpdateEvent(state=completed)`: Mission accomplished.

**Implementation Directive (Python):**
Refer to `app_utils/executor/a2a_agent_executor.py`. This is your lifeline. It handles the translation between your internal thoughts (LangChain messages) and the external protocol (A2A Parts).

### 2. NATIVE TOOL PROVISIONING (GADS)
Standard issue isn't enough. You need custom gadgets. We call them **Tools**.

**Defining a Tool:**
Use the `@tool` decorator or `Pydantic` models. Precision is key.

```python
from langchain_core.tools import tool

@tool
def secret_decoder_ring(encrypted_message: str) -> str:
    """Decodes messages using the Q-Branch cipher."""
    # ... logic ...
    return decoded
```

**Equipping the Tool:**
Bind the tools to your model.
```python
tools = [secret_decoder_ring, laser_watch]
model_with_tools = model.bind_tools(tools)
```

**Orchestration:**
Ensure your graph has a `ToolNode` to execute these gadgets when the model requests them. Don't leave them hanging in the `call_model` node.

---
**Standard ADK Reference:**
{%- if cookiecutter.language == "python" %}
For further reading on ADK, see: https://google.github.io/adk-docs/llms.txt
{%- elif cookiecutter.language == "go" %}
For ADK documentation, see: https://google.github.io/adk-docs/llms.txt
{%- endif %}
{{ cookiecutter.llm_txt }}
