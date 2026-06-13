---
tags:
  - areas
  - software
  - typescript
  - utils
  - programming-languages
author: Santi Tabbach
created: 2026-04-27
modified: 2026-05-08
---

[[IS_OBJECT]]
```typescript
/**
 * Recursively creates a deep clone of the given object.
 *
 * @param <T> The type of the source object.
 * @param source The object to be deeply cloned.
 * @return A deep copy of the given object.
 *
 * Credits: https://flowbite.com/
 */
export function cloneDeep<T>(source: T): T {
	if (!isObject(source)) {
		return source;
	}

	const output: Record<string, unknown> = {};

	for (const key in source) {
		output[key] = cloneDeep(source[key]);
	}

	return output as T;
}
```