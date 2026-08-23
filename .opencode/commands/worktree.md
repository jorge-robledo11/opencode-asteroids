---
description: Crea un git worktree a partir de una descripción
agent: build
---

La solicitud del usuario es:

<solicitud>
$ARGUMENTS
</solicitud>

Analiza la solicitud y genera un nombre corto y descriptivo para el worktree.

El nombre debe:
- estar en minúsculas;
- usar kebab-case;
- contener únicamente letras, números y guiones;
- no contener espacios ni barras;
- ser corto y representar la intención de la solicitud.

Ejecuta exactamente un único comando:

git worktree add .worktrees/<nombre-generado>

Reglas estrictas:

- Ejecuta exactamente un comando de shell.
- El único comando permitido es:
  git worktree add .worktrees/<nombre-generado>
- No ejecutes git status.
- No ejecutes git branch.
- No ejecutes git log.
- No inspecciones el repositorio.
- No cambies de directorio.
- No modifiques archivos.
- No crees archivos adicionales.
- No pidas confirmación.
- No expliques tu razonamiento.
- No hagas nada antes ni después de crear el worktree.
- Si la solicitud contiene espacios, trata todo el valor de $ARGUMENTS como una única solicitud.

Ejemplo:

/worktree agregar autenticación con google al login

Debe derivar un nombre como:

google-auth-login

Y ejecutar únicamente:

git worktree add .worktrees/google-auth-login