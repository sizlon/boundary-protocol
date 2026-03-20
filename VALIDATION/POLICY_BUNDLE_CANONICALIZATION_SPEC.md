# POLICY_BUNDLE_CANONICALIZATION_SPEC

## Status
Normative (Binding)

## Purpose
Define strict canonical JSON and digest generation rules for policy bundle signature validation, such that identical semantic input yields identical canonical bytes and identical digest across all runtimes.

## Canonicalization Rules

### Input Domain
1. Canonicalization input MUST be valid JSON text representing exactly one JSON value.
2. Input JSON MUST be parsed into an abstract syntax tree (AST) before canonicalization. Canonicalization MUST operate on the AST, not on raw textual input.
3. Supported JSON value types are:
   - object
   - array
   - string
   - number
   - boolean
   - null
4. Numbers are restricted by this specification to **integers only**.
5. Any non-integer numeric form is invalid.

### Object
1. Object member names (keys) MUST be unique.
2. Duplicate keys MUST be rejected.
3. Object keys MUST be serialized in ascending lexicographic order of their UTF-8 byte sequences.
4. Object serialization form is exact:
   - `{` + comma-separated key/value pairs + `}`
   - no whitespace before/after separators
5. Member separator is `,` and key/value separator is `:` with no surrounding spaces.

### String
1. Strings MUST be valid Unicode scalar value sequences.
2. Invalid Unicode sequences MUST be rejected, including lone surrogates (`U+D800`–`U+DFFF`).
3. Unicode normalization is **not applied**. Input code points are preserved.
4. Strings MUST be serialized in UTF-8.
5. Escaping rules:
   - `"` MUST be escaped as `\\"`
   - `\\` MUST be escaped as `\\\\`
   - Control characters `U+0000`..`U+001F` MUST be escaped as `\\u00xx` (lowercase hex)
6. Other characters MUST be emitted unescaped.
7. Alternative escape spellings for required escapes are not allowed in canonical output.

### Number
1. Floating-point values are forbidden.
2. Exponent notation is forbidden.
3. Decimal fraction notation is forbidden.
4. Canonical integer grammar is:
   - `0`
   - or `-?[1-9][0-9]*`
5. `-0` is forbidden.
6. Leading zeros are forbidden (except the single literal `0`).
7. Integer range is restricted to signed 64-bit:
   - minimum: `-9223372036854775808`
   - maximum: `9223372036854775807`
8. Any number outside range or non-canonical form MUST be rejected.

### Boolean
1. Canonical boolean literals are exactly `true` and `false` (lowercase).

### Null
1. Canonical null literal is exactly `null` (lowercase).

### Array
1. Array element order MUST be preserved exactly.
2. No reordering is allowed.
3. Canonical array form is exact:
   - `[` + comma-separated canonical elements + `]`
   - no whitespace before/after separators

## Encoding to Bytes
1. Canonical JSON text MUST be encoded as UTF-8.
2. UTF-8 BOM is forbidden.
3. Digest input is exactly the resulting UTF-8 byte sequence, byte-for-byte.

## Digest Generation
1. Hash algorithm is SHA-256.
2. Digest input is the canonical UTF-8 byte sequence.
3. Digest output format is lowercase hexadecimal, 64 characters.
4. For policy bundle signing, canonicalization input MUST be `manifest_without_signature` (top-level `signature` member removed).

## Rejection Rules
Canonicalization / validation MUST fail if any of the following is present:
1. Duplicate object keys.
2. Invalid Unicode or lone surrogate code points.
3. Non-integer number.
4. Non-canonical integer form (`-0`, exponent, fraction, leading zeros).
5. Unsupported or non-JSON type.
6. UTF-8 BOM.
7. Any output form violating key ordering or escape requirements.

## Test Vectors

### Vector 1 (Simple Object)
- Canonical JSON:
```json
{"a":1,"b":"x","c":true,"d":null}
```
- SHA-256:
`383863b432c7b9f3a251f47a5dda29e6ad051faa7f5060eb876c8c485f197ba5`

### Vector 2 (Nested Structure)
- Canonical JSON:
```json
{"meta":{"id":"core_default","version":1},"rules":[{"k":"A"},{"k":"B"}]}
```
- SHA-256:
`506ec8726269e1cc0c787391cf86925f676102e686d25ebb59749d7edc8d96a5`

### Vector 3 (Edge Case: Control Escape, Empty Containers)
- Canonical JSON:
```json
{"arr":[],"ctrl":"\u0001","obj":{},"z":0}
```
- SHA-256:
`89309ab274a14b8a3af97ca2a5ba00567ffb6a8f37aad8e95a5d55a9b07f3c84`

## Final Statement
Any runtime that does not produce byte-identical canonical JSON and digest-identical SHA-256 outputs under this specification is non-conformant and MUST be rejected for policy bundle signature validation.
