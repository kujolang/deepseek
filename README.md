# Kujo DeepSeek Provider

[![Version](https://img.shields.io/badge/version-0.1.1-black)](https://github.com/kujolang/deepseek/releases/tag/v0.1.1)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)
[![built with Kujo](https://img.shields.io/badge/built%20with-Kujo-white.svg)](https://github.com/kujolang/kujo)

DeepSeek chat and reasoning models for Kujo through the compatible API.

## Install

```bash
kujo run /path/to/kennel/kennel.kujo --interpreter -- add github:kujolang/deepseek@v0.1.1 --alias deepseek
kujo run /path/to/kennel/kennel.kujo --interpreter -- install
export DEEPSEEK_API_KEY=your-key
```

## 30-second quick start

```kujo
from deepseek import create_client, client_chat
client := create_client({})
result := client_chat(client, {"model":"deepseek-chat","messages":[{"role":"user","content":"Hello from Kujo!"}]})
print(result["data"]["choices"][0]["message"]["content"])
```

## Native API

The native layer preserves DeepSeek response fields, reasoning controls, tools, JSON output, streaming, and usage metadata.

## AI SDK integration

`deepseek_provider({"model": "deepseek-chat"})` supplies normalized chat and streaming semantics through the compatible driver. The existing AI SDK preset remains supported.

## Authentication and security

Set `DEEPSEEK_API_KEY`. Remote endpoints require HTTPS; embedded credentials, header injection, and secret leakage are rejected.

## Testing and documentation

```bash
bash scripts/release_quality_gate.sh
bash scripts/verify_installed_package.sh
```

The default gate is deterministic and offline. See [docs/](docs/) for implementation and Contract v1 evidence.
