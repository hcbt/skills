# anti-slop

anti-slop is vendored Oxlint policy, not an Agent Skill. Its upstream `src/` is canonical at bootstrap time; after devenv seeds it, the project owns the writable copy.

## Materialize the rules

Add the non-flake input:

```sh
devenv inputs add anti-slop github:dmmulroy/anti-slop
```

Set `flake: false` on `anti-slop` in `devenv.yaml`, then run `devenv update`. Add this to `devenv.nix`:

```nix
files."tools/oxlint/anti-slop" = {
  source = inputs.anti-slop + "/src";
  copyMode = "seed";
};

files."tools/oxlint/anti-slop/LICENSE" = {
  source = inputs.anti-slop + "/LICENSE";
  copyMode = "seed";
};
```

The built-in `devenv:files` task runs before `devenv:enterShell`. Seed mode creates a writable copy only when the destination is absent, so later activation preserves project changes. Enter the environment once and confirm the destination is a writable directory rather than a symlink and contains the upstream MIT license.

## Configure Oxlint

Read the resolved Oxlint version and install `@oxlint/plugins` at that exact version:

```sh
devenv shell -- sh -c '
  set -eu
  oxlint_version=$(bunx oxlint --version)
  oxlint_version=${oxlint_version#Version: }
  bun add --dev --exact "@oxlint/plugins@$oxlint_version"
'
```

Merge these values into `oxlint.config.ts`; preserve existing entries:

```ts
ignorePatterns: [
  ".agent/**",
  ".agents/**",
  ".claude/**",
  ".codex/**",
  ".continue/**",
  ".cursor/**",
  ".gemini/**",
  ".opencode/**",
  ".pi/**",
  ".roo/**",
  ".windsurf/**",
  "tools/oxlint/anti-slop/**",
],
jsPlugins: [
  { name: "anti-slop", specifier: "./tools/oxlint/anti-slop/index.ts" },
],
rules: {
  "anti-slop/no-chained-type-assertions": "error",
  "anti-slop/no-conditional-empty-object-spread": "error",
  "anti-slop/no-known-value-widening": "error",
  "anti-slop/no-module-mocking": "error",
  "anti-slop/no-object-parameters": "error",
  "anti-slop/no-reflect-apply": "error",
  "anti-slop/no-reflect-get": "error",
  "anti-slop/no-runtime-typeof": "error",
  "anti-slop/no-shape-in-symbol-names": "error",
  "anti-slop/no-unknown-parameters": "error",
  "anti-slop/no-unknown-returns": "error",
  "anti-slop/no-unknown-type-aliases": "error",
  "anti-slop/no-unsafe-dictionary-type": "error",
  "anti-slop/no-widen-then-assert": "error",
  "anti-slop/require-safety-comment-for-type-assertion": "error",
},
```

If `effect` is a direct dependency in the package manifest, also merge the opt-in Effect plugin and rule:

```ts
jsPlugins: [
  {
    name: "anti-slop-effect",
    specifier: "./tools/oxlint/anti-slop/effect/index.ts",
  },
],
rules: {
  "anti-slop-effect/no-service-constructor-imports": "error",
},
```

Merge these entries with the generic plugin configuration. A transitive lockfile entry does not enable the Effect policy.

The `tools/oxlint/anti-slop/**` ignore prevents Oxlint from checking the vendored plugin as application source. Add the same path to `ignorePatterns` in `.oxfmtrc.json` so oxfmt also leaves it alone. When configuring Vite+, merge the paths under both `lint.ignorePatterns` and `fmt.ignorePatterns` instead.

Exclude `tools/oxlint/anti-slop` from the application's `tsconfig.json`. The vendored plugin is executed by Oxlint and is not application source; compiling it against the application's TypeScript version creates a second, unintended compatibility contract.

Run the repository's lint, format check, typecheck, and tests. Fix findings in generated starter code; keep every rule at `"error"`.
