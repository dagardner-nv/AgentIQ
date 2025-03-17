<!--
SPDX-FileCopyrightText: Copyright (c) 2024-2025, NVIDIA CORPORATION & AFFILIATES. All rights reserved.
SPDX-License-Identifier: Apache-2.0

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http:/www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

![NVIDIA AgentIQ](./docs/source/_static/agentiq_banner.png "AgentIQ banner image")

# NVIDIA AgentIQ

AgentIQ is a flexible library designed to seamlessly integrate your enterprise agents—regardless of framework—with various data sources and tools. By treating agents, tools, and agentic workflows as simple function calls, AgentIQ enables true composability: build once and reuse anywhere.

## Key Features

- [**Framework Agnostic:**](https://docs.nvidia.com/agentiq/latest/concepts/plugins.html) Works with any agentic framework, so you can use your current technology stack without replatforming.
- [**Reusability:**](https://docs.nvidia.com/agentiq/latest/guides/sharing_workflows_and_tools.html) Every agent, tool, or workflow can be combined and repurposed, allowing developers to leverage existing work in new scenarios.
- [**Rapid Development:**](https://docs.nvidia.com/agentiq/latest/guides/create_customize_workflows.html) Start with a pre-built agent, tool, or workflow, and customize it to your needs.
- [**Profiling:**](https://docs.nvidia.com/agentiq/latest/guides/profiler.html) Profile entire workflows down to the tool and agent level, track input/output tokens and timings, and identify bottlenecks.
- [**Observability:**](https://docs.nvidia.com/agentiq/latest/guides/observe_workflow_with_phoenix.html) Monitor and debug your workflows with any OpenTelemetry-compatible observability tool.
- [**Evaluation System:**](https://docs.nvidia.com/agentiq/latest/guides/evaluate.html) Validate and maintain accuracy of agentic workflows with built-in evaluation tools.
- [**User Interface:**](https://docs.nvidia.com/agentiq/latest/guides/use_aiq_serve.html) Use the AgentIQ UI chat interface to interact with your agents, visualize output, and debug workflows.
- [**MCP Compatibility**](https://docs.nvidia.com/agentiq/latest/components/mcp.html) Compatible with Model Context Protocol (MCP), allowing tools served by MCP Servers to be used as AgentIQ functions.

With AgentIQ, you can move quickly, experiment freely, and ensure reliability across all your agent-driven projects.

## Links

 * [Documentation](https://docs.nvidia.com/agentiq/latest/index.html): Explore the full documentation for AgentIQ.
 * [About AgentIQ](https://docs.nvidia.com/agentiq/latest/intro/why_aiq.html): Learn more about the benefits of using AgentIQ.
 * [Get Started Guide](https://docs.nvidia.com/agentiq/latest/intro/get_started.html): Set up your environment and start building with AgentIQ.
 * [Examples](https://github.com/NVIDIA/AgentIQ/tree/main/examples#readme): Explore examples of AgentIQ workflows.
 * [Create and Customize AgentIQ Workflows](https://docs.nvidia.com/agentiq/latest/guides/create_customize_workflows.html): Learn how to create and customize AgentIQ workflows.
 * [Evaluate with AgentIQ](https://docs.nvidia.com/agentiq/latest/guides/evaluate.html): Learn how to evaluate your AgentIQ workflows.
 * [Troubleshooting](https://docs.nvidia.com/agentiq/latest/troubleshooting.html): Get help with common issues.


## Get Started

1. Ensure you have Python 3.12, and a Python development environment.

   Assuming Python 3.12 is installed, create a virtual environment and activate it with:
   ```bash
   python -m venv env
   source env/bin/activate
   ```

2. Install AgentIQ with support for your desired LLM framework

   ```bash
   pip install agentiq[<your framework>]
   ```

   For example, to install AgentIQ with support for the LangChain framework (which is necessary for the Hello World example), use the following command:

   ```bash
   pip install agentiq[langchain]
   ```

   > [!NOTE]
   > AgentIQ also supports other LLM frameworks. Refer to the [plugin guide](https://docs.nvidia.com/agentiq/latest/concepts/plugins.md) for more information.


3. Verify the installation using the AgentIQ CLI

   ```bash
   aiq --version
   ```

   This should output the AgentIQ version which is currently installed.

## Hello World Example

1. Ensure you have set the `NVIDIA_API_KEY` environment variable to allow the example to use NVIDIA NIMs. An API key can be obtained by visiting [`build.nvidia.com`](https://build.nvidia.com/) and creating an account.

   ```bash
   export NVIDIA_API_KEY=<your_api_key>
   ```

2. Create the AgentIQ workflow configuration file. This file will define the agents, tools, and workflows that will be used in the example. Save the following as `workflow.yaml`:

   ```yaml
   functions:
      # Add a tool to search wikipedia
      wikipedia_search:
         _type: wiki_search
         max_results: 2

   llms:
      # Tell AgentIQ which LLM to use for the agent
      nim_llm:
         _type: nim
         model_name: meta/llama-3.1-70b-instruct
         temperature: 0.0

   workflow:
      # Use an agent that 'reasons' and 'acts'
      _type: react_agent
      # Give it access to our wikipedia search tool
      tool_names: [wikipedia_search]
      # Tell it which LLM to use
      llm_name: nim_llm
      # Make it verbose
      verbose: true
      # Retry parsing errors because LLMs are non-deterministic
      retry_parsing_errors: true
      # Retry up to 3 times
      max_retries: 3
   ```

3. Run the Hello World example using the `aiq` CLI and the `workflow.yaml` file.

   ```bash
   aiq run --config_file workflow.yaml --input "List five subspecies of Aardvarks"
   ```

   This will run the workflow and output the results to the console.

   ```console
   Workflow Result:
   ['Here are five subspecies of Aardvarks:\n\n1. Orycteropus afer afer (Southern aardvark)\n2. O. a. adametzi  Grote, 1921 (Western aardvark)\n3. O. a. aethiopicus  Sundevall, 1843\n4. O. a. angolensis  Zukowsky & Haltenorth, 1957\n5. O. a. erikssoni  Lönnberg, 1906']
   ```

## Feedback

We would love to hear from you! Please file an issue on [GitHub](https://github.com/NVIDIA/AgentIQ/issues) if you have any feedback or feature requests.

## Acknowledgements

We would like to thank the following open source projects that made AgentIQ possible:

- [CrewAI](https://github.com/crewAIInc/crewAI)
- [FastAPI](https://github.com/tiangolo/fastapi)
- [LangChain](https://github.com/langchain-ai/langchain)
- [Llama-Index](https://github.com/run-llama/llama_index)
- [Mem0ai](https://github.com/mem0ai/mem0)
- [Ragas](https://github.com/explodinggradients/ragas)
- [Semantic Kernel](https://github.com/microsoft/semantic-kernel)
- [uv](https://github.com/astral-sh/uv)
