# Hermes Antigravity Plugin

A Google Antigravity OAuth account manager and local OpenAI-compatible proxy for [Hermes Agent](https://github.com/NousResearch/hermes-agent).

Point any OpenAI-style client at `http://127.0.0.1:8999/v1`. The plugin takes the request, translates it for Google's internal Gemini/Claude endpoints, and rotates across your signed-in Antigravity accounts. Text, images, tool calls, reasoning effort all survive the trip.

Chat completions (streaming and plain), live model catalog, multi-account rotation with per-family cooldowns, silent token refresh with auto re-login when that fails, and in-chat `/antigravity-login` plus quota commands. Tested on Python 3.11+ with stdlib only.

## Install

```bash
cp -r . ~/.hermes/plugins/antigravity-oauth
hermes gateway restart   # from your own shell, not inside a gateway session
```

Then in any Hermes chat:

```text
/antigravity-login
```

## Configure

```yaml
providers:
  antigravity:
    api_key: mock
    base_url: http://127.0.0.1:8999/v1
    default_model: gemini-3.8-flash

auxiliary:
  vision:
    provider: antigravity
    model: gemini-3.8-flash
```

## How an image gets through

Whatever the source (chat app, CLI, desktop, API call), Hermes hands the plugin the image bytes alongside the text, and the plugin forwards both to Gemini in its native part format. Nothing gets summarized or re-described in between.

This path used to drop the image part and let Gemini guess blind. v1.1.0 forwards the bytes, so descriptions match the actual picture. Verified with three identical reads of the same photo.

## Tests

```bash
python3 tests/test_image_parts.py   # vision translation, 3 tests
python3 tests/test_auto_reauth.py   # OAuth recovery, 6 tests
```

All nine pass. If you touch the translator, add a case first and watch it fail.

## Layout

```text
__init__.py            # proxy handler, translation, OAuth, CLI
plugin.yaml            # Hermes manifest (id: antigravity-oauth)
LICENSE                # MIT
SECURITY.md            # how to report vulnerabilities
tests/
  test_image_parts.py
  test_auto_reauth.py
CHANGELOG.md
```

## Credentials

The OAuth client ID/secret in `__init__.py` are Google Cloud credentials the Antigravity sign-in flow needs. For your own deployment, register your own OAuth client and swap them in.
