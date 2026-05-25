# Shared GitHub Actions Workflows

Este repositorio contiene workflows reutilizables para todos los repositorios de paquetes NPM de Grupo-ProSolid.

## 📋 Workflows disponibles

### 1. `npm-publish.yml`
Publica paquetes NPM a GitHub Package Registry.

**Triggers:**
- `workflow_dispatch` (manual)
- `push` a `main` cuando cambian `package.json`

**Inputs:**
- `node-version`: Versión de Node (default: `20`)
- `package-paths`: Array JSON de rutas de paquetes

**Secrets requerido:**
- `npm-token`: Token de autenticación

**Ejemplo de uso:**
```yaml
jobs:
  publish:
    uses: Grupo-ProSolid/.github/.github/workflows/npm-publish.yml@main
    with:
      node-version: '20'
      package-paths: '["packages/my-package"]'
    secrets:
      npm-token: ${{ secrets.GITHUB_TOKEN }}
```

### 2. `npm-ci.yml`
Ejecuta tests y builds para validar cambios.

**Triggers:**
- `push` a `main`
- `pull_request` a `main`

**Inputs:**
- `node-version`: Versión de Node (default: `20`)

**Ejemplo de uso:**
```yaml
jobs:
  ci:
    uses: Grupo-ProSolid/.github/.github/workflows/npm-ci.yml@main
    with:
      node-version: '20'
```

## 🚀 Cómo usar en tus repositorios

### En tu repositorio (ej: `ardis-io`)

Crea `.github/workflows/publish.yml`:
```yaml
name: Publish Packages

on:
  workflow_dispatch:
  push:
    branches:
      - main
    paths:
      - 'packages/*/package.json'

jobs:
  publish:
    uses: Grupo-ProSolid/.github/.github/workflows/npm-publish.yml@main
    with:
      node-version: '20'
      package-paths: '["packages/ardis-io-js", "packages/ardis-io-cli"]'
    secrets:
      npm-token: ${{ secrets.GITHUB_TOKEN }}
```

Crea `.github/workflows/ci.yml`:
```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    uses: Grupo-ProSolid/.github/.github/workflows/npm-ci.yml@main
    with:
      node-version: '20'
```

## 📝 Notas importantes

- Los workflows usan `GITHUB_TOKEN` por defecto
- `continue-on-error: true` permite que el workflow continúe si una publicación falla (ej: versión sin cambios)
- Todos los paquetes se publican como `--access public`

## 🔄 Versioning

Usa `@main` para usar la última versión, o especifica un tag/commit si prefieres estabilidad.

## 📚 Referencias
- [GitHub Actions - Reusable Workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [GitHub Package Registry - npm](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)
