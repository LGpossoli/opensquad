# OpenSquad

Crea squads de agentes de IA que trabajan juntos — desde tu terminal.

## Instalación

**Requisitos:** Node.js 20+ y [Claude Code](https://claude.com/claude-code).

```bash
npx opensquad init
```

## Actualizar

```bash
npx opensquad update
```

## Cómo Usar

Abre esta carpeta en Claude Code y escribe:

```
/opensquad
```

Esto abre el menú principal. Desde ahí puedes crear squads, ejecutarlos, abrir la Oficina Virtual y más.

También puedes ser directo — describe lo que necesitas en lenguaje natural:

```
/opensquad crea un squad para escribir posts en LinkedIn sobre IA
/opensquad ejecuta el squad my-squad
```

## Crear un Squad

Escribe `/opensquad` y elige "Crear squad" en el menú, o ve directo:

```
/opensquad crea un squad para [lo que necesitas]
```

El Arquitecto hará algunas preguntas, diseñará el squad y configurará todo automáticamente.

## Ejecutar un Squad

Escribe `/opensquad` y elige "Ejecutar squad" en el menú, o ve directo:

```
/opensquad ejecuta el squad <nombre-del-squad>
```

El squad ejecuta automáticamente, pausando solo en los checkpoints de decisión.

## Oficina Virtual

La Oficina Virtual es una interfaz visual 2D que muestra a tus agentes trabajando en tiempo real.

**Paso 1 — Genera el dashboard** (en Claude Code):

```
/opensquad dashboard
```

**Paso 2 — Sírvelo localmente** (en terminal):

```bash
npx serve squads/<nombre-del-squad>/dashboard
```

**Paso 3 —** Abre `http://localhost:3000` en tu navegador.
