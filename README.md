# AI Chatbot and Research Assistant with LangChain, Groq, and Tavily

This project is an advanced AI chatbot and research assistant, built using the powerful integration of LangChain, Groq API, Tavily search engine, and LangGraph. The bot is capable of fetching real-time data, conducting research, and providing well-structured, context-aware answers. This makes it ideal for interactive and research-focused applications.

## Table of Contents

* [Overview](#overview)
* [Features](#features)
* [Tech Stack](#tech-stack)
* [Installation](#installation)
* [Environment Variables](#environment-variables)
* [Usage](#usage)
* [Memory Functionality](#memory-functionality)
* [Visualization](#visualization)
* [Contributing](#contributing)
* [License](#license)

## Overview

This project showcases an AI-driven chatbot capable of advanced research using LangChain, Groq model integration, and the Tavily API. It supports context-aware interaction through a memory mechanism that retains chat history to provide coherent and continuous conversations.

## Features

* **LLM Initialization** : Utilizes Groq's Llama-3.1-8B-Instant model.
* **Real-time Data Retrieval** : Integrates Tavily API to search and fetch live data.
* **Graph-based Workflow** : Employs LangGraph for building and visualizing state graphs.
* **Tool Integration** : Custom tools for searching and data extraction.
* **Interactive Chat Interface** : Supports user interaction with memory-based context preservation.
* **Memory Functionality** : Stores chat history to maintain context across multiple queries.

## Tech Stack

* **Python**
* **LangChain** : Framework for developing applications with LLMs.
* **Groq API** : For accessing Groq LLMs.
* **Tavily API** : Web search and content retrieval.
* **LangGraph** : Constructing and visualizing graphs for agent workflows.
* **Google Colab** (for development and testing)

## Installation

Follow the steps below to set up and run the project locally.

### Dependencies

Ensure you have Python installed. Then, install the required libraries:

```
!pip install -q langchain langchain_community langchain_huggingface langgraph langchain-groq langgraph-checkpoint-sqlite
!pip install -q tavily-python
```

### Additional Dependencies (if required)

```
!pip install -qU langchain-community
```

## Environment Variables

To run this project, set up the following environment variables:

1. `GROQ_API_KEY`: Your API key for Groq LLM.
2. `TAVILY_API_KEY`: Your API key for Tavily.

Add these keys securely using `userdata.get()` or other secure storage methods:

```
import os
from google.colab import userdata

os.environ["GROQ_API_KEY"] = userdata.get("GROQ_API_KEY")
os.environ["TAVILY_API_KEY"] = userdata.get("TAVILY_API_KEY")
```

## Usage

### Initialize the LLM

```
from langchain_groq import ChatGroq
llm = ChatGroq(model_name="Llama-3.1-8b-Instant")
```

### Invoke Simple Queries

```
import pprint
response = llm.invoke("Who is Imran Khan")
pprint.pprint(response.content)
```

### Tavily API Search

```
from tavily import TavilyClient
client = TavilyClient(api_key=os.environ["TAVILY_API_KEY"])
response = client.search(query="Election 2024 Pakistan")
pprint.pprint(response['results'][1]['content'])
```

### Advanced Search with Q&A

```
response = client.search(
    query="Who is the director of Daredevil born again?",
    search_depth='advance',
    max_results=7,
    include_images=True,
    include_answer=True,
    include_raw_content=False
)
print(response["answer"])
```

## Memory Functionality

A unique aspect of this chatbot is its ability to save and recall chat history, allowing it to provide answers based on previous interactions. This functionality ensures a coherent conversation where the bot can understand context across multiple user queries.

### Implementation of Memory

The memory feature is implemented using LangChain's built-in memory management:

```
from langgraph.checkpoint.sqlite import SqliteSaver
from langgraph.checkpoint.memory import MemorySaver

memory = SqliteSaver.from_conn_string(':memory:')
graph = graph_builder.compile(checkpointer = MemorySaver()) # Adding Memory
```

This memory stores the chat history and enables the chatbot to retrieve context when answering follow-up questions. For example:

```
## Initial question

response = llm.invoke("Tell me about the Eiffel Tower.")
print(response.content)

### Follow-up question utilizing memory

response = llm.invoke("How tall is it?")
print(response.content)  # The bot responds with context from the previous question.
```

## Visualization

To visualize the agent graph:

```
from IPython.display import Image, display
display(Image(graph.get_graph().draw_mermaid_png()))
```

## Graph

![img]()

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your improvements.

## License

This project is licensed under the MIT License.
