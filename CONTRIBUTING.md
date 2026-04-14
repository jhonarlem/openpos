# Contribuir a OpenPOS

Gracias por tu interés en contribuir. Este documento explica cómo configurar el proyecto, las convenciones que seguimos y el proceso para enviar cambios.

---

## Tabla de Contenidos

1. [Prerrequisitos](#prerrequisitos)
2. [Comenzar](#comenzar)
3. [Estructura del Proyecto](#estructura-del-proyecto)
4. [Flujo de Trabajo](#flujo-de-trabajo)
5. [Convenciones de Código](#convenciones-de-código)
6. [Mensajes de Commit](#mensajes-de-commit)
7. [Proceso de Pull Request](#proceso-de-pull-request)
8. [Reportar Problemas](#reportar-problemas)

---

## Prerrequisitos

| Herramienta | Versión |
|------------|---------|
| [Bun](https://bun.sh) | Última |
| Node.js | 22+ |
| Git | Cualquier versión reciente |

> Windows es la plataforma principal de desarrollo. El script de compilación `build.bat` está diseñado para Windows x64.

---

## Comenzar

```bash
# 1. Haz fork del repositorio en GitHub, luego clona tu fork
git clone https://github.com/avalontm/openpos.git
cd openpos

# 2. Instala las dependencias
npm install

# 3. Llena la base de datos con datos de ejemplo
npm run seed

# 4. Inicia la TUI en modo desarrollo
npm run dev
```

Deberías ver la pantalla de inicio de sesión. Las credenciales por defecto están definidas en `src/db/seed.ts`.

---

## Estructura del Proyecto

```
openpos/
├── assets/          # Recursos estáticos (logo.png, banner.png)
├── images/         # Capturas de pantalla para el README
├── src/
│   ├── app.tsx    # Punto de entrada — enrutamiento de argumentos CLI + bootstrap de TUI
│   ├── cli.ts     # Interfaz de línea de comandos para importar productos
│   ├── db/        # Esquema de Drizzle ORM, migraciones, seed
│   ├── modules/   # Módulos de pantalla (PosScreen, ReportsScreen, etc.)
│   ├── shared/    # Tema, hook de layout, componentes compartidos (BgBox, etc.)
│   ├── store/     # Stores de Zustand (auth, cart)
│   ├── types/     # Definiciones de tipos TypeScript
│   └── utils/     # Utilidades varias
├── config.json    # Configuración de impresora y plantilla (no se hace commit con secretos)
├── productos.csv # Archivo de ejemplo para importar productos
├── build.bat      # Script de compilación para Windows → pos.exe
└── pos.exe        # Ejecutable compilado
```

---

## Flujo de Trabajo

### Ramas

| Rama | Propósito |
|------|-----------|
| `main` | Estable, siempre lista para lanzamiento |
| `feat/<nombre>` | Nuevas funcionalidades |
| `fix/<nombre>` | Correcciones de errores |
| `chore/<nombre>` | Herramientas, dependencias, refactores sin cambio visible |

Siempre haz rama desde `main` y abre un PR hacia `main`.

### Ejecutar y Compilar

```bash
# Modo desarrollo
npm run dev

# Compilar ejecutable independiente
build.bat
```

### Cambios en la Base de Datos

Las migraciones se manejan con Drizzle ORM. Si cambias `src/db/schema.ts`:

```bash
# Generar archivos de migración
bunx drizzle-kit generate

# Aplicar migraciones
bunx drizzle-kit migrate
```

No editares manualmente los archivos de migración.

---

## Convenciones de Código

- **Lenguaje**: TypeScript con strict mode habilitado, sin `any` a menos que sea absolutamente necesario.
- **Formato**: No se fuerza formatter aún; adapta el estilo existente (indent de 2 espacios, comas finales).
- **Componentes**: Solo componentes funcionales de Ink. Sin componentes de clase.
- **Estado**: Stores de Zustand en `src/store/`. No uses estado local en componentes para nada que cruce fronteras de pantallas.
- **Tema**: Todos los colores deben venir de `src/shared/theme.ts`. Sin valores hex hardcodeados.
- **Tamaños de layout**: Toda la lógica de dimensiones pertenece en `src/shared/useLayout.ts`. Los componentes reciben `width`, `height`, `cols`, `itemH` como props.
- **Sin emojis en UI**: La TUI usa caracteres de dibujo de cajas y elementos de bloque Unicode, no emojis.
- **Español en código**: Nombres de variables, comentarios e identificadores de tipo en español. Las cadenas visibles al usuario permanecen en español.

---

## Mensajes de Commit

Sigue la especificación de [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>(<alcance>): <descripción corta>

[cuerpo opcional]
[pie opcional]
```

Tipos comunes: `feat`, `fix`, `chore`, `refactor`, `docs`, `style`, `test`.

Ejemplos:

```
feat(modulos): añadir pantalla de reportes
fix(carrito): corregir cursor cuando se eliminan artículos
chore(deps): actualizar drizzle-orm a 0.32
docs: añadir guía CONTRIBUTING
```

---

## Proceso de Pull Request

1. Asegúrate de que `npm run dev` inicie sin errores en una base de datos limpia (`npm run seed`).
2. Si cambiaste el layout de alguna pantalla, incluye una captura en la descripción del PR.
3. Referencia cualquier issue relacionado con `Closes #<número>`.
4. Mantén los PRs enfocados — una funcionalidad o corrección por PR.
5. Un mantenedor revisará y puede solicitar cambios antes de hacer merge.

---

## Reportar Problemas

Abre un issue en GitHub con:

- Un título claro que describa el problema.
- Pasos para reproducir.
- Tamaño de terminal y SO.
- Si estás usando el ejecutable compilado `pos.exe` o `npm run dev`.
- Cualquier salida de error relevante.

---

MIT License — Copyright (c) 2026 AvalonTM