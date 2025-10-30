# AgentAI

AgentAI is a modular, extensible **Python-based agent framework** for building autonomous assistants and headless automation agents. It provides a lightweight architecture to combine planning, tools, memory, and LLM-driven reasoning so you can create agents that perform real-world tasks, integrate with APIs, and be extended with custom tools.

---

## Table of Contents

* [Features](#features)
* [Requirements](#requirements)
* [Installation](#installation)
* [Quick Start](#quick-start)
* [Usage Examples](#usage-examples)
* [Configuration](#configuration)
* [Architecture & Key Modules](#architecture--key-modules)
* [Extending with Tools & Skills](#extending-with-tools--skills)
* [Testing](#testing)
* [Contributing](#contributing)
* [License](#license)
* [Acknowledgements](#acknowledgements)

---

## Features

* Lightweight agent orchestration driven by LLM prompts or your own reasoning loop.
* Plugin-style tools (HTTP clients, file I/O, web scraping, database connectors, etc.).
* Simple memory components to store short-term and long-term state.
* Task planning and step-by-step action execution.
* Configurable logging and telemetry for debugging.

---

## Requirements

* Python 3.10+ (3.11 recommended)
* pip

Python packages (example):

```text
openai==0.27.0
requests
python-dotenv
pytest
sqlalchemy  # optional if using persistent memory
```

> Adjust versions to match your project's `pyproject.toml` or `requirements.txt`.

---

## Installation

1. Clone the repo:

```bash
git clone https://github.com/<your-org>/agentai.git
cd agentai
```

2. Create a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate  # or .\venv\Scripts\activate on Windows
pip install -r requirements.txt
```

3. Add environment variables (example `.env`):

```env
OPENAI_API_KEY=sk-...
AGENTAI_DEFAULT_MODEL=gpt-4o-mini
```

---

## Quick Start

Start a simple agent that answers user questions using the configured LLM tool.

```bash
python examples/run_simple_agent.py
```

Or run the interactive REPL-style agent:

```bash
python -m agentai.repl
```

---

## Usage Examples

### 1. Programmatic

```python
from agentai import Agent, Tool

# create an agent with a simple echo tool
agent = Agent(name="MyAgent")
agent.register_tool(Tool("echo", lambda text: text))

response = agent.handle("Say hello and echo back the phrase 'hi'")
print(response)
```

### 2. Custom Tool

Create a tool that fetches weather from an API and register it with the agent.

```python
from agentai.tools import HTTPTool

weather_tool = HTTPTool("https://api.weather.example/forecast")
agent.register_tool(weather_tool)
```

---

## Configuration

Configuration is environment-driven. Typical options:

* `OPENAI_API_KEY` – API key for LLM provider.
* `AGENTAI_DEFAULT_MODEL` – default LLM model to use for reasoning.
* `AGENTAI_MAX_TOKENS` – token limit for generation.
* `AGENTAI_LOG_LEVEL` – `DEBUG`, `INFO`, `WARN`, `ERROR`.

You can also override configuration at runtime using the `AgentConfig` object.

---

## Architecture & Key Modules

```
agentai/
├─ core/           # Agent orchestration and runtime loop
├─ tools/          # Prebuilt tools (HTTP, file, db, shell)
├─ memory/         # Memory implementations (in-memory, sqlite)
├─ prompts/        # Prompt templates and prompt utils
├─ integrations/   # Third-party connectors
├─ examples/       # Example agents and scripts
├─ tests/          # Unit and integration tests
```

* `Agent` — main entrypoint. Manages tools, memory, planner, and LLM calls.
* `Tool` — abstract class representing an external capability (I/O, API, internal function).
* `Planner` — breaks complex tasks into smaller steps and chooses tools/actions.
* `Memory` — stores context and conversation history.
* `LLMClient` — wrapper for OpenAI or other LLM providers.

---

## Extending with Tools & Skills

To add a new tool:

1. Create a subclass of `agentai.tools.base.ToolBase` (or implement the expected interface).
2. Implement `run(...)` that accepts input and returns output.
3. Register the tool on agent instantiation:

```python
my_tool = MyNewTool("name", config={})
agent.register_tool(my_tool)
```

For more advanced skills, implement a `Skill` that coordinates multiple tools.

---

## Testing

Run unit tests with pytest:

```bash
pytest -q
```

Tips:

* Use dependency injection for LLM clients to run tests without actual API calls.
* Provide fixtures that mock tool outputs.

---

## Contributing

1. Fork the repository.
2. Create a feature branch `feature/your-change`.
3. Make your changes and add tests.
4. Open a Pull Request describing the purpose and design.

Be sure to follow the coding style and run tests locally before submitting.

---

## License

This project is released under the MIT License. See `LICENSE` for details.

---

## Acknowledgements

Built with inspiration from open-source agent frameworks and LLM toolkits. Thanks to the contributors and the open-source community.

---

If you'd like, I can also:

* produce a `requirements.txt` or `pyproject.toml` for this project,
* generate example scripts (REPL, HTTP server) that show how to run the agent,
* or tailor the README to a specific LLM provider (OpenAI, Anthropic, etc.).
