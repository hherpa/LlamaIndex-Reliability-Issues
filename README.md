# Reliability Issue/Version Problems in LlamaIndex

Due to terrible documentation and a severe lack of optimization for developers, which I elaborated on in [LangChain-ToolDocs-Problem](https://github.com/hherpa/LangChain-ToolDocs-Problem), and while waiting for a response to my [issue](https://github.com/langchain-ai/langchain/issues/27668), I had to switch from LangChain to LlamaIndex. However, this immediately presented difficulties: the first three lines of the [documentation](https://llamahub.ai/l/llms/llama-index-llms-fireworks?from=llms)—two lines for installing the library and the line `from llama_index.llms.fireworks import Fireworks`—led me straight to two errors.

In the case of LangChain, the reasons for creating an issue were the need to manually process 98 pages of documentation dedicated to integrating various tools with the LLM agent, as well as API provider platforms (instead of a convenient table like in other sections of the documentation or at least more templated code like in Llama_Index). Here, the problem turned out to be that errors occur "out of the box," literally in the first lines of code after installation.

* I am more interested not so much in solving the specific errors I encountered but in the likelihood of user-side errors occurring in the future. It is expected that by following the documentation instructions, a developer should be able to run the project effortlessly. However, the presence of errors at such an early stage of using the library raises doubts about its stability and reliability.

1. **[Notebook](https://github.com/hherpa/LlamaIndex-Reliability-Issues/blob/main/cannot_import_name_Secret_from_pydantic.ipynb) with the first error**
2. **[Notebook](https://github.com/hherpa/LlamaIndex-Reliability-Issues/blob/main/cannot_import_name_global_handler_from_llama_index.ipynb) with the second error**
