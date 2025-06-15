# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Agent Development Kit (ADK) - Google's open-source Python framework for building, evaluating, and deploying AI agents. It's a modular, code-first toolkit optimized for Gemini and the Google ecosystem but designed to be model-agnostic and deployment-agnostic.

## Common Development Commands

### Environment Setup
```bash
# Install with development dependencies
uv sync --all-extras

# For testing only (recommended for accurate test reproduction)
uv sync --extra test --extra eval

# Install from local wheel
uv build
pip install dist/google_adk-<version>-py3-none-any.whl
```

### Code Quality & Formatting
```bash
# Auto-format all code (imports + style)
./autoformat.sh

# Individual formatting commands
isort src/ tests/ contributing/     # Import sorting
pyink --config pyproject.toml src/  # Code formatting (Google style)

# Type checking and linting
mypy src/                           # Type checking (excludes tests/)
pylint src/                         # Linting (Google Python style guide)
```

### Testing
```bash
# Run all unit tests
pytest tests/unittests/

# Run integration tests
pytest tests/integration/

# Run specific test file
pytest tests/unittests/test_<module>_<feature>.py

# Run tests with parallel execution
pytest -n auto tests/unittests/
```

### CLI Usage
```bash
# ADK CLI help
adk --help

# Evaluate an agent
adk eval <agent_path> <eval_set>

# Launch development web UI
adk web
```

## Architecture Overview

### Core Package Structure
- **`src/google/adk/agents/`** - Agent implementations (LlmAgent, BaseAgent, parallel/sequential orchestration)
- **`src/google/adk/tools/`** - Rich ecosystem of pre-built tools (BigQuery, Google APIs, OpenAPI, MCP)
- **`src/google/adk/models/`** - LLM integrations (Gemini, Anthropic, LiteLLM support)
- **`src/google/adk/flows/`** - LLM conversation flows and processing logic
- **`src/google/adk/memory/`** - Memory service implementations for agent state
- **`src/google/adk/sessions/`** - Session management across agent interactions
- **`src/google/adk/evaluation/`** - Agent evaluation framework and metrics
- **`src/google/adk/cli/`** - Command-line interface and web development UI

### Key Design Patterns
- **Multi-Agent Systems**: Agents can have `sub_agents` for hierarchical orchestration
- **Tool Integration**: Flexible tool system supporting custom functions, OpenAPI specs, and existing frameworks
- **Model Abstraction**: Unified interface across different LLM providers
- **Code-First**: Agents defined directly in Python code for maximum flexibility
- **Evaluation-Driven**: Built-in evaluation framework for agent performance testing

### Testing Architecture
- **Unit Tests**: Located in `tests/unittests/` with comprehensive coverage
- **Integration Tests**: Located in `tests/integration/` with fixture agents
- **Parameterized Testing**: Tests run against both Google AI and Vertex AI backends
- **Environment Setup**: `conftest.py` files handle test environment configuration

### Configuration Files
- **`pyproject.toml`** - Primary configuration (dependencies, build system, tool configs)
- **`pylintrc`** - Google Python style guide enforcement
- **`autoformat.sh`** - Automated code formatting script

## Development Workflow

### Code Style Requirements
- Follow Google Python style guide (enforced via pylint and pyink)
- Use isort for import organization
- 80-character line length limit
- 2-space indentation for Python (pyink configuration)
- Type hints required (mypy type checking enabled)

### Testing Requirements
- All new features must include unit tests in `tests/unittests/`
- Integration tests should be added for end-to-end workflows
- Tests should cover both Google AI and Vertex AI backends where applicable
- Use pytest framework with async support enabled

### Build and Packaging
- Uses flit as build backend
- Package name: `google-adk`
- Supports Python 3.9+ (3.11+ recommended)
- Weekly release cadence via PyPI

## Extension Points

The ADK is designed for extensibility:
- **Custom Tools**: Implement tools by extending base tool classes
- **Custom Agents**: Extend `BaseAgent` or `LlmAgent` for specialized behavior
- **Custom Models**: Implement model providers via the unified model interface
- **Custom Memory**: Implement memory services for different storage backends
- **Custom Evaluation**: Create evaluation metrics and datasets for your use cases