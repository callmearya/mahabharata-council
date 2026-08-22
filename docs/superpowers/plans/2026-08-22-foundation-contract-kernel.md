# Foundation Contract Kernel Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Establish the deterministic, cross-platform contract kernel on which every agent, workflow, adapter, runtime receipt, registry record, and evaluation artifact can safely depend.

**Architecture:** A single strict ESM TypeScript project loads canonical JSON sources, rejects ambiguous encodings and duplicate keys, produces RFC 8785 bytes and domain-separated fingerprints, validates versioned JSON Schemas through one strict registry, generates TypeScript types from those schemas, and applies append-only forward migrations with receipts. Provider-independent CI proves the kernel on Linux, macOS, and Windows before any domain logic is added.

**Tech Stack:** Node.js 24.19.0 LTS, npm 11.17.0, TypeScript 7.0.2, JSON Schema 2020-12, Ajv 8.20.0, ajv-formats 3.0.1, jsonc-parser 3.3.1, canonicalize 3.0.0, json-schema-to-typescript 15.0.4, Vitest 4.1.7, fast-check 4.9.0, Prettier 3.9.6.

**Spec:** [System design Sections 8.1, 8.4, 8.5, and 13](../specs/2026-08-21-mahabharata-council-design.md) and [Delegation Assurance Sections 11.5 and 16.1](../specs/2026-08-21-delegation-assurance-design.md)

## Global Constraints

- This package creates infrastructure only. It cannot publish compatibility, managed-runtime, quality, security-in-production, or performance claims.
- Automatic Council routing remains disabled. No Council route, including I0, is activated; P00 implements no router.
- The implementation is one root npm project, not a workspace collection.
- JSON Schema files are the semantic authority. Generated TypeScript files are outputs and must not be edited by hand.
- Every JSON source is strict UTF-8, NFC-normalized after escape decoding, LF-only, LF-terminated, and duplicate-key-free.
- Every semantic fingerprint input is explicit, versioned, and domain-separated. Labeled raw artifact checksums—such as fixture provenance—hash exact bytes directly, declare the algorithm in their containing contract, and are never reused as semantic record identifiers. `migrator_code_hash` is domain-separated over the canonical declarative program that the interpreter executes. Never hash `JSON.stringify` output.
- Migrations are forward-only, append-only transformations. They never mutate or silently rehash the input record.
- Every task begins with a representative behavioral test that is observed failing before the smallest implementation that makes it pass. Later property, mutation, and regression cases may probe that minimal implementation; whenever such a case exposes missing behavior, record its failure before applying the focused fix.
- Each task ends with its focused tests, the relevant broader suite, `git diff --check`, and a logical commit.
- This Arya-owned package contains no README, public guide, public cultural prose, prompt fine-tuning, public rubric, or release claim.
- Do not push, switch GitHub accounts, change global Git configuration, or change desktop account-switcher state while executing this plan unless a later explicit instruction authorizes that operation.

## Contract Conventions

These conventions are fixed for every task in this plan:

```ts
export type JsonPrimitive = null | boolean | number | string;

export type JsonValue =
  | JsonPrimitive
  | JsonValue[]
  | { [key: string]: JsonValue };

export type CanonicalizationVersion = "jcs-rfc8785-v1";
export type HashAlgorithm = "sha-256";
```

- Schema IDs use `https://callmearya.github.io/mahabharata-council/schemas/<semver>/<name>.schema.json`.
- Schema filenames are lowercase kebab case and end in `.schema.json`.
- Schema versions are explicit strings; filenames do not silently resolve “latest.”
- SHA-256 digests use RFC 4648 base64url without padding and are exactly 43 characters.
- Fingerprints use `<namespace>:jcs-rfc8785-v1:sha-256:<digest>`.
- Hash preimages use the UTF-8 prefix `mahabharata-council:v1\0<namespace>\0` followed by RFC 8785 bytes.
- Errors expose a stable machine code and a human message; tests assert the code rather than entire prose.

---

## Task 1: Pin the root toolchain and create the executable test scaffold

**Files:**

- Create: `.nvmrc`
- Create: `.node-version`
- Create: `.editorconfig`
- Create: `.gitattributes`
- Create: `package.json`
- Create: `package-lock.json`
- Create: `tsconfig.base.json`
- Create: `tsconfig.json`
- Create: `tsconfig.build.json`
- Create: `vitest.config.ts`
- Create: `prettier.config.mjs`
- Create: `runtime/src/index.ts`
- Test: `tests/tooling/smoke.test.ts`

**Interfaces:**

- Consumes: the design specifications after G0 records their approval and target-branch integration, plus the exact toolchain versions in this plan.
- Produces: `FOUNDATION_CONTRACT_VERSION`, root npm scripts, a reproducible lockfile, and shared TypeScript/test configuration.
- Invariant: `npm ci` must install without rewriting `package-lock.json`.

- [ ] **Step 1: Verify the execution runtime before creating files**

Run:

```bash
node --version
npm --version
```

Expected: `v24.19.0` and `11.17.0`. If the versions differ, stop this task and select Node 24.19.0 through the existing local version manager; do not install a global package manager or change unrelated shell configuration.

- [ ] **Step 2: Add the pinned project metadata**

Write `.nvmrc` and `.node-version` as exactly:

```text
24.19.0
```

Write `package.json` with exact dependency versions and no range prefixes:

```json
{
  "name": "@callmearya/mahabharata-council",
  "version": "0.0.0",
  "private": true,
  "type": "module",
  "engines": {
    "node": "24.19.0",
    "npm": "11.17.0"
  },
  "packageManager": "npm@11.17.0",
  "scripts": {
    "build": "tsc -p tsconfig.build.json",
    "typecheck": "tsc -p tsconfig.json --noEmit",
    "test": "npm run build --silent && vitest run",
    "test:contracts": "npm run build --silent && vitest run tests/contracts",
    "format": "prettier --write --ignore-unknown --no-error-on-unmatched-pattern \"runtime/**/*\" \"scripts/**/*\" \"tests/**/*\" \"schemas/**/*\" \".github/**/*\" \"*.{json,ts,mjs}\"",
    "format:check": "prettier --check --ignore-unknown --no-error-on-unmatched-pattern \"runtime/**/*\" \"scripts/**/*\" \"tests/**/*\" \"schemas/**/*\" \".github/**/*\" \"*.{json,ts,mjs}\"",
    "generate:schemas": "npm run build --silent && node scripts/build-schema-catalog.mjs",
    "generate:types": "npm run build --silent && node scripts/generate-contract-types.mjs",
    "check:sources": "npm run build --silent && node scripts/check-source-invariants.mjs",
    "check:generated": "npm run build --silent && node scripts/check-generated.mjs",
    "ci": "npm run check:sources && npm run check:generated && npm run format:check && npm run typecheck && npm run test && npm run build"
  },
  "dependencies": {
    "ajv": "8.20.0",
    "ajv-formats": "3.0.1",
    "canonicalize": "3.0.0",
    "jsonc-parser": "3.3.1"
  },
  "devDependencies": {
    "@types/node": "24.13.3",
    "fast-check": "4.9.0",
    "json-schema-to-typescript": "15.0.4",
    "prettier": "3.9.6",
    "typescript": "7.0.2",
    "vitest": "4.1.7"
  }
}
```

Write `tsconfig.base.json`:

```json
{
  "compilerOptions": {
    "target": "ES2024",
    "lib": ["ES2024"],
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitOverride": true,
    "useUnknownInCatchVariables": true,
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": false
  }
}
```

Write `tsconfig.json`:

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "noEmit": true,
    "types": ["node", "vitest/globals"]
  },
  "include": ["runtime/src/**/*.ts", "tests/**/*.ts", "vitest.config.ts"]
}
```

Write `tsconfig.build.json`:

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "rootDir": ".",
    "outDir": "dist",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["runtime/src/**/*.ts"],
  "exclude": ["tests", "dist", "node_modules"]
}
```

Write `vitest.config.ts`:

```ts
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    clearMocks: true,
    environment: "node",
    include: ["tests/**/*.test.ts"],
    restoreMocks: true,
    testTimeout: 10_000,
  },
});
```

Write `prettier.config.mjs`:

```js
/** @type {import("prettier").Config} */
const config = {
  endOfLine: "lf",
  printWidth: 100,
  semi: true,
  singleQuote: false,
  tabWidth: 2,
  trailingComma: "all",
};

export default config;
```

Write `.editorconfig`:

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[Makefile]
indent_style = tab
```

Write `.gitattributes`:

```gitattributes
* text=auto eol=lf
*.gif binary
*.gz binary
*.jpg binary
*.jpeg binary
*.pdf binary
*.png binary
*.webp binary
*.zip binary
```

- [ ] **Step 3: Install exact dependencies and create the lockfile**

Run:

```bash
npm install
npm ls --depth=0
```

Expected: installation succeeds, `package-lock.json` records lockfile version 3, and every top-level version exactly matches `package.json`.

- [ ] **Step 4: Write the failing smoke test**

Create `tests/tooling/smoke.test.ts`:

```ts
import { describe, expect, it } from "vitest";

import { FOUNDATION_CONTRACT_VERSION } from "../../runtime/src/index.js";

describe("foundation toolchain", () => {
  it("exports the locked contract version", () => {
    expect(FOUNDATION_CONTRACT_VERSION).toBe("1.0.0");
  });
});
```

Run:

```bash
npx --no-install vitest run tests/tooling/smoke.test.ts
```

Expected failure: Vitest starts directly and cannot resolve `runtime/src/index.js` or the export is missing. The red step intentionally bypasses the build script because `tsconfig.build.json` has no source input until the runtime entry point is created.

- [ ] **Step 5: Add the minimal runtime entry point**

Create `runtime/src/index.ts`:

```ts
export const FOUNDATION_CONTRACT_VERSION = "1.0.0" as const;
```

Run:

```bash
npm test -- tests/tooling/smoke.test.ts
npm run typecheck
npm run build
```

Expected: one test passes, TypeScript reports no errors, and `dist/runtime/src/index.js` plus declarations are emitted.

- [ ] **Step 6: Format, inspect, and commit**

Run:

```bash
npm run format
npm run format:check
git diff --check
git status --short
```

Expected: formatting and whitespace checks pass; only Task 1 files are present as working-tree changes, with no unrelated paths.

Commit:

```bash
git add .nvmrc .node-version .editorconfig .gitattributes package.json package-lock.json tsconfig.base.json tsconfig.json tsconfig.build.json vitest.config.ts prettier.config.mjs runtime/src/index.ts tests/tooling/smoke.test.ts
git commit -m "build: establish foundation toolchain"
```

---

## Task 2: Reject ambiguous JSON before ordinary parsing can erase it

**Files:**

- Create: `runtime/src/contracts/json-value.ts`
- Create: `runtime/src/contracts/strict-json.ts`
- Create: `tests/contracts/strict-json.test.ts`
- Create: `tests/fixtures/strict-json/valid.json`
- Create: `tests/fixtures/strict-json/duplicate-key.hex`
- Create: `tests/fixtures/strict-json/non-nfc-value.hex`
- Modify: `runtime/src/index.ts`

**Interfaces:**

```ts
export type StrictJsonErrorCode =
  | "INVALID_UTF8"
  | "INVALID_JSON"
  | "DUPLICATE_KEY"
  | "NON_NFC"
  | "NON_LF_LINE_ENDING"
  | "MISSING_FINAL_LF";

export declare class StrictJsonError extends Error {
  readonly code: StrictJsonErrorCode;
  readonly offset?: number;
}

export declare function parseStrictJsonSource(bytes: Uint8Array): JsonValue;
```

- Consumes: raw bytes only; never accepts a pre-parsed object.
- Produces: a `JsonValue` only after source and decoded-value invariants pass.
- Invariant: every canonical repository/source loader accepts raw bytes and calls `parseStrictJsonSource`; no such loader accepts pre-parsed input. Downstream APIs that accept `JsonValue` are only for values returned by that boundary or created inside trusted runtime code and are not source-ingestion APIs.

- [ ] **Step 1: Add valid and adversarial fixtures**

Create a small valid object ending in LF. Store negative source bytes as lowercase hexadecimal text so invalid JSON never masquerades as a repository JSON source: `duplicate-key.hex` contains `7b226964223a312c226964223a327d0a`, and `non-nfc-value.hex` contains `7b226e616d65223a22436166655c7530333031227d0a`. Every committed `.hex` representation itself ends in LF.

- [ ] **Step 2: Write the failing strict-loader tests**

Create `tests/contracts/strict-json.test.ts` with these core cases:

```ts
import { readFile } from "node:fs/promises";
import { fileURLToPath } from "node:url";
import { describe, expect, it } from "vitest";

import {
  parseStrictJsonSource,
  StrictJsonError,
} from "../../runtime/src/contracts/strict-json.js";

const validFixture = (name: string) =>
  readFile(
    fileURLToPath(new URL(`../fixtures/strict-json/${name}`, import.meta.url)),
  );

const invalidFixture = async (name: string): Promise<Uint8Array> =>
  Buffer.from(
    (
      await readFile(
        fileURLToPath(
          new URL(`../fixtures/strict-json/${name}.hex`, import.meta.url),
        ),
        "utf8",
      )
    ).trim(),
    "hex",
  );

const expectCode = (
  bytes: Uint8Array,
  code: StrictJsonError["code"],
): void => {
  expect(() => parseStrictJsonSource(bytes)).toThrowError(
    expect.objectContaining({ code }),
  );
};

describe("parseStrictJsonSource", () => {
  it("loads valid LF-terminated JSON", async () => {
    await expect(
      validFixture("valid.json").then(parseStrictJsonSource),
    ).resolves.toEqual({ enabled: true, name: "council" });
  });

  it("rejects invalid UTF-8", () => {
    expectCode(Uint8Array.of(0xff), "INVALID_UTF8");
  });

  it("rejects duplicate keys before JSON.parse erases them", async () => {
    expectCode(await invalidFixture("duplicate-key"), "DUPLICATE_KEY");
  });

  it("rejects CRLF", () => {
    expectCode(
      new TextEncoder().encode('{\r\n  "enabled": true\r\n}\r\n'),
      "NON_LF_LINE_ENDING",
    );
  });

  it("requires a final LF", () => {
    expectCode(
      new TextEncoder().encode('{"enabled":true}'),
      "MISSING_FINAL_LF",
    );
  });

  it("rejects a decoded non-NFC string", async () => {
    expectCode(await invalidFixture("non-nfc-value"), "NON_NFC");
  });
});
```

Run:

```bash
npm test -- tests/contracts/strict-json.test.ts
```

Expected failure: the contract modules do not exist.

- [ ] **Step 3: Implement the recursive JSON types**

Create `runtime/src/contracts/json-value.ts`:

```ts
export type JsonPrimitive = null | boolean | number | string;

export type JsonValue =
  | JsonPrimitive
  | JsonValue[]
  | { [key: string]: JsonValue };

export const isJsonValue = (value: unknown): value is JsonValue => {
  if (
    value === null ||
    typeof value === "boolean" ||
    typeof value === "string"
  ) {
    return true;
  }
  if (typeof value === "number") {
    return Number.isFinite(value);
  }
  if (Array.isArray(value)) {
    return value.every(isJsonValue);
  }
  if (typeof value !== "object") {
    return false;
  }
  const prototype = Object.getPrototypeOf(value);
  if (prototype !== Object.prototype && prototype !== null) {
    return false;
  }
  return Object.values(value).every(isJsonValue);
};
```

- [ ] **Step 4: Implement strict decoding, duplicate detection, and NFC checks**

Create `runtime/src/contracts/strict-json.ts` using `TextDecoder("utf-8", { fatal: true })` and `jsonc-parser.visit`. The implementation must:

1. map decoder failure to `INVALID_UTF8`;
2. reject any carriage return before parsing;
3. require the last byte to decode as LF;
4. call `visit` with comments and trailing commas disallowed;
5. push a new `Set<string>` on every object begin, reject a repeated property in the active object, and pop on object end;
6. map syntax errors to `INVALID_JSON`;
7. call `JSON.parse` only after the visitor succeeds;
8. recursively reject non-NFC string values and object keys; and
9. assert the parsed result satisfies `isJsonValue`.

Use this error shape:

```ts
export class StrictJsonError extends Error {
  override readonly name = "StrictJsonError";

  constructor(
    readonly code: StrictJsonErrorCode,
    message: string,
    readonly offset?: number,
  ) {
    super(message);
  }
}
```

The duplicate-key visitor must be scoped per object:

```ts
const keyScopes: Array<Set<string>> = [];
let duplicate: { key: string; offset: number } | undefined;

const errors = visit(
  source,
  {
    onObjectBegin: () => {
      keyScopes.push(new Set());
    },
    onObjectProperty: (key, offset) => {
      const scope = keyScopes.at(-1);
      if (scope?.has(key)) {
        duplicate ??= { key, offset };
      }
      scope?.add(key);
    },
    onObjectEnd: () => {
      keyScopes.pop();
    },
  },
  { allowTrailingComma: false, disallowComments: true },
);
```

Export the new public types and loader from `runtime/src/index.ts`.

- [ ] **Step 5: Run focused, mutation, and broader tests**

Add cases for nested objects reusing the same key legally, duplicate nested keys illegally, comments, trailing commas, top-level primitives, arrays, non-NFC keys, malformed escapes, empty bytes, and a valid emoji surrogate pair. Add cases proving the caller’s input bytes are not mutated and `isJsonValue` rejects non-JSON prototypes such as `Date` and `Map`.

Run:

```bash
npm test -- tests/contracts/strict-json.test.ts
npm run typecheck
npm test
```

Expected: every strict-loader and smoke test passes.

- [ ] **Step 6: Commit the strict source boundary**

Run `npm run format` and `git diff --check`, then commit:

```bash
git add runtime/src/contracts runtime/src/index.ts tests/contracts/strict-json.test.ts tests/fixtures/strict-json
git commit -m "feat: enforce strict JSON source invariants"
```

---

## Task 3: Produce RFC 8785 bytes and domain-separated fingerprints

**Files:**

- Create: `runtime/src/contracts/canonical-json.ts`
- Create: `tests/contracts/canonical-json.test.ts`
- Create: `tests/fixtures/rfc8785/input.json`
- Create: `tests/fixtures/rfc8785/expected.txt`
- Create: `tests/fixtures/rfc8785/manifest.json`
- Modify: `runtime/src/index.ts`

**Interfaces:**

```ts
export const CANONICALIZATION_VERSION = "jcs-rfc8785-v1" as const;
export const HASH_ALGORITHM = "sha-256" as const;

export type CanonicalJsonErrorCode =
  | "INVALID_JSON_VALUE"
  | "INVALID_UNICODE_SCALAR"
  | "SERIALIZATION_FAILED"
  | "INVALID_NAMESPACE";

export declare class CanonicalJsonError extends Error {
  readonly code: CanonicalJsonErrorCode;
}

export declare function canonicalBytes(value: JsonValue): Uint8Array;
export declare function sha256Base64Url(bytes: Uint8Array): string;
export declare function fingerprint(namespace: string, value: JsonValue): string;
```

- Consumes: a validated `JsonValue`.
- Produces: RFC 8785 UTF-8 bytes, a 43-character SHA-256 base64url digest, and a namespaced fingerprint.
- Invariant: the namespace is present in both the visible identifier and the hash preimage.

- [ ] **Step 1: Add provenance-locked RFC 8785 fixtures**

Copy the input and expected serialization from RFC 8785 Section 3.2.2 into `tests/fixtures/rfc8785/` with LF termination. Record the RFC URL, section, retrieval date, fixture filenames, and their raw SHA-256 digests in `manifest.json`. Do not modify numeric spellings or escape sequences to make a test easier.

- [ ] **Step 2: Write failing vector and fingerprint tests**

Create `tests/contracts/canonical-json.test.ts`:

```ts
import { readFile } from "node:fs/promises";
import { describe, expect, it } from "vitest";

import {
  canonicalBytes,
  fingerprint,
  sha256Base64Url,
} from "../../runtime/src/contracts/canonical-json.js";
import { parseStrictJsonSource } from "../../runtime/src/contracts/strict-json.js";

describe("canonical JSON", () => {
  it("matches the RFC 8785 example bytes", async () => {
    const input = parseStrictJsonSource(
      await readFile(new URL("../fixtures/rfc8785/input.json", import.meta.url)),
    );
    const expected = (
      await readFile(
        new URL("../fixtures/rfc8785/expected.txt", import.meta.url),
        "utf8",
      )
    ).slice(0, -1);

    expect(new TextDecoder().decode(canonicalBytes(input))).toBe(expected);
  });

  it("sorts keys independently of insertion order", () => {
    expect(canonicalBytes({ b: 2, a: 1 })).toEqual(
      canonicalBytes({ a: 1, b: 2 }),
    );
  });

  it("uses base64url without padding", () => {
    expect(sha256Base64Url(new Uint8Array())).toMatch(/^[A-Za-z0-9_-]{43}$/u);
  });

  it("separates record namespaces", () => {
    expect(fingerprint("mpf", { id: "same" })).not.toBe(
      fingerprint("evidence", { id: "same" }),
    );
  });
});
```

Run:

```bash
npm test -- tests/contracts/canonical-json.test.ts
```

Expected failure: the canonical JSON module does not exist.

- [ ] **Step 3: Implement canonical bytes and hashes**

Create `runtime/src/contracts/canonical-json.ts`:

```ts
import { createHash } from "node:crypto";
import canonicalize from "canonicalize";

import type { JsonValue } from "./json-value.js";

export const CANONICALIZATION_VERSION = "jcs-rfc8785-v1" as const;
export const HASH_ALGORITHM = "sha-256" as const;

export type CanonicalJsonErrorCode =
  | "INVALID_JSON_VALUE"
  | "INVALID_UNICODE_SCALAR"
  | "SERIALIZATION_FAILED"
  | "INVALID_NAMESPACE";

export class CanonicalJsonError extends Error {
  constructor(
    readonly code: CanonicalJsonErrorCode,
    message: string,
    options?: ErrorOptions,
  ) {
    super(message, options);
    this.name = "CanonicalJsonError";
  }
}

const encoder = new TextEncoder();
const namespacePattern = /^[a-z][a-z0-9-]{0,31}$/u;

export const canonicalBytes = (value: JsonValue): Uint8Array => {
  assertJsonValueAndUnicodeScalars(value);
  let serialized: string | undefined;
  try {
    serialized = canonicalize(value);
  } catch (cause) {
    throw new CanonicalJsonError(
      "SERIALIZATION_FAILED",
      "Value cannot be serialized by RFC 8785",
      { cause },
    );
  }
  if (serialized === undefined) {
    throw new CanonicalJsonError(
      "SERIALIZATION_FAILED",
      "Value cannot be serialized by RFC 8785",
    );
  }
  return encoder.encode(serialized);
};

export const sha256Base64Url = (bytes: Uint8Array): string =>
  createHash("sha256").update(bytes).digest("base64url");

export const fingerprint = (
  namespace: string,
  value: JsonValue,
): string => {
  if (!namespacePattern.test(namespace)) {
    throw new CanonicalJsonError(
      "INVALID_NAMESPACE",
      `Invalid fingerprint namespace: ${namespace}`,
    );
  }
  const prefix = encoder.encode(
    `mahabharata-council:v1\0${namespace}\0`,
  );
  const digest = sha256Base64Url(
    Buffer.concat([prefix, canonicalBytes(value)]),
  );
  return `${namespace}:${CANONICALIZATION_VERSION}:${HASH_ALGORITHM}:${digest}`;
};
```

Before calling `canonicalize`, `assertJsonValueAndUnicodeScalars` rejects non-finite numbers and non-JSON prototypes with `INVALID_JSON_VALUE`, and lone UTF-16 surrogates in values and keys with `INVALID_UNICODE_SCALAR`. RFC 8785 requires failure for invalid Unicode data; no path may fall back to replacement characters or an untyped expected error.

Export the public constants, error, and functions from `runtime/src/index.ts`.

- [ ] **Step 4: Add property and environment invariance coverage**

Using fast-check, add at least 500 deterministic runs with a fixed seed for:

- equivalent objects built with different key insertion orders;
- one-value mutations changing the fingerprint;
- every digest matching 43-character base64url;
- namespace rejection for empty, uppercase, NUL-containing, and overlong values, each asserting `CanonicalJsonError.code === "INVALID_NAMESPACE"`;
- invalid JSON values, invalid Unicode scalars, and forced serializer failures asserting their exact stable codes; and
- input values remaining deeply equal after canonicalization.

Run the focused suite as separate processes under `TZ=UTC, LANG=C` and `TZ=Pacific/Kiritimati, LANG=en_US.UTF-8`. The test itself uses only fixed JSON values; compare its exact expected fingerprint fixture in both invocations.

Run:

```bash
npm test -- tests/contracts/canonical-json.test.ts
env TZ=UTC LANG=C npm test -- tests/contracts/canonical-json.test.ts
env TZ=Pacific/Kiritimati LANG=en_US.UTF-8 npm test -- tests/contracts/canonical-json.test.ts
npm run typecheck
npm test
```

Expected: the RFC vector, properties, mutation cases, and both environments agree.

- [ ] **Step 5: Commit canonicalization**

Run `npm run format` and `git diff --check`, then commit:

```bash
git add runtime/src/contracts/canonical-json.ts runtime/src/index.ts tests/contracts/canonical-json.test.ts tests/fixtures/rfc8785
git commit -m "feat: add canonical contract fingerprints"
```

---

## Task 4: Build the strict schema catalog and generate TypeScript types

**Files:**

- Create: `schemas/1.0.0/common.schema.json`
- Create: `schemas/1.0.0/schema-catalog.schema.json`
- Create: `schemas/1.0.0/record-envelope.schema.json`
- Create: `schemas/1.0.0/migration-program.schema.json`
- Create: `schemas/1.0.0/migration-receipt.schema.json`
- Create: `schemas/catalog.json`
- Create: `runtime/src/contracts/schema-registry.ts`
- Generate: `runtime/src/contracts/generated/common.ts`
- Generate: `runtime/src/contracts/generated/schema-catalog.ts`
- Generate: `runtime/src/contracts/generated/record-envelope.ts`
- Generate: `runtime/src/contracts/generated/migration-program.ts`
- Generate: `runtime/src/contracts/generated/migration-receipt.ts`
- Generate: `runtime/src/contracts/generated/index.ts`
- Create: `scripts/build-schema-catalog.mjs`
- Create: `scripts/generate-contract-types.mjs`
- Create: `tests/contracts/schema-registry.test.ts`
- Create: `tests/tooling/schema-generation.test.ts`
- Create: `tests/fixtures/schemas/invalid-additional-property.json`
- Create: `tests/fixtures/schemas/no-network.cjs`
- Modify: `runtime/src/index.ts`

**Interfaces:**

```ts
export type ValidationIssue = {
  instancePath: string;
  schemaPath: string;
  keyword: string;
  message: string;
};

export declare class SchemaValidationError extends Error {
  readonly code: "UNKNOWN_SCHEMA" | "INVALID_CONTRACT";
  readonly issues: readonly ValidationIssue[];
}

export interface SchemaRegistry {
  has(schemaId: string): boolean;
  validate(schemaId: string, value: JsonValue): JsonValue;
}

export declare function createSchemaRegistry(
  schemas: readonly JsonValue[],
): SchemaRegistry;
```

- Consumes: strict JSON schema bytes and P00 canonical fingerprints.
- Produces: a sorted catalog, a strict Ajv registry, and schema-derived TypeScript declarations.
- Invariant: generation is deterministic and never mutates input during validation.

- [ ] **Step 1: Write the failing registry tests**

Create `tests/contracts/schema-registry.test.ts`:

```ts
import { describe, expect, it } from "vitest";

import {
  createSchemaRegistry,
  SchemaValidationError,
} from "../../runtime/src/contracts/schema-registry.js";
import type { JsonValue } from "../../runtime/src/contracts/json-value.js";

const schemaId = "https://example.test/schemas/item.schema.json";
const schema: JsonValue = {
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": schemaId,
  type: "object",
  required: ["id"],
  properties: { id: { type: "string", minLength: 1 } },
  additionalProperties: false,
};

describe("SchemaRegistry", () => {
  it("returns valid input without applying defaults or coercions", () => {
    const registry = createSchemaRegistry([schema]);
    const input = { id: "x" };
    expect(registry.validate(schemaId, input)).toBe(input);
  });

  it("rejects additional properties", () => {
    const registry = createSchemaRegistry([schema]);
    expect(() =>
      registry.validate(schemaId, { id: "x", extra: true }),
    ).toThrowError(
      expect.objectContaining<Partial<SchemaValidationError>>({
        code: "INVALID_CONTRACT",
      }),
    );
  });

  it("rejects unknown schema IDs", () => {
    const registry = createSchemaRegistry([schema]);
    expect(() => registry.validate("urn:missing", { id: "x" })).toThrowError(
      expect.objectContaining({ code: "UNKNOWN_SCHEMA" }),
    );
  });
});
```

Run:

```bash
npm test -- tests/contracts/schema-registry.test.ts
```

Expected failure: the registry module does not exist.

- [ ] **Step 2: Add the versioned foundation schemas**

Every schema declares draft 2020-12, a stable `$id` under the project URL, `schema_version` where it represents data, explicit `required` fields, and an explicit additional-properties policy.

Define reusable `$defs` in `common.schema.json` for:

- `Semver`;
- `CanonicalizationVersion` fixed to `jcs-rfc8785-v1`;
- `HashAlgorithm` fixed to `sha-256`;
- `Sha256Base64Url` with `^[A-Za-z0-9_-]{43}$`;
- `Fingerprint` with a namespaced JCS/SHA-256 shape;
- UTC `Timestamp` with an uppercase `Z` suffix; and
- nonempty `RecordId`.

Define `record-envelope.schema.json` with these required fields:

```json
{
  "schema_version": "1.0.0",
  "canonicalization_version": "jcs-rfc8785-v1",
  "hash_algorithm": "sha-256",
  "record_id": "example:record:1",
  "created_at": "2026-08-22T00:00:00Z",
  "supersedes": null,
  "provenance_hashes": [],
  "payload": {}
}
```

Use this schema structure:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/record-envelope.schema.json",
  "title": "RecordEnvelope",
  "type": "object",
  "required": [
    "schema_version",
    "canonicalization_version",
    "hash_algorithm",
    "record_id",
    "created_at",
    "supersedes",
    "provenance_hashes",
    "payload"
  ],
  "properties": {
    "schema_version": { "const": "1.0.0" },
    "canonicalization_version": {
      "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/CanonicalizationVersion"
    },
    "hash_algorithm": {
      "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/HashAlgorithm"
    },
    "record_id": {
      "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/RecordId"
    },
    "created_at": {
      "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/Timestamp"
    },
    "supersedes": {
      "anyOf": [
        { "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/RecordId" },
        { "type": "null" }
      ]
    },
    "provenance_hashes": {
      "type": "array",
      "items": {
        "$ref": "https://callmearya.github.io/mahabharata-council/schemas/1.0.0/common.schema.json#/$defs/Sha256Base64Url"
      },
      "uniqueItems": true
    },
    "payload": {},
    "extensions": {
      "type": "object",
      "additionalProperties": true
    }
  },
  "additionalProperties": false
}
```

The named `extensions` object retains explicitly preserved extension data. An older reader may preserve and display raw bytes from an unknown schema ID, but `SchemaRegistry.validate` must never route from an unknown schema.

Define `schema-catalog.schema.json` with a sorted `entries` array whose items contain `schema_id`, `schema_version`, repository-relative `source_path`, and `schema_fingerprint`. Define `migration-program.schema.json` for Task 5’s side-effect-free, object-pointer-only declarative operations: `set`, `remove`, `copy`, and `move`; it requires `schema_version: "1.0.0"`, `program_version: "migration-program-v1"`, source/destination schema IDs, ordered operations, and explicit extension mappings, and forbids every unknown field. Define `migration-receipt.schema.json` in Task 5’s final receipt shape now so the migration implementation consumes schemas rather than inventing duplicate TypeScript authority.

- [ ] **Step 3: Implement strict Ajv 2020-12 validation**

Create `runtime/src/contracts/schema-registry.ts` with `Ajv2020` imported from `ajv/dist/2020.js` and the default `addFormats` export from `ajv-formats`. Use exactly:

```ts
const ajv = new Ajv2020({
  strict: true,
  allErrors: true,
  removeAdditional: false,
  coerceTypes: false,
  useDefaults: false,
  validateFormats: true,
});
addFormats(ajv);
```

Before validation, snapshot `canonicalBytes(value)`. After validation, compare bytes and throw an internal invariant error if Ajv changed the value. Normalize Ajv errors into sorted `ValidationIssue[]` by `instancePath`, `schemaPath`, `keyword`, then `message` so cross-platform output is stable.

Reject duplicate schema IDs at registry creation. Reject a schema without `$schema`, `$id`, or draft 2020-12. Compile every schema during construction so configuration failures are startup failures.

Export the registry interfaces from `runtime/src/index.ts`.

- [ ] **Step 4: Write the failing hermetic-generation tests**

Create `tests/tooling/schema-generation.test.ts` before either generator exists. The test invokes each script through `execFile(process.execPath, ...)` against isolated temporary source, catalog, and output roots and asserts:

- two runs produce byte-identical catalog and TypeScript trees;
- every absolute `$ref` whose document ID is in the local catalog resolves without external I/O;
- an absolute HTTPS `$ref` absent from the local catalog fails with stable code `UNRESOLVED_SCHEMA_REF`;
- a referenced local schema change changes the dependent generated declaration;
- exactly five catalog entries and six TypeScript files are produced; and
- source and generated input trees remain unchanged.

Create `tests/fixtures/schemas/no-network.cjs` as a child-process preload that throws distinct sentinel errors from `globalThis.fetch`, `node:http`, `node:https`, and `node:net` connection entry points. Run the happy path and unknown-ref case with `execFile(process.execPath, ["--require", preloadPath, scriptPath, ...args])`; the expected unknown-ref error must be `UNRESOLVED_SCHEMA_REF`, never a network sentinel.

Run:

```bash
npm test -- tests/tooling/schema-generation.test.ts
```

Expected failure: the catalog and type-generator scripts do not exist.

- [ ] **Step 5: Implement deterministic schema-catalog generation**

Implement `scripts/build-schema-catalog.mjs` to:

1. enumerate only `schemas/<semver>/*.schema.json`;
2. sort paths by Unicode code point;
3. load each through the built strict JSON loader;
4. require its URL version segment to match its directory;
5. require the filename to match the final `$id` segment;
6. reject duplicate IDs;
7. fingerprint each schema with namespace `schema`;
8. emit `schemas/catalog.json` in stable key order, pretty-printed with two spaces and one final LF; and
9. validate the emitted catalog against `schema-catalog.schema.json`.

The script accepts optional arguments `--source-root <path>` and `--output-root <path>` so drift checking can generate into an isolated temporary directory without touching the working tree.

Run:

```bash
npm run generate:schemas
```

Expected: `schemas/catalog.json` contains five entries sorted by `schema_id`, and a second run produces no diff.

- [ ] **Step 6: Generate TypeScript from the schema authority without network resolution**

Implement `scripts/generate-contract-types.mjs` with `json-schema-to-typescript`. Before compilation, load every catalog schema through `parseStrictJsonSource` into a local map keyed by its exact `$id`. A project-owned resolver must recursively replace each `$ref` using only that map and RFC 6901 fragments:

1. split the document URI from its fragment;
2. treat an empty document URI as the current schema and an absolute document URI as a required exact local-map key;
3. reject relative document URIs, unknown IDs, invalid pointer escapes, missing pointer segments, and reference cycles with stable generation errors;
4. deep-clone the referenced node so resolution never mutates a source schema; and
5. finish with no document-level `$ref` remaining.

Pass the fully local resolved clone to the compiler with `$refOptions: { resolve: { http: false, file: false } }`. No generator code may call `fetch`, `http`, `https`, `net`, or read a path derived from an HTTPS host. Compile each catalog entry independently using:

- `bannerComment` identifying the source schema and forbidding manual edits;
- `style.singleQuote = false` and `style.semi = true`;
- `additionalProperties = false` unless the schema explicitly permits them;
- stable alphabetical source order; and
- a generated `index.ts` with explicit `export type` statements.

Pass every compiled string through the project’s pinned Prettier 3.9.6 API before writing it. Generator output is therefore already in its final checked-in format; running `npm run format` afterward must be byte-idempotent.

The script accepts `--source-root`, `--catalog-root`, and `--output-root`. `--source-root` supplies schema files, while `--catalog-root` supplies the already generated `schemas/catalog.json`; both default to the repository root. It writes LF/NFC output only and removes no file outside the generated output directory it was explicitly given.

Run:

```bash
npm run generate:types
npm run format
npm run typecheck
```

Expected: all six generated TypeScript files compile and are formatted.

- [ ] **Step 7: Complete positive, negative, and drift-precondition tests**

Extend `schema-registry.test.ts` to cover:

- every catalog schema compiles in strict mode;
- each `$id` matches path and version;
- duplicate IDs fail;
- format validation rejects a non-UTC timestamp;
- numeric/string coercion never occurs;
- defaults are never inserted;
- unknown properties fail where forbidden;
- extension data survives where explicitly permitted;
- validation error order is deterministic; and
- valid input retains identical canonical bytes.

Run:

```bash
npm test -- tests/contracts/schema-registry.test.ts
npm test -- tests/tooling/schema-generation.test.ts
npm run generate:schemas
npm run generate:types
git add schemas/catalog.json runtime/src/contracts/generated
npm run generate:schemas
npm run generate:types
git diff --exit-code -- schemas/catalog.json runtime/src/contracts/generated
npm test
```

Expected: tests pass, all references resolve from the local catalog with the network sentinel armed, and regeneration leaves no diff.

- [ ] **Step 8: Commit schemas, registry, and generated types**

Run `npm run format:check` and `git diff --check`, then commit:

```bash
git add schemas runtime/src/contracts/schema-registry.ts runtime/src/contracts/generated runtime/src/index.ts scripts/build-schema-catalog.mjs scripts/generate-contract-types.mjs tests/contracts/schema-registry.test.ts tests/tooling/schema-generation.test.ts tests/fixtures/schemas
git commit -m "feat: add versioned schema registry"
```

---

## Task 5: Add append-only forward migrations with equivalence receipts

**Files:**

- Create: `runtime/src/contracts/migration-registry.ts`
- Create: `tests/contracts/schema-migrations.test.ts`
- Create: `tests/fixtures/migrations/profile-1.0.0.json`
- Create: `tests/fixtures/migrations/profile-1.1.0.json`
- Create: `tests/fixtures/migrations/profile-1.2.0.json`
- Create: `tests/fixtures/migrations/profile-1.0.0.schema.json`
- Create: `tests/fixtures/migrations/profile-1.1.0.schema.json`
- Create: `tests/fixtures/migrations/profile-1.2.0.schema.json`
- Create: `tests/fixtures/migrations/profile-1.0.0-to-1.1.0.migration.json`
- Create: `tests/fixtures/migrations/profile-1.1.0-to-1.2.0.migration.json`
- Modify: `runtime/src/index.ts`

**Interfaces:**

```ts
export type MigrationErrorCode =
  | "UNKNOWN_SCHEMA"
  | "INVALID_SOURCE_VALUE"
  | "INVALID_MIGRATION"
  | "INVALID_MIGRATION_PROGRAM"
  | "AMBIGUOUS_ROUTE"
  | "MIGRATION_NOT_FOUND"
  | "DOWNGRADE_UNSUPPORTED"
  | "EXTENSION_PRESERVATION_FAILED"
  | "INVALID_MIGRATED_VALUE";

export declare class MigrationError extends Error {
  readonly code: MigrationErrorCode;
}

export type ExtensionMapping = {
  source_pointer: string;
  destination_pointer: string;
};

export type MigrationOperation =
  | { op: "set"; path: string; value: JsonValue }
  | { op: "remove"; path: string }
  | { op: "copy" | "move"; from: string; path: string };

export type MigrationProgram = {
  schema_version: "1.0.0";
  program_version: "migration-program-v1";
  from_schema_id: string;
  to_schema_id: string;
  extension_mappings: ExtensionMapping[];
  operations: MigrationOperation[];
};

export type AppliedMigrationResult = {
  kind: "migrated";
  value: JsonValue;
  receipt: MigrationReceipt;
};

export type NoMigrationResult = {
  kind: "unchanged";
  value: JsonValue;
  receipt: null;
};

export type MigrationResult = AppliedMigrationResult | NoMigrationResult;

export type MigrationRegistryOptions = {
  schemas: SchemaRegistry;
  now: () => string;
};

export declare class MigrationRegistry {
  constructor(options: MigrationRegistryOptions);
  registerProgram(sourceBytes: Uint8Array): void;
  migrate(
    fromSchemaId: string,
    toSchemaId: string,
    value: JsonValue,
  ): MigrationResult;
}
```

`ExtensionMapping`, `MigrationOperation`, and `MigrationProgram` above show the generated public shape of `migration-program.schema.json`; implementation imports/re-exports those generated types and does not maintain a handwritten duplicate.

- Consumes: schema IDs known to `SchemaRegistry`, immutable input, and strict JSON bytes for a schema-validated declarative migration program.
- Produces: for an applied forward migration, a new value and receipt chain binding input, output, schema path, validated extension mappings, and a domain-separated hash of the exact canonical program interpreted; for identity, a schema-valid unchanged value and `receipt: null`.
- Invariant: no caller supplies a code hash or executable callback; the interpreter has no process, filesystem, network, clock, randomness, import, or ambient-global operation. Downgrade, invalid source, ambiguity, missing route, mutation, false extension-preservation metadata, or invalid output fails without returning a partial result.

- [ ] **Step 1: Add versioned migration fixtures**

The receipt schema created in Task 4 already requires:

- `schema_version`;
- `receipt_id`;
- `created_at` supplied by an injected clock;
- `input_schema_id` and `output_schema_id`;
- `input_fingerprint` and `output_fingerprint`;
- nonempty ordered `steps`, each with `from_schema_id`, `to_schema_id`, a 43-character `migrator_code_hash`, and a sorted `preserved_extensions` array; and
- each preserved-extension entry containing `source_pointer`, `destination_pointer`, and the domain-separated fingerprint of the byte-equivalent preserved value.

The schema forbids additional properties. Add three test-only profile schemas with stable IDs under `https://example.test/schemas/1.0.0/`, `1.1.0/`, and `1.2.0/`. Version 1.0.0 permits a top-level vendor extension; version 1.1.0 moves it unchanged into an explicit `extensions` object and forbids undeclared top-level fields; version 1.2.0 retains the extension container while adding one required migrated field. Add matching value fixtures so the tests prove one-step and chained preservation rather than only checking synthetic object literals.

Each `.migration.json` fixture passes `migration-program.schema.json`. The 1.0.0-to-1.1.0 program performs ordered `set /extensions {}`, `move /vendor_extension /extensions/vendor_extension`, and `set /schema_version "1.1.0"` operations, then declares `/vendor_extension` → `/extensions/vendor_extension`. The 1.1.0-to-1.2.0 program sets the new required field and schema version and declares `/extensions` → `/extensions`. The registry interprets only these closed operations and derives `migrator_code_hash` from the domain prefix `mahabharata-council:v1\0migration-program\0` plus RFC 8785 program bytes.

- [ ] **Step 2: Write failing migration tests**

Create `tests/contracts/schema-migrations.test.ts` with a frozen injected clock and this core scaffold:

```ts
import { readFileSync } from "node:fs";
import { describe, expect, it } from "vitest";

import { parseStrictJsonSource } from "../../runtime/src/contracts/strict-json.js";
import { MigrationRegistry } from "../../runtime/src/contracts/migration-registry.js";
import { createSchemaRegistry } from "../../runtime/src/contracts/schema-registry.js";
import type { JsonValue } from "../../runtime/src/contracts/json-value.js";

const SCHEMA_100 =
  "https://example.test/schemas/1.0.0/profile.schema.json";
const SCHEMA_110 =
  "https://example.test/schemas/1.1.0/profile.schema.json";
const SCHEMA_120 =
  "https://example.test/schemas/1.2.0/profile.schema.json";

const loadStrictJsonFixtures = (names: readonly string[]): JsonValue[] =>
  names.map((name) =>
    parseStrictJsonSource(
      readFileSync(
        new URL(`../fixtures/migrations/${name}`, import.meta.url),
      ),
    ),
  );

const makeRegistry = (): MigrationRegistry => {
  const schemas = loadStrictJsonFixtures([
    "profile-1.0.0.schema.json",
    "profile-1.1.0.schema.json",
    "profile-1.2.0.schema.json",
  ]);
  schemas.push(
    parseStrictJsonSource(
      readFileSync(
        new URL("../../schemas/1.0.0/common.schema.json", import.meta.url),
      ),
    ),
    parseStrictJsonSource(
      readFileSync(
        new URL(
          "../../schemas/1.0.0/migration-program.schema.json",
          import.meta.url,
        ),
      ),
    ),
    parseStrictJsonSource(
      readFileSync(
        new URL(
          "../../schemas/1.0.0/migration-receipt.schema.json",
          import.meta.url,
        ),
      ),
    ),
  );
  const registry = new MigrationRegistry({
    schemas: createSchemaRegistry(schemas),
    now: () => "2026-08-22T00:00:00Z",
  });
  registry.registerProgram(
    readFileSync(
      new URL(
        "../fixtures/migrations/profile-1.0.0-to-1.1.0.migration.json",
        import.meta.url,
      ),
    ),
  );
  registry.registerProgram(
    readFileSync(
      new URL(
        "../fixtures/migrations/profile-1.1.0-to-1.2.0.migration.json",
        import.meta.url,
      ),
    ),
  );
  return registry;
};

describe("MigrationRegistry", () => {
  it("emits a linked receipt without mutating the input", () => {
    const input = loadStrictJsonFixtures(["profile-1.0.0.json"]).at(0);
    if (input === undefined) {
      throw new Error("Missing migration value fixture");
    }
    const before = structuredClone(input);
    const registry = makeRegistry();

    const result = registry.migrate(SCHEMA_100, SCHEMA_110, input);

    expect(input).toEqual(before);
    expect(result.value).toMatchObject({
      schema_version: "1.1.0",
      extensions: { vendor_extension: { retained: true } },
    });
    expect(result.kind).toBe("migrated");
    if (result.kind !== "migrated") {
      throw new Error("Expected an applied migration");
    }
    expect(result.receipt).toMatchObject({
      input_schema_id: SCHEMA_100,
      output_schema_id: SCHEMA_110,
    });
    expect(result.receipt.input_fingerprint).not.toBe(
      result.receipt.output_fingerprint,
    );
  });

  it("rejects downgrade", () => {
    const input = loadStrictJsonFixtures(["profile-1.1.0.json"]).at(0);
    if (input === undefined) {
      throw new Error("Missing migration value fixture");
    }
    const registry = makeRegistry();
    expect(() =>
      registry.migrate(SCHEMA_110, SCHEMA_100, input),
    ).toThrowError(expect.objectContaining({ code: "DOWNGRADE_UNSUPPORTED" }));
  });
});
```

Before the red command, extend the same test file with cases for:

- a two-step 1.0.0 → 1.2.0 migration with ordered canonical-program hashes;
- invalid 1.0.0 source rejected as `INVALID_SOURCE_VALUE` before an applied route executes;
- invalid 1.1.0 source rejected as `INVALID_SOURCE_VALUE` before the identity path returns;
- a schema-valid operation sequence whose later pointer target is missing, with no partial result;
- output failing its destination schema;
- duplicate and ambiguous routes;
- unknown schemas;
- source equals destination returning `{ kind: "unchanged", value, receipt: null }` without interpreting a program;
- declared extension mappings that resolve to absent or byte-different values failing as `EXTENSION_PRESERVATION_FAILED`;
- unknown nested extension values surviving byte-for-byte and appearing in the receipt with their source pointer, destination pointer, and value fingerprint;
- interpreter operations leaving the original frozen input unchanged;
- semantically equal program JSON producing the same computed code hash, an operation change producing a different hash, and a caller-suggested `codeHash` field being rejected by schema validation rather than trusted;
- unknown operations, executable-looking fields, array pointers, `__proto__`/`prototype`/`constructor` segments, invalid pointer escapes, and missing parent paths failing before mutation; and
- armed `fetch`, process-launch, filesystem-write, clock, and randomness spies remaining untouched during registration and migration;
- deterministic receipt ID under the frozen clock and same inputs; and
- receipt validation through `SchemaRegistry`.

Run:

```bash
npm test -- tests/contracts/schema-migrations.test.ts
```

Expected failure: the migration registry does not exist.

- [ ] **Step 3: Implement the forward-only registry**

The constructor receives `SchemaRegistry` and an injected UTC clock. Fingerprinting always uses P00’s canonical implementation and is not replaceable by callers. `registerProgram` must:

- pass `sourceBytes` through `parseStrictJsonSource` and validate the result against `migration-program.schema.json` before reading any field;
- compute `migrator_code_hash` itself as SHA-256 base64url over the domain prefix `mahabharata-council:v1\0migration-program\0` followed by `canonicalBytes(program)`;
- accept only the closed `set`, `remove`, `copy`, and `move` union; reject every unknown or executable-looking field, including a caller-supplied `codeHash`;
- validate each operation and extension mapping as absolute, non-root, object-only RFC 6901 pointers; reject array indexes, invalid escapes, duplicate mapping endpoints, and `__proto__`, `prototype`, or `constructor` segments;
- reject equal source and destination IDs;
- require both schema IDs to be known;
- parse and compare exact numeric `major.minor.patch` versions from their schema-ID path segments without adding a semver dependency;
- reject a non-increasing destination;
- reject an exact duplicate `(from_schema_id, to_schema_id)` edge while allowing distinct forward branches to be registered; and
- retain only a deep-frozen validated program plus the internally computed semantic code hash.

The interpreter is a closed switch over the four operations. It traverses only own properties on cloned plain JSON objects, deep-clones literal and copied values, requires destination parents to exist, and never evaluates text or calls dynamic import, `Function`, `eval`, process, filesystem, network, clock, randomness, environment, or locale APIs. A later package that needs computation beyond this DSL must introduce a separately reviewed sandboxed migration runner; P00 does not create an executable escape hatch.

Because every edge is strictly semantic-version-increasing, a cycle is unrepresentable; the registry does not expose a separate cycle error that its own invariants could never reach.

`migrate` must:

1. require both requested schema IDs to exist;
2. validate the untouched input against `fromSchemaId`, translating failure to `INVALID_SOURCE_VALUE` before identity, downgrade, or route resolution;
3. for equal IDs, return the already validated original as `{ kind: "unchanged", value, receipt: null }`;
4. reject a target version lower than the source;
5. find exactly one forward path;
6. clone each step input, interpret that program once into a new value, and leave both the original and prior-step values unchanged;
7. validate each interpreted step output against its destination schema;
8. resolve every program-declared source/destination pointer against that step’s input/output, require identical canonical bytes, and record the pointers plus `fingerprint("extension-value", sourceValue)` in sorted order;
9. for a non-identity path, emit one receipt containing the ordered path and internally computed canonical-program hashes;
10. fingerprint the untouched original and final output; and
11. return nothing if any interpretation, preservation, or validation step fails.

Use the exact stable `MigrationErrorCode` contract declared above; wrap source, program, interpreter, pointer, and validation failures without leaking an untyped expected error.

Export migration types from `runtime/src/index.ts`.

- [ ] **Step 4: Run the prewritten adversarial and chain coverage**

Compute each record fingerprint over `{ schema_id, value }` so equal JSON under different schema IDs does not share an identity. Compute `receipt_id` with namespace `migration-receipt` over the completed receipt body before its `receipt_id` field is added, avoiding recursive self-hashing.

Run:

```bash
npm test -- tests/contracts/schema-migrations.test.ts
npm run generate:schemas
npm run generate:types
npm run typecheck
npm test
```

Expected: all migration, registry, canonicalization, and strict-source tests pass.

- [ ] **Step 5: Commit the migration kernel**

Run `npm run format` and `git diff --check`, then commit:

```bash
git add schemas runtime/src/contracts/migration-registry.ts runtime/src/contracts/generated runtime/src/index.ts tests/contracts/schema-migrations.test.ts tests/fixtures/migrations
git commit -m "feat: add append-only schema migrations"
```

---

## Task 6: Enforce repository source invariants and generated-file drift

**Files:**

- Create: `scripts/check-source-invariants.mjs`
- Create: `scripts/check-generated.mjs`
- Create: `scripts/check-source-invariants.d.mts`
- Create: `scripts/check-generated.d.mts`
- Create: `tests/tooling/source-invariants.test.ts`
- Create: `tests/tooling/generated-drift.test.ts`
- Create: `tests/fixtures/source-invariants/decomposed.hex`
- Create: `tests/fixtures/source-invariants/invalid-utf8.hex`
- Create: `tests/fixtures/source-invariants/crlf.hex`
- Create: `tests/fixtures/source-invariants/no-final-lf.hex`

**Interfaces:**

```ts
export type SourceViolation = {
  path: string;
  code:
    | "INVALID_UTF8"
    | "NON_NFC"
    | "NON_LF_LINE_ENDING"
    | "MISSING_FINAL_LF"
    | "DUPLICATE_KEY"
    | "INVALID_JSON";
  detail: string;
};
```

- Consumes: repository root and explicit inclusion/exclusion rules.
- Produces: sorted source violations and a nonzero process status on any violation or generated drift.
- Invariant: checks never rewrite the working tree.

- [ ] **Step 1: Write failing checker tests around temporary directories**

Each fixture `.hex` file contains lowercase hexadecimal bytes plus LF, allowing invalid byte sequences to remain representable as valid repository text. Tests decode those bytes into an isolated `mkdtemp` directory, invoke the checker with `--root`, and remove only that exact temporary directory in `afterEach`.

Create `tests/tooling/source-invariants.test.ts`:

```ts
import { describe, expect, it } from "vitest";

import { runSourceInvariantCheck } from "../../scripts/check-source-invariants.mjs";

describe("source invariant checker", () => {
  it.each([
    ["invalid-utf8.hex", "INVALID_UTF8"],
    ["decomposed.hex", "NON_NFC"],
    ["crlf.hex", "NON_LF_LINE_ENDING"],
    ["no-final-lf.hex", "MISSING_FINAL_LF"],
  ])("reports %s as %s", async (fixture, code) => {
    const root = await materializeHexFixture(fixture);
    await expect(runSourceInvariantCheck(root)).resolves.toEqual([
      expect.objectContaining({ code }),
    ]);
  });
});
```

Also create `tests/tooling/generated-drift.test.ts` before the drift checker exists. It copies the minimal schema/catalog/generated source set to a temporary tree, asserts a clean result, mutates one generated byte and expects `BYTE_MISMATCH`, deletes one file and expects `MISSING_FILE`, and adds one file and expects `EXTRA_FILE`. Every case validates the temporary root before cleanup and asserts the real working tree is unchanged.

Add `scripts/check-source-invariants.d.mts` and `scripts/check-generated.d.mts` before the first typecheck. Their declarations mirror the actual exported functions, violation records, argument types, and return types; do not suppress either module with `any`.

Run:

```bash
npm test -- tests/tooling/source-invariants.test.ts tests/tooling/generated-drift.test.ts
```

Expected failure: both checker modules do not exist.

- [ ] **Step 2: Implement the read-only source checker**

`check-source-invariants.mjs` scans:

- extensions `.json`, `.md`, `.ts`, `.mts`, `.cts`, `.js`, `.mjs`, `.cjs`, `.yml`, `.yaml`, `.toml`, `.txt`, and `.py`;
- exact root names `.nvmrc`, `.node-version`, `.editorconfig`, `.gitattributes`, and `.gitignore`.

It excludes only `.git/`, `node_modules/`, `dist/`, `coverage/`, and `eval/private/`. Paths are normalized to POSIX separators and sorted by code point before reading.

For every included file:

1. decode with fatal UTF-8;
2. reject carriage returns;
3. reject an empty included source and otherwise require LF termination;
4. require `text.normalize("NFC") === text`; and
5. for `.json`, call the built `parseStrictJsonSource` so duplicate keys and decoded non-NFC strings are caught.

Export `runSourceInvariantCheck(root): Promise<SourceViolation[]>` and keep CLI exit behavior in a small `if (import.meta.url === pathToFileURL(process.argv[1]).href)` block. CLI output is one stable line per violation: `<path>: <code>: <detail>`.

Run:

```bash
npm test -- tests/tooling/source-invariants.test.ts
```

Expected: the source-invariant suite now passes while the separately runnable generated-drift suite remains red until Step 3.

- [ ] **Step 3: Implement byte-for-byte generated drift checking**

`check-generated.mjs` must:

1. create a unique temporary output root with `mkdtemp`;
2. invoke the catalog generator with the repository source root and temporary output root, then invoke the type generator with the repository schema root, the temporary catalog root, and the temporary output root;
3. enumerate expected generated files and checked-in files;
4. report missing, extra, or byte-different files in sorted order;
5. always clean only its validated temporary root in `finally`; and
6. never call a shell or accept an unvalidated removal path.

Cover catalog and `runtime/src/contracts/generated/`. Its process status is nonzero on drift.

Run:

```bash
npm test -- tests/tooling/generated-drift.test.ts
```

Expected: clean, mismatch, missing, and extra-file cases pass and the checker never rewrites the source tree.

- [ ] **Step 4: Run repository and negative checks**

Run:

```bash
npm run check:sources
npm run check:generated
npm test -- tests/tooling
npm run typecheck
npm test
```

Expected: the repository passes; every temporary adversarial fixture produces its exact stable code; checked-in generated files match regenerated bytes.

- [ ] **Step 5: Commit invariant enforcement**

Run `npm run format`, repeat both check scripts, and run `git diff --check`. Commit:

```bash
git add scripts/check-source-invariants.mjs scripts/check-source-invariants.d.mts scripts/check-generated.mjs scripts/check-generated.d.mts tests/tooling tests/fixtures/source-invariants
git commit -m "test: enforce source and generation invariants"
```

---

## Task 7: Gate the foundation on provider-independent cross-platform CI

**Files:**

- Create: `.github/workflows/ci.yml`
- Create: `tests/tooling/ci-policy.test.ts`

**Interfaces:**

- Consumes: exact Node version, `package-lock.json`, and `npm run ci`.
- Produces: one deterministic, non-live-provider result on Ubuntu, macOS, and Windows after ordinary action and package acquisition.
- Invariant: CI has read-only repository permission, no persisted checkout credentials, no provider credentials, and no live evaluation step.

- [ ] **Step 1: Write the failing CI-policy test**

Inspect `.github/workflows/ci.yml` with a purpose-built strict text policy checker, avoiding a new YAML dependency. The test must assert:

- `permissions:\n  contents: read`;
- `persist-credentials: false`;
- exactly one `npm ci`;
- exactly one `npm run ci`;
- all three pinned runner labels;
- both action references are 40-character SHAs; and
- absence of `pull_request_target`, `workflow_run`, `secrets.`, `npm install`, provider invocations, and live-evaluation script names.

Run:

```bash
npm test -- tests/tooling/ci-policy.test.ts
```

Expected failure: the workflow does not exist.

- [ ] **Step 2: Add the pinned CI workflow**

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  pull_request:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  foundation:
    name: Foundation (${{ matrix.os }})
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os:
          - ubuntu-24.04
          - macos-15
          - windows-2025
    steps:
      - name: Check out repository
        uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1
        with:
          persist-credentials: false
      - name: Set up Node.js
        uses: actions/setup-node@820762786026740c76f36085b0efc47a31fe5020
        with:
          node-version-file: .node-version
          cache: npm
      - name: Install locked dependencies
        run: npm ci
      - name: Verify foundation
        run: npm run ci
```

There is no Python, Podman, provider login, network call beyond package/action acquisition, benchmark, deployment, release, or write credential in this workflow.

- [ ] **Step 3: Add workflow mutation cases**

In `ci-policy.test.ts`, refactor the checker to accept text and mutate one property per case:

- replace a SHA with `v6`;
- enable persisted credentials;
- add `contents: write`;
- replace `npm ci` with `npm install`;
- add `pull_request_target`; and
- add `secrets.PROVIDER_TOKEN`.

Each mutation must fail for a distinct code so the test proves policy rather than matching one long snapshot.

- [ ] **Step 4: Run the complete local CI command**

Run:

```bash
npm run ci
git diff --check
```

Expected: source, generation, format, type, test, and build gates all pass locally.

- [ ] **Step 5: Commit CI**

```bash
git add .github/workflows/ci.yml tests/tooling/ci-policy.test.ts
git commit -m "ci: verify foundation across platforms"
```

Do not push during this task. Remote CI is verified only after the user authorizes opening or updating the implementation pull request.

---

## Task 8: Perform the foundation acceptance audit

**Files:**

- Modify only if a preceding verification exposes a defect.
- Inspect: every file introduced in Tasks 1–7.

**Interfaces:**

- Consumes: the complete P00 branch.
- Produces: local acceptance evidence and a clean handoff to P01/P03 planning.
- Invariant: a failed check is fixed and rerun; it is never described as passing.

- [ ] **Step 1: Reinstall from the lockfile**

Use a unique temporary npm cache so the lockfile path is exercised without deleting unrelated user state:

```bash
task_npm_cache="$(mktemp -d /tmp/mahabharata-council-npm-cache.XXXXXX)"
npm ci --cache "$task_npm_cache"
npm ls --depth=0
```

Expected: exact dependencies install and `npm ls` exits zero.

- [ ] **Step 2: Run the full deterministic suite**

```bash
npm run ci
env TZ=UTC LANG=C npm test
env TZ=Pacific/Kiritimati LANG=en_US.UTF-8 npm test
```

Expected: all commands pass and canonical fingerprints are identical in both environments.

- [ ] **Step 3: Resolve any acceptance defect before the clean audit**

If Step 1 or Step 2 fails, fix only the implicated P00 files, rerun the failing command and `npm run ci`, then inspect the exact boundary:

```bash
git status --short
git diff --name-only
git ls-files --others --exclude-standard
```

Expected: every changed path is owned by P00 and directly explains the defect. If an unrelated path appears, stop without staging it. Stage the bounded P00 surface and inspect the staged diff:

```bash
git add -A -- .github runtime schemas scripts tests package.json package-lock.json tsconfig.base.json tsconfig.json tsconfig.build.json vitest.config.ts prettier.config.mjs .editorconfig .gitattributes .node-version .nvmrc
git diff --cached --check
git diff --cached --stat
git commit -m "fix: satisfy foundation acceptance checks"
```

Do not create an empty audit commit. After any defect commit, restart Task 8 at Step 1. Apply the same fix, commit, and restart loop if a later audit command fails; no clean-worktree expectation is evaluated while a known defect remains uncommitted.

- [ ] **Step 4: Verify clean regeneration and source bytes**

```bash
npm run generate:schemas
npm run generate:types
git diff --exit-code
npm run check:sources
npm run check:generated
```

Expected: generation changes no checked-in bytes, and both invariant checkers pass.

- [ ] **Step 5: Inspect the change boundary**

```bash
git status --short
git diff --check
git log --oneline --decorate -8
if rg -n 'TO''DO|TB''D|FIX''ME|X''XX' runtime schemas scripts tests .github; then
  exit 1
else
  task_marker_status=$?
  test "$task_marker_status" -eq 1
fi
rg -n "outperform|superior|better performance|performance-qualified" runtime schemas scripts tests .github || {
  task_claim_status=$?
  test "$task_claim_status" -eq 1
}
```

Expected: the worktree is clean after committed tasks; whitespace check passes; the inverted marker assertion exits zero only when the scan is empty; the claim scan exits zero for either matches or a clean no-match status but still fails on an `rg` execution error. Every printed performance-related match is then inspected and is either an explicit denial in a test fixture or removed.

- [ ] **Step 6: Confirm no forbidden behavior entered P00**

Inspect tests and code to confirm:

- no router, automatic delegation, model allowlist, provider credential, network call, benchmark score, public claim, installer mutation, or production private key exists;
- no global Git or authentication setting changed;
- no generated type is manually maintained;
- no validator applies defaults, coercion, or removal;
- no migration permits downgrade or overwrites input; and
- no canonical hash uses locale, time zone, object insertion order, or ordinary `JSON.stringify`.

- [ ] **Step 7: Map specifications to passing tests**

| Requirement | Test evidence |
|---|---|
| Parent Section 8.1 strict UTF-8/NFC/LF/duplicate-free JSON | `tests/contracts/strict-json.test.ts` and `tests/tooling/source-invariants.test.ts` |
| Parent Section 8.1 RFC 8785 hashing | `tests/contracts/canonical-json.test.ts` with RFC vectors and property tests |
| Parent Section 8.1 stable schema IDs and explicit forward migrations | `tests/contracts/schema-registry.test.ts` and `tests/contracts/schema-migrations.test.ts` |
| Parent Section 8.4 deterministic, hermetic generation | `tests/tooling/schema-generation.test.ts` and `tests/tooling/generated-drift.test.ts` |
| Parent Section 8.5 Node/TypeScript runtime division | `tests/tooling/smoke.test.ts` and locked build configuration |
| Parent Section 13 P00 subset: schema/unit tests and cross-platform CI | contract suites, `.github/workflows/ci.yml`, and `tests/tooling/ci-policy.test.ts` |
| Addendum Section 11.5 record integrity and no silent rehash | migration fingerprint and immutability cases |
| Addendum Section 16.1 canonicalization, schema, migration, and mutation tests | all P00 contract and tooling suites |

- [ ] **Step 8: Record the acceptance result without making a public claim**

Report the exact commands, pass counts, operating systems verified locally versus remotely, and any remote CI still pending. The only permitted completion statement is:

> P00’s deterministic contract-kernel checks pass in the environments named by the recorded test output. No host compatibility, managed-runtime, or performance qualification was evaluated.

## P00 Exit Criteria

- `npm ci` and `npm run ci` pass from the locked root project.
- Strict JSON rejects invalid UTF-8, CRLF, missing LF, duplicate keys, syntax errors, and non-NFC decoded keys/values.
- RFC 8785 fixtures and property tests establish deterministic canonical bytes.
- Fingerprints are domain-separated, namespaced, SHA-256 base64url identifiers.
- Every schema has a stable versioned ID, compiles in strict Ajv 2020-12 mode, and generates drift-checked TypeScript through an HTTP/file-disabled local `$id` resolver.
- Forward migrations validate source and every step, interpret and hash the same canonical declarative program, verify declared extension-pointer equivalence, emit linked receipts, and reject downgrade, ambiguity, missing routes, and partial output.
- Repository source and generated outputs pass read-only invariant checks.
- Provider-independent, non-live CI is pinned and defined for Ubuntu, macOS, and Windows.
- Automatic routing remains disabled and the performance evidence state remains `unverified`.
- P01 and P03 may consume the kernel only after G1 is observed on the implementation pull request.
