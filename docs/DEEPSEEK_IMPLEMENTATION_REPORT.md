# DeepSeek Implementation Report

## Executive Summary

Initial Kujo DeepSeek package for the documented DeepSeek OpenAI-compatible HTTP API, with a provider-qualified native client and pure AI SDK driver.

## Official API Evidence / Evidence Date

Official DeepSeek API documentation identifies `DEEPSEEK_API_KEY`, bearer authentication, and the OpenAI-compatible base `https://api.deepseek.com/v1` for chat completions, tools, JSON output, streaming, and reasoning models. Evidence date: 2026-08-27. The dedicated package preserves compatibility with the existing AI SDK `deepseek_provider()` preset. Source: [DeepSeek API documentation](https://api-docs.deepseek.com/).

## Protocol Classification

OPENAI-COMPATIBLE WITH PROVIDER EXTENSIONS. DeepSeek provides chat-completions-compatible wire semantics with DeepSeek model and inference metadata.

## Architecture / Native API Coverage

Native client: `src/deepseek.kujo`; AI SDK adapter: `src/provider.kujo`; root exports: `deepseek.kujo`. Chat, model listing, SSE parsing, tools, reasoning options, and usage are covered.

## Public Exports

`create_client`, `chat`, `client_chat`, `client_models`, `parse_stream`, `deepseek_provider`, `deepseek_driver`.

## Kujo Requirement / AI SDK Dependency

Kujo >= 1.0.2; `github:kujolang/ai-sdk@v1.1.0`.

## Authentication / Native Semantics / Streaming

Bearer `DEEPSEEK_API_KEY`, HTTPS enforcement, URL credential rejection, redaction, protected headers, and OpenAI-compatible SSE parsing. Native response fields remain in raw provider data.

## Tools / Structured Output / Reasoning / Multimodal / Embeddings

Tools, response format, reasoning, and provider-supported content fields remain provider-owned. Embeddings and multimodal model tasks are not claimed.

## Usage / Finish Reasons / Errors

Prompt/completion/total usage maps where supplied. Native error payloads and provider codes are retained subject to redaction.

## AI SDK Driver / Security / Tests

Pure descriptor/decoder hooks with no network I/O or policy bypass. Two deterministic offline files plus installed consumer smoke are included.

## Clean-Room Install / Installed Consumer Smoke

Passed with Kujo v1.0.2, including immutable Kennel add/install/reinstall/validate and installed consumer smoke with `KUJO_MODULE_PATH` unset.

## Live Validation

SKIPPED — credentials/environment unavailable.

## AI SDK Changes / Kujo Changes / Kennel Changes

None. Embeddings are exposed natively; the normalized AI SDK driver claims chat capabilities only because paired embedding hooks are not implemented.

## Contract Conformance / Limitations

See `DEEPSEEK_PROVIDER_PACKAGE_CONFORMANCE.md`. Native gRPC SDK semantics, image/video generation endpoints, files, batch jobs, and realtime features are outside this initial HTTP package.
