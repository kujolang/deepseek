# DeepSeek provider hardening

Reviewed `main` from `033935514114488a91c74865ff9714cf4440d97a`. Verified implementation at `78b2633584c3d464429c77c498801f15bdc565a4`. The package provides native DeepSeek chat and model discovery with an AI SDK Provider Contract v1 driver. The immutable SDK dependency remains `v1.1.0`.

The review covered native request and response boundaries, provider hooks, thinking-mode continuations, streaming completion, cache accounting, source and installed consumers, docs, fixtures and package reproducibility inputs. The provider has no persistent state, executable tools, subprocesses in its runtime or background jobs. Four baseline checks passed. Five additional contract cases failed against the baseline and passed after the fixes.

| Priority | Finding | Fix |
| --- | --- | --- |
| P0 | Model discovery bypassed native HTTPS and bearer checks. Successful replies could contain an unredacted key or invalid JSON. | Validate native requests consistently, disable redirects, validate status and reply shapes, and scrub configured bearer values from diagnostics and native replies. |
| P0 | The driver discarded thinking options, assistant reasoning and tool identifiers required for continuations. | Preserve prepared messages and documented DeepSeek thinking controls. Skip stream-only request options for non-streaming chat. |
| P0 | Buffered native streams had no public decoded events. The SDK stream result dropped indexed tool fragments, finish reasons and token usage. | Validate native buffered events, require completion for successful stream requests, and apply shared SDK normalization with typed cache token accounting. Preserve provider reasoning in raw events. |
| P1 | The parser repeatedly joined strings and treated each SSE data line as an event. | Join fragments once, handle CRLF and multiline frames, and pre-size decoded event storage. |
| P2 | The example used a retired DeepSeek model identifier, and the install check used a stale tag. | Use `deepseek-flash` in the quick start and verify the documented `v0.1.1` installation. |

Native model listing retains its Result shape. Native streaming results add `events`, and the standalone parser returns a completion flag. The provider ID, API paths, credential variable, immutable SDK pin and public function names stay supported. The updated branch requires Kujo 1.5.0. The pinned SDK's raw-choice receipt fails on 1.4.0 and passes on 1.5.0. Cache usage fields are exposed in normalized `usage` so consumers can read their numeric values independently of the generic SDK raw-payload filter. [Chat Completions API](https://api-docs.deepseek.com/api/create-chat-completion/) [Thinking Mode](https://api-docs.deepseek.com/guides/thinking_mode/) [Current models](https://api-docs.deepseek.com/quick_start/pricing/)

On Intel macOS, seven alternating runs of 2,000 fixed SSE events under published Kujo 1.5.0 produced identical event digests. Median internal parse time changed from 595.169 ms to 48.240 ms. The sample ranges were 582.942–649.560 ms and 39.832–52.938 ms. Peak memory and token usage were unmeasured.

All nine deterministic cases passed through `bash scripts/release_quality_gate.sh` on Kujo 1.5.0 and 1.10.0 with the pinned SDK and on 1.10.0 with the current SDK. An official tag install, reinstall, validation and consumer check passed through `bash scripts/verify_installed_package.sh`. A clean source archive passed the offline gate and produced identical public-consumer output in the VM and interpreter. The verification used injected responses and local package artifacts.
