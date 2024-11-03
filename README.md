# Reliability Issue/Version Problems in LlamaIndex

* https://github.com/run-llama/llama_index/issues/16774

Due to terrible documentation and a severe lack of optimization for developers, which I elaborated on in [LangChain-ToolDocs-Problem](https://github.com/hherpa/LangChain-ToolDocs-Problem), and while waiting for a response to my [issue](https://github.com/langchain-ai/langchain/issues/27668), I had to switch from LangChain to LlamaIndex. However, this immediately presented difficulties: the first three lines of the [documentation](https://llamahub.ai/l/llms/llama-index-llms-fireworks?from=llms)—two lines for installing the library and the line `from llama_index.llms.fireworks import Fireworks`—led me straight to two errors.

In the case of LangChain, the reasons for creating an issue were the need to manually process 98 pages of documentation dedicated to integrating various tools with the LLM agent, as well as API provider platforms (instead of a convenient table like in other sections of the documentation or at least more templated code like in Llama_Index). Here, the problem turned out to be that errors occur "out of the box," literally in the first lines of code after installation.

* I am more interested not so much in solving the specific errors I encountered but in the likelihood of user-side errors occurring in the future. It is expected that by following the documentation instructions, a developer should be able to run the project effortlessly. However, the presence of errors at such an early stage of using the library raises doubts about its stability and reliability.

**Brief Description of the Bug (you can see the full picture in Notebook №1-2):** I am using the latest version of the `llama-Index` and `llama-index-llms-fireworks` libraries, but when I try to access `from llama_index.llms.fireworks import Fireworks`, I encounter two import errors: `cannot import name 'global_handler' from 'llama_index.core' (unknown location)` and `ImportError: cannot import name 'Secret' from 'pydantic'`.

1. **[Notebook](https://github.com/hherpa/LlamaIndex-Reliability-Issues/blob/main/cannot_import_name_Secret_from_pydantic.ipynb) with the first error**
2. **[Notebook](https://github.com/hherpa/LlamaIndex-Reliability-Issues/blob/main/cannot_import_name_global_handler_from_llama_index.ipynb) with the second error**

Reinstalling Jupyter and the following set of libraries:

```
llama-index-llms-fireworks==0.2.2
llama-cloud==0.1.4
llama-index==0.11.21
llama-index-agent-openai==0.3.4
llama-index-cli==0.3.1
llama-index-core==0.11.21
llama-index-embeddings-openai==0.2.5
llama-index-indices-managed-llama-cloud==0.4.0
llama-index-legacy==0.9.48.post3
llama-index-llms-fireworks==0.2.2
llama-index-llms-openai==0.2.16
llama-index-multi-modal-llms-openai==0.2.3
llama-index-program-openai==0.2.0
llama-index-question-gen-openai==0.2.0
llama-index-readers-file==0.2.2
llama-index-readers-llama-parse==0.3.0
llama-parse==0.5.12
```

a new error has appeared:

```python
-----------------------------------------------------------------------
ValueError                            Traceback (most recent call last)
Cell In[1], line 6
      1 from llama_index.llms.fireworks import Fireworks
      3 llm = Fireworks(
      4     model="accounts/fireworks/models/firefunction-v1", api_key="My api key was here"
      5 )
----> 6 resp = Fireworks().complete("Paul Graham is ")
      7 print(resp)

File ~\AppData\Roaming\Python\Python311\site-packages\llama_index\llms\fireworks\base.py:60, in Fireworks.__init__(self, model, temperature, max_tokens, additional_kwargs, max_retries, api_base, api_key, callback_manager, default_headers, system_prompt, messages_to_prompt, completion_to_prompt, pydantic_program_mode, output_parser)
     57 callback_manager = callback_manager or CallbackManager([])
     59 api_base = get_from_param_or_env("api_base", api_base, "FIREWORKS_API_BASE")
---> 60 api_key = get_from_param_or_env("api_key", api_key, "FIREWORKS_API_KEY")
     62 super().__init__(
     63     model=model,
     64     temperature=temperature,
   (...)
     76     output_parser=output_parser,
     77 )

File ~\AppData\Roaming\Python\Python311\site-packages\llama_index\core\base\llms\generic_utils.py:311, in get_from_param_or_env(key, param, env_key, default)
    309     return default
    310 else:
--> 311     raise ValueError(
    312         f"Did not find {key}, please add an environment variable"
    313         f" `{env_key}` which contains it, or pass"
    314         f"  `{key}` as a named parameter."
    315     )

ValueError: Did not find api_key, please add an environment variable FIREWORKS_API_KEY which contains it, or pass  api_key as a named parameter.
```

it turns out that the code itself in the documentation (and on several pages) llama_index had an error, but no one noticed it.
* **PR & Fix:** https://github.com/run-llama/llama_index/pull/16794
