# OpenClaw + Defapi (Gemini 3 Pro) Setup Guide

This guide documents a working setup for using [**Defapi**](https://defapi.org) with [**OpenClaw 2026.2.26**](https://openclaw.ai/) and `google/gemini-3-pro`.

## Why this guide exists

When switching from Mistral to Defapi/Gemini, common errors were:

- `Unknown model: defapi/gemini-3-pro`
- `No API key found for provider "google"`
- `HTTP 404: "Not Found"`
- Config schema errors for unsupported keys

The final working setup uses the **OpenAI-compatible Defapi mode** (Variant B).

---

## Prerequisites

- OpenClaw installed and running
- Defapi API key
- Access to:
  - `~/.openclaw/openclaw.json`
  - `/root/.openclaw/agents/main/agent/auth-profiles.json`

---

## 1) Configure provider in `openclaw.json`

> Important: In this OpenClaw version, the working config was **Variant B**:
> - `api: "openai-completions"`
> - `baseUrl: "https://api.defapi.org/v1/chat/completions"`

Example:

```json
{
  "auth": {
    "profiles": {
      "defapi:gemini": {
        "provider": "defapi",
        "mode": "token"
      }
    }
  },
  "models": {
    "mode": "merge",
    "providers": {
      "defapi": {
        "baseUrl": "https://api.defapi.org/v1/chat/completions",
        "api": "openai-completions",
        "models": [
          {
            "id": "google/gemini-3-pro",
            "name": "Gemini 3 Pro",
            "contextWindow": 1000000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "defapi/google/gemini-3-pro"
      },
      "models": {
        "defapi/google/gemini-3-pro": {}
      }
    }
  }
}

```

## 2) Configure API key in auth-profiles.json


Example:

```json

{
  "profiles": {
    "defapi:gemini": {
      "provider": "defapi",
      "mode": "token",
      "token": "YOUR_DEFAPI_KEY"
    }
  },
  "lastGood": {
    "defapi": "defapi:gemini"
  }
}
```

## 3) Restart gateway

``` openclaw gateway restart ```
