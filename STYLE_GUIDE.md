# 🧭 Deno Forge Style Guide

This style guide standardizes code conventions across all `@deno-forge/*` modules. It draws inspiration from the [Deno Standard Library](https://docs.deno.com/runtime/contributing/style_guide/) but is simplified and tailored for our use.

---

## 📁 File Naming

- **Method/function files**: Must use **Snake Case**
  - ✅ Correct: `check_git_installed.ts`
  - ❌ Incorrect: `check-git-installed.ts` (this is kebab-case, not allowed)
  - ⛔ Do not use hyphens (-)—they are for CLI flags, not filenames.
- **Class files**: Must use **Pascal Case**
  - ✅ Correct: `GitInitializer.ts`
  - ❌ Incorrect: `git_initializer.ts` (GitInitializer is a class, should be named in PascalCase)
- **Test files**: Use `.test.ts` suffix
  - ✅ Correct: `check_git_installed.test.ts`
  - ❌ Incorrect: `check_git_installed_test.ts`
- **Tests are co-located** with the file they test.

---

## 🧪 Testing

- Tests must be colocated and use `.test.ts` suffix.
- Use `@std/assert` for assertions.
- Avoid global state. Prefer testable units with injected dependencies.
- Default values should be destructured in the method signature to avoid test coverage penalties.

---

## 🧾 Function Declarations

- Top-level functions must use the `function` keyword.

```ts
// ✅ Good
export function setupRepo1(): void {}

// 🚫 Bad
export const setupRepo2 = () => {};
```

---

## 💥 Error Messages

- Use sentence case.
- Include punctuation at the end of error messages.

```ts
// ✅ Good
throw new Error("Git is not installed or not available in your PATH.")

// 🚫 Bad
throw new Error("git is not installed")
```

---

## 📦 Module Entry Points

- Each module must have a `mod.ts` file as its public entry point.

---

## 🟦 TypeScript Only

- All source files must use `.ts`.
- Do not include `.js` files or mixed-format code.

---

## ✅ Linting

- All code must pass `deno lint`.
- If lint rules must be bypassed, use targeted `// deno-lint-ignore` comments.

---

## 📚 Documentation

- All exported functions, classes, and constants must include JSDoc-style comments.

```ts
/**
 * Initializes a Git repository with sane defaults.
 */
export function initGitRepo(): void { /* ... */ }
```

---

## 📜 Function Signature Design

- Exported Functions: max 4 arguments

The following argument types are allowed if needed, in the following order:

| Name         | Description                               |
|--------------|-------------------------------------------|
| Pos. Arg 1   | Any primitive type; only use if needed;   |
| Pos. Arg 2   | Any primitive type; only use if needed;   |
| Options Type | contains user-facing configuration        |
| Injects Type | List of optional injections (for testing) |

### 🧮 Positional Parameters

Public functions and methods may use **up to 2 required positional parameters** if:

- All parameters are primitive types (`string`, `boolean`, `number`)
- No parameters are optional or have default values

This aligns with the [Deno Style Guide](https://docs.deno.com/runtime/contributing/style_guide/).

### Options Type

- The `options` property should always have a defined Options Type, following the convention of `MyFunctionOptions` where the method name is written in PascalCase and "Options" is appended.
- Options Types should be exported with a jsdoc for every property
- **Always export Options Types in `mod.ts`.**

```ts
/** Options for runGitSetup */
export type RunGitSetupOptions = {
  /** The name of the branch to create. Defaults to 'main'. */
  branchName?: string,
  /** Whether to run in dry-run mode. Defaults to false. */
  dryRun?: boolean,
  /** Whether to skip committing files. Defaults to false. */
  noCommit?: boolean,
  /** Whether to open the GitHub URL in the browser. Defaults to true. */
  open?: boolean,
}
```

### Injection Types

- The `injects` property should always have a defined Injection Type, following the convention of `MyFunctionInjects` where the method name is written in PascalCase and "Injects" is appended.
- Injection Types should be exported with a jsdoc marking them `@internal`
- **Never export Injection Types in `mod.ts`.**

```ts
/** @internal */
export type RunGitSetupInjects = {
  parseGithubSettings?: typeof defaultParseGithubSettings
  checkGitInstalled?: typeof defaultCheckGitInstalled
  initRepoIfNeeded?: typeof defaultInitGitRepo
  setRemoteOrigin?: typeof defaultSetRemoteOrigin
  printGitHubUrl?: typeof defaultPrintGithubUrl
}
```

### Inline Defaulting

- Always destructure and assign default values inline in the function signature
- Avoid repeating default values (e.g., = { ... }) in both the parameter list and body
- Destructuring should always fully replace internal fallback logic
- Never default inside the function body

```ts
export async function runGitSetup(
  {
    branchName = "main",
    dryRun = false,
    noCommit = false,
    open = true,
  }: RunGitSetupOptions = {},
  {
    parseGithubSettings = defaultParseGithubSettings,
    checkGitInstalled = defaultCheckGitInstalled,
    initRepoIfNeeded = defaultInitGitRepo,
    setRemoteOrigin = defaultSetRemoteOrigin,
    printGitHubUrl = defaultPrintGithubUrl,
  }: RunGitSetupInjects = {},
): Promise<void> {
  //...
}
```

### 🔹 Inline readonly Constructor Properties

Inline readonly parameters in constructors are allowed only if:
- All params are 1:1 readonly public properties.
- The class has no mutable state or side effects.
- The constructor performs no validation or defaulting.

Use this for pure data containers like value objects or templates.
```ts
export class License {
  constructor(
    readonly key: string,
    readonly template: string,
    readonly tokens: string[],
  ) {}
}
```
Avoid if the class mutates, validates, or needs internal logic.

---

## 🔤 Naming Conventions for Types

### Acronyms in PascalCase

- Treat acronyms as regular words.
- Do not capitalize them entirely in type names.

| ✅ Good               | ❌ Bad                |
|----------------------|----------------------|
| `HttpClient`         | `HTTPClient`         |
| `GitRepoConfig`      | `GITRepoConfig`      |
| `RunGitSetupInjects` | `RunGitSetupINJECTS` |

---

## 🔒 Internal Types (`@internal`)

- Mark types with `@internal` to exclude them from `deno doc --lint` checks
- Use for helper or injection types not meant for public consumption
- Do not export internal types from `mod.ts`
- Required if an exported function references a private type
- Not a substitute for proper documentation of public APIs

---

### 🧱 Internal Support Code

- Internal helpers may live in the same file as a public function **if**:
  - They are marked `/** @internal */`
  - They support only that file’s exported symbol(s)
  - They are not exported from `mod.ts`
  - They appear after the main export unless needed earlier for types

- **Do not export internal helpers unless absolutely necessary.** Valid exceptions include:
  - The helper performs or depends on injected logic requiring elevated permissions (e.g. `--allow-read`)
  - It has enough complexity to justify isolated testing
  - It is reused across files in the module

- If exported, it must be clearly marked `@internal` and **never exported in `mod.ts`**

### 🌐 JSR Imports

When importing from a module published to [jsr.io](https://jsr.io), do **not** include file extensions. JSR modules resolve paths based on entrypoint declarations and registry metadata.

This ensures compatibility with JSR-aware tooling and matches the conventions of `@std` modules.

When importing from local files or remote HTTP sources, always include the `.ts` extension to comply with ECMAScript module rules.

Use **import maps** to manage external imports consistently across modules.

### 🗺️ Deno Import Maps (`deno.jsonc`)

All external dependencies—especially those from `@std`—must be mapped through the `imports` property in `deno.jsonc`.

This ensures:
- Predictable, version-pinned resolution across all files
- Clean and portable `jsr:`-style imports without file extensions
- Easier upgrades and module auditability

#### 🔒 Rules

- Always define external JSR imports in the `"imports"` section.
- Do **not** hardcode `jsr:` paths in source files—use the aliases instead.
- Keep import map aliases short and thematic (e.g. `@std/assert`, `@smith/test`).

#### 🧱 Examples of good aliases

- `@std/jsonc`
- `@std/testing/mock`

> 🔧 For Forge modules, use compact aliases:
> - `@anvil` for `@deno-forge/anvil`
> - `@scaffold` for `@deno-forge/scaffold`

---

Happy forging. 🔨
