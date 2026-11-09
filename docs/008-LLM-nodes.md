# LLM Nodes

**LLM Nodes** are dedicated servers designed to run Large Language Models (LLMs) locally, reducing our dependency on external AI providers such as OpenAI. 

Their primary purpose is to handle AI-powered tasks through self-hosted inference, helping us reduce API costs.

## Installation

```bash
export OPSLIB=~/code1/secret/production && \
saas install --node=ai01 --root && \
saas deploy --node=ai01 --root && \
saas start --node=ai01 --root
```

## Accessing 

[http://ai01.connectionsphere.com:3000](http://ai01.connectionsphere.com:3000)
