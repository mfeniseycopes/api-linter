---
rule:
  aip: 234
  name: [core, '0234', response-resource-field]
  summary:
    Batch Update RPCs must have a repeated field for the resource in the response.
permalink: /234/response-resource-field
redirect_from:
  - /0234/response-resource-field
---

# Batch Update methods: Resource field

This rule enforces that all `BatchUpdate` methods have a repeated field in the
response message for the resource itself, as mandated in [AIP-234][].

## Details

This rule looks at any message matching `BatchUpdate*Response` and complains if
there is no repeated field of the resource's type with the expected field name.

## Examples

**Incorrect** code for this rule:

```proto
// Incorrect.
message BatchUpdateBooksResponse {
  // `repeated Book books` is missing.
}
```

```proto
// Incorrect.
message BatchUpdateBooksResponse {
  Book books = 1;  // Field should be repeated.
}
```

**Correct** code for this rule:

```proto
// Correct.
message BatchUpdateBooksResponse {
  repeated Book books = 1;
}
```

## Disabling

If you need to violate this rule, use a leading comment above the message (if
the resource field is missing) or above the field (if it is improperly named).
Remember to also include an [aip.dev/not-precedent][] comment explaining why.

```proto
// (-- api-linter: core::0234::response-resource-field=disabled
//     aip.dev/not-precedent: We need to do this because reasons. --)
message BatchUpdateBooksResponse {
  // `repeated Book books` is missing.
}
```

If you need to violate this rule for an entire file, place the comment at the
top of the file.

[aip-234]: https://aip.dev/234
[aip.dev/not-precedent]: https://aip.dev/not-precedent
