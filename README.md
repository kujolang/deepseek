# Kujo DeepSeek Provider

[![Version](https://img.shields.io/badge/version-0.1.1-black)](https://github.com/kujolang/deepseek/tree/v0.1.1)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)
[![built with Kujo](https://img.shields.io/badge/built%20with-Kujo-white.svg)](https://github.com/kujolang/kujo)

DeepSeek chat and reasoning models for Kujo 1.5.0 or later through the compatible API.

## Install

The current package requires Kujo 1.6.0 or newer.

```bash
kujo run /path/to/kennel/kennel.kujo \
  -- add github:kujolang/deepseek@v0.1.1 \
  --alias deepseek
kujo run /path/to/kennel/kennel.kujo -- install
export DEEPSEEK_API_KEY=your-key
```

## 30-second quick start

```kujo
from deepseek import create_client, client_chat

client := create_client({})
request := {
    "model": "deepseek-flash",
    "messages": [
        {
            "role": "user",
            "content": "Hello from Kujo!"
        }
    ]
}

result := client_chat(client, request)

print(result["data"]["choices"][0]["message"]["content"])
```

## Native API

The native layer handles DeepSeek chat, model discovery, reasoning controls, tools, JSON output and usage metadata. For a native chat request with `stream: true`, successful results include buffered `events`. The standalone parser also reports whether it encountered the completion marker.

## AI SDK integration

`deepseek_provider({"model": "deepseek-flash"})` supplies normalized chat and streaming semantics through the compatible driver. Use the current model name `deepseek-flash` for new configurations. When continuing a thinking-mode conversation with tools, pass the provider's `reasoning_content` on assistant turns along with the tool messages. [DeepSeek thinking mode](https://api-docs.deepseek.com/guides/thinking_mode/)

## Authentication and security

Set `DEEPSEEK_API_KEY`. Native requests require HTTPS and disable redirects. The native client filters the configured bearer value from diagnostics and raw responses.

## Testing and documentation

```bash
bash scripts/release_quality_gate.sh
bash scripts/verify_installed_package.sh
```

The default gate is deterministic and offline. See [docs/](docs/) for implementation and Contract v1 evidence.
