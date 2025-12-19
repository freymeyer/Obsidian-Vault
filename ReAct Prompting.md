All that was mentioned is possible due to the fact that agentic AI uses  chain-of-thought (CoT) reasoning to improve its ability to perform complex, multi-step tasks autonomously. CoT is a prompt-engineering method designed to improve the reasoning capabilities of [[Large Language Models (LLMS)]], especially for tasks that require complex, multi-step thinking. 

[[Chain-of-thought (CoT)]] prompting demonstrated that large language models can generate explicit reasoning traces to solve tasks requiring arithmetic, logic, and common-sense reasoning. However, CoT has a critical limitation: because it operates in isolation, without access to external knowledge or tools, it often suffers from fact hallucination, outdated knowledge, and error propagation.

ReAct (Reason + Act) addresses this limitation by unifying reasoning and acting within the same framework. Instead of producing only an answer or a reasoning trace, a ReAct-enabled LLM alternates between:

- Verbal reasoning traces: Articulating its current thought process.
- Actions: Executing operations in an external environment (e.g., searching Wikipedia, querying an API, or running code).

This allows the model to:

- Dynamically plan and adapt: Updating its strategy as new observations come in.
- Ground reasoning in reality: Pulling in external knowledge to reduce hallucinations.
- Close the loop between thought and action: Much like humans, who reason about what to do, act, observe the outcome, and refine their next steps.