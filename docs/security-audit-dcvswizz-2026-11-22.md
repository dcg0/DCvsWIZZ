# Auditoría de seguridad y calidad — DCvsWIZZ

**Repositorio:** `dcg0/DCvsWIZZ`  
**Fecha:** 2026-11-22  
**Rama auditada:** `main`  
**Alcance:** CI/CD, documentación, dependencias, secretos, estructura, Android, archivos grandes, GitHub Pages y protección de rama.

## Resumen ejecutivo

La auditoría encontró y corrigió una causa determinista del fallo de compilación Android: el proyecto usa Android Gradle Plugin `9.4.1`, que requiere Gradle `9.6.0` o posterior, mientras el wrapper apuntaba a `8.7`. También se corrigieron enlaces de documentación que apuntaban al propietario equivocado (`DebianS4`), una URL local incompleta, la insignia de licencia y la referencia a una release/APK que no existía. Se añadió `pnpm-lock.yaml` y se eliminó la opción Gradle AndroidX obsoleta.

El repositorio queda **REQUIERE REVISIÓN**, no aprobado todavía: la protección de `main` no está habilitada, las APIs de Code Scanning, Secret Scanning y Dependabot devuelven `403 Resource not accessible by integration`, no hay licencia declarada ni releases publicadas, y la nueva ejecución de `Build APK` debe terminar en verde después del push.

## Correcciones aplicadas

- `android/gradle/wrapper/gradle-wrapper.properties`: Gradle `8.7` → `9.6.0`.
- `android/gradle.properties`: `android.useAndroidX=false` → `true` para evitar una opción obsoleta de AGP.
- `README.md`: enlaces de propietario, Pages, Actions, repositorio y releases corregidos; URL `localhost:3000` reparada; instrucciones alineadas con pnpm; no se anuncia una release inexistente; insignia de licencia marcada como no declarada.
- `SECURITY.md`: enlace de Security Advisories corregido a `dcg0/DCvsWIZZ`.
- `pnpm-lock.yaml`: generado con `pnpm install --lockfile-only --ignore-scripts`.
- Este informe se incorpora al repositorio para conservar la evidencia de la auditoría.

## Resultados técnicos

### CI/CD

- `Deploy Pages`: ejecuciones anteriores verificadas como exitosas.
- GitHub Pages: estado `built`, URL pública `https://dcg0.github.io/DCvsWIZZ/`, HTTPS forzado.
- `Build APK`: las ejecuciones anteriores fallaban por la incompatibilidad AGP/Gradle. La corrección se envía en este cierre y debe verificarse con la nueva ejecución de GitHub Actions.
- Workflow Android: usa `actions/setup-java@v4` con Temurin 17, instala Android SDK Platform 35 y Build Tools 35.0.0, y declara `permissions: contents: read`.
- Workflow Pages: usa permisos mínimos necesarios (`contents: read`, `pages: write`, `id-token: write`) y concurrencia para cancelar despliegues obsoletos.

### Pruebas locales

Pasaron:

- `pnpm install --lockfile-only --ignore-scripts`.
- `pnpm audit`: **No known vulnerabilities found**.
- `pnpm run build`.
- `node --check server.js`.
- `node --check` en los JavaScript de `public/`.
- Parsing de `public/index.html` y `public/game/game.html`.
- `git diff --check`.
- Búsqueda de archivos mayores de 50 MB: ninguno.
- Búsqueda de nombres rastreados asociados a `.env`, secretos, credenciales, tokens, contraseñas o claves privadas: ninguno.

La compilación Android avanzó hasta `processDebugResources` con JDK 17 y Android SDK 35. No pudo terminar en este sandbox ARM64 porque el AAPT2 distribuido por AGP es un binario x86_64 y el entorno no dispone del cargador x86_64 (`x86_64-binfmt-P: Could not open '/lib64/ld-linux-x86-64.so.2'`). Esto es una limitación del entorno local, no un fallo de Java ni de Gradle; la validación definitiva se realiza en `ubuntu-latest` de GitHub Actions.

### Seguridad y permisos

- El manifiesto Android solicita únicamente `android.permission.INTERNET`.
- `android:usesCleartextTraffic="false"` está configurado.
- No se encontraron secretos evidentes en los archivos revisados.
- Las consultas API a `code-scanning/alerts`, `secret-scanning/alerts` y `dependabot/alerts` respondieron `403 Resource not accessible by integration`; por tanto, no es válido declarar esas alertas limpias.
- `main` no está protegida (`Branch not protected`, HTTP 404). Se recomienda activar protección, exigir revisión y exigir el workflow Android verde antes de fusionar.
- No existe una licencia declarada. El README lo indica explícitamente en vez de atribuir una licencia no confirmada.

### Releases y distribución

En la fecha de auditoría `gh release list -R dcg0/DCvsWIZZ` no encontró releases y `v1.0.0` no existe. El README ahora enlaza a la página general de releases y al artefacto de Actions no se le presenta como una descarga permanente. Si se desea una APK pública, debe generarse después de que CI quede verde y publicarse mediante una Release con checksum.

## Puntuación

| Área | Resultado | Puntos |
| --- | --- | ---: |
| CI/CD y reproducibilidad | Corrección aplicada; pendiente confirmar nuevo run verde | 17/25 |
| Dependencias y lockfile | Lockfile creado; sin dependencias npm declaradas y `pnpm audit` limpio | 14/15 |
| Seguridad de aplicación | Permisos mínimos, HTTPS y sin secretos evidentes | 16/20 |
| Documentación y enlaces | Corregida; licencia aún no declarada y sin Release | 12/15 |
| Estructura y calidad | Sintaxis, HTML, build estático y formato correctos | 14/15 |
| Gobierno del repositorio | `main` sin protección y alertas privadas no consultables | 3/10 |
| **Total** |  | **76/100** |

## Veredicto

**REQUIERE REVISIÓN.** El problema reproducible del wrapper quedó corregido y las validaciones locales disponibles pasan, pero la aprobación final depende de confirmar el nuevo `Build APK` en GitHub Actions y de resolver, con permisos de propietario, la protección de `main` y la visibilidad de las alertas de seguridad. No se declara una APK publicada porque no existe una Release en este repositorio.
