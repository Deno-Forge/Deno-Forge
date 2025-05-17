# 🧱 Core `@deno-forge` Module Ideas

## 1. `@deno-forge/tokenizer`
> Tokenizes strings into model-compatible tokens  
**Use for:** token counting, prompt prep, model input validation  
**Bonus:** Integrates with `@deno-forge/mphf` for fast vocab lookups

---

## 2. `@deno-forge/mphf`
> Builds Minimally Perfect Hash Functions for vocabularies, CLIs, or any static key set  
**Use for:** blazing-fast key lookups with zero collisions  
**Exports:** `buildMPHF`, `.hash()`, `.toJSON()`

---

## 3. `@deno-forge/changelog`
> Summarizes recent commit messages into human-readable changelogs via LLMs  
**Input:** last `n` Git commits  
**Output:** clean `## Added`, `## Changed`, etc.  
**Hooks into:** `@deno-forge/tokenizer`, `@deno-forge/prompts`

---

## 4. `@deno-forge/machine-prompts`
> Houses reusable AI prompt templates (changelog, readme, doc summaries, etc.)  
**Use for:** consistent tone and behavior across modules

---

## 5. `@deno-forge/init-readme`
> Generates `README.md` from `deno.jsonc`, CLI help, and optionally AI  
**AI-enhanced:** detects deprecated features, generates usage from source  
**Pulls from:** `prompts`, `changelog`, `branding`, `cost-model`

---

## 6. `@deno-forge/branding`
> Defines brand rules for consistent formatting, tone, and structure  
**Enforces:** required sections, badge layout, voice  
**Injects into prompts + validates AI output**

---

## 7. `@deno-forge/cost-model`
> Estimates token cost per LLM provider  
**Use for:** budget prediction, real-time token accounting  
**Supports:** DeepSeek, OpenAI, Claude (extendable)

---

## 8. `@deno-forge/temper`
> Prepares a module for release: version bump, README regeneration, test run, changelog check  
**Acts like:** `cargo release` or `np` but for `@deno-forge` tools  
**Enforces:** branding, tests, changelog freshness

---

Each module is powerful on its own, but combined, they form a **full AI-powered doc + release toolchain** for Deno modules, tuned to your specs, enforceable at every stage.
