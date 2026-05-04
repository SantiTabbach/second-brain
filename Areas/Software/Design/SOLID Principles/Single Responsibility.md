---
tags: [areas, software, design, solid-principles]
author: Santi Tabbach
created: 2026-05-04
modified: 2026-05-04
---

Un objeto (clase, módulo o funcion) debe hacer una sola cosa y debe hacerla bien.

Bad
```typescript

const createUser = (user: User): Promise<Result | Error> => {
	const { password } = user;
	
	if(passwordRegex.validate(password)) {
		return await create(user)
	} else {
		throw new Error("Password is invalid.")
	}
}

```

Good
```typescript
const validatePassword = (password: string) => {
	
}
```