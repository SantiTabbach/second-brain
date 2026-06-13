---
tags:
  - areas
  - software
  - typescript
  - types
  - programming-languages
author: Santi Tabbach
created: 2026-04-27
modified: 2026-04-27
---

```typescript
/**
 * Recursively makes all properties of an object optional.
 *
 * @template T - The type to make deeply partial.
 *
 * Credits: https://flowbite.com/
 */
export type DeepPartial<T> = T extends object
	? { [P in keyof T]?: DeepPartial<T[P]> }
	: T;

/**
 * Removes index signatures from an object type, keeping only explicitly defined keys.
 *
 * @template T - The object type to process.
 *
 * Credits: https://flowbite.com/
 */
export type RemoveIndexSignature<T> = {
	[K in keyof T as string extends K ? never : K]: T[K];
};

/**
 * Creates a type that allows a fixed set of values from `T`,
 * but also allows any arbitrary string.
 *
 * @template T - The base type for the enum.
 *
 * Credits: https://flowbite.com/
 */
export type DynamicStringEnum<T> = T | (string & {});

/**
 * Extracts the keys of an object type as a `DynamicStringEnum`,
 * excluding index signatures.
 *
 * @template T - The object type from which to extract keys.
 *
 * Credits: https://flowbite.com/
 */
export type DynamicStringEnumKeysOf<T extends object> = DynamicStringEnum<
	keyof RemoveIndexSignature<T>
>;
```