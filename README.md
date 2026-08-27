# Kujo DeepSeek Provider

Native DeepSeek OpenAI-compatible chat client with DeepSeek reasoning, tools, vision, and an AI SDK adapter.

```bash
kujo package-add github:kujolang/deepseek@v0.1.0
export DEEPSEEK_API_KEY=your-key
```

```kujo
from deepseek import create_client, client_chat
c := create_client({})
r := client_chat(c, {"model":"deepseek-chat","messages":[{"role":"user","content":"Hello"}],"reasoning_effort":"high"})
```

Native use preserves DeepSeek response fields, reasoning controls, tools, and usage metadata. `deepseek_provider()` supplies normalized AI SDK chat and streaming semantics. Tests are offline and credential-free.
