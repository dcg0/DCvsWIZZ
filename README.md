🔗https://dcg0.github.io/DCvsWIZZ/
# DC vs WiZZ

[![Build APK](https://github.com/dcg0/DCvsWIZZ/actions/workflows/android.yml/badge.svg)](https://github.com/dcg0/DCvsWIZZ/actions/workflows/android.yml)
[![Latest APK](https://img.shields.io/github/v/release/dcg0/DCvsWIZZ?label=APK&logo=android)](https://github.com/dcg0/DCvsWIZZ/releases/latest)
[![APK Download](https://img.shields.io/badge/download-APK-34a853?logo=android&logoColor=white)](https://github.com/dcg0/DCvsWIZZ/releases)
[![Website](https://img.shields.io/badge/website-arcade-13a7ff)](https://dcg0.github.io/DCvsWIZZ/)
[![Security](https://img.shields.io/badge/security-SECURITY.md-2ea44f)](SECURITY.md)
[![License](https://img.shields.io/badge/license-not%20declared-lightgrey)](https://github.com/dcg0/DCvsWIZZ)

![Portada DC vs WiZZ](public/assets/portada.jpg)

**DC vs WiZZ** es un arcade fighter remasterizado

## Enlaces principales

- **[Visitar la página](https://dcg0.github.io/DCvsWIZZ/)**
- **[Descargar la APK](https://github.com/dcg0/DCvsWIZZ/releases)**
- **[Ver las releases](https://github.com/dcg0/DCvsWIZZ/releases)**
- **[Ver el repositorio](https://github.com/dcg0/DCvsWIZZ)**

La APK está firmada para instalación de prueba en Android

## Contenido

- Landing arcade

## Desarrollo web

```bash
pnpm install
pnpm run dev
```

La página se sirve en `http://localhost:3000`.

## Compilar la APK

Requiere Java 17, Android SDK Platform 35 y Build Tools 35.0.0.

```bash
cd android
./gradlew assembleDebug
```

El resultado se genera en:

```text
android/app/build/outputs/apk/debug/app-debug.apk
```

El workflow de GitHub Actions compila la APK en cada push a `main` y permite descargarla como artefacto desde la pestaña **Actions**.

## Seguridad

Consulta la [política de seguridad](SECURITY.md) dependencias y acciones de GitHub se revisan mediante Dependabot.

## Estructura

- `public/`: página, estilos, juego y recursos.
- `android/`: aplicación Android nativa que carga los recursos locales del juego.
- `.github/workflows/android.yml`: compilación automática de la APK.
- `.github/dependabot.yml`: actualizaciones automáticas de Gradle y GitHub Actions.
- `SECURITY.md`: proceso de reporte y prácticas de seguridad.
