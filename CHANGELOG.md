# Changelog

## v1.1.0 — 2026-09-07

- **Fix vision:** `translate_openai_to_gemini()` forwarded only `text` parts and silently
  dropped `image_url` parts, so Gemini received text-only messages and confabulated
  image descriptions. Image parts are now converted to Gemini `inlineData`
  (`_openai_image_to_inline_data()`), covering `data:` URLs inline and `http(s)` URLs
  via fetch. Order of mixed text/image parts is preserved.
- Added `tests/test_image_parts.py` regression tests.
- Added package docs: `README.md`, `CHANGELOG.md`, module docstring.
- Bumped manifest version to 1.1.0.

## v1.0.0

- Baseline: multi-account proxy, silent refresh + auto re-login, model catalog,
  tool-call/thinking translation, quota + CLI commands.
