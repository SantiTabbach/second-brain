---
tags: [areas, software, tooling]
author: Santi Tabbach
created: 2026-04-28
modified: 2026-04-28
---

Un **symlink** (symbolic link, enlace simbólico) es básicamente un “atajo” a otro archivo o carpeta.

No duplica el contenido: solo apunta a la ubicación original.

Ejemplo:
- Tenés una carpeta real: `/Users/santi/shared/skills`
- Creás un symlink en otro lugar: `/Users/santi/project/skills`

Cuando alguien entra a `project/skills`, en realidad está accediendo a `shared/skills`.

---
#### Cómo crearlo

Sintaxis:
```bash
ln -s <origen> <destino>
```

Ejemplo
```bash
ln -s ~/best-practices/skills ./skills
```
Eso crea en el directorio actual un link llamado `skills` que apunta a `~/best-practices/skills`.

#### Cómo verlo

```bash
ls -l
```

Vas a ver algo así:

```bash
skills -> /Users/santi/best-practices/skills
```

La flecha `->` indica que es un symlink.

