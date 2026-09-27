---
ai_hash: 23f4dc90f2c10b20
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
created: 2026-06-16
entities: []
source: LUZ-154613 session 2026-06-16
status: seedling
tags:
- java
- overloading
- varargs
- gotcha
- compile-error
title: Java int-vs-Object-vararg overload call is ambiguous — pass explicit array
  to disambiguate
type: lesson
---

# Java int-vs-Object-vararg overload call is ambiguous — pass explicit array to disambiguate

When a class has overloaded constructors/methods where one takes `(... , int, Object...)` and another takes `(... , Object...)`, calling it with an `int` as the last explicit arg is AMBIGUOUS — javac cannot decide between binding the int to the `int` parameter (empty vararg) vs boxing it into the `Object...` vararg. Error: 'reference to X is ambiguous, both constructor ... match'.

Disambiguate by passing an explicit array for the varargs slot so the arity picks the intended overload:
`super(code, bundlePath, HttpStatus.SC_INTERNAL_SERVER_ERROR, new Object[0]);`
This forced luz-docs ParallelizeCountException to compile against LocalizedRuntimeException, which has both `(String,String,Object...)` and `(String,String,int,Object...)` ctors — exactly how the sibling DocumentException already calls it (`new Object[1]`). Seen LUZ-154613.

## Related

- [[Divide-and-Conquer Visible-Document Count]]

%% ai-graph-start %%

**Related notes:**
- [[Lombok one bad symbol cascades into hundreds of phantom missing-method errors]]
- [[Java List.of rejects null elements (NPE); use Arrays.asList for null-tolerant varargs]]
- [[Problem of class cast exception]]
- [[JSON-P createArrayBuilder(Collection) rejects built JsonValues]]
- [[A delegating overload changes less code than widening an existing method signature]]

%% ai-graph-end %%