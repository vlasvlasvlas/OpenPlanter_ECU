# OpenPlanter en local con Docker (macOS, Windows, Linux)

Este README es solo para instalar y correr `ShinMegamiBoson/OpenPlanter` en local con Docker.

Repo oficial:

`https://github.com/ShinMegamiBoson/OpenPlanter`

## 1) Resultado esperado

Al final vas a poder:

1. Levantar OpenPlanter en modo interactivo.
2. Ejecutar una tarea puntual en modo headless.
3. Apagar, actualizar y volver a levantar sin friccion.

## 2) Requisitos por sistema operativo

### macOS

```bash
brew install --cask docker
git --version || brew install git
docker --version
docker compose version
```

Luego abre Docker Desktop y espera estado `Engine running`.

### Windows (PowerShell)

```powershell
winget install -e --id Docker.DockerDesktop
winget install -e --id Git.Git
docker --version
docker compose version
git --version
```

Usar Docker Desktop con WSL2 habilitado.

### Linux (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install -y docker.io docker-compose-plugin git
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
docker --version
docker compose version
git --version
```

## 3) Instalacion y primer arranque

### Paso 1: clonar

```bash
git clone https://github.com/ShinMegamiBoson/OpenPlanter.git
cd OpenPlanter
```

### Paso 2: crear workspace

```bash
mkdir -p workspace
```

### Paso 3: crear `.env` (obligatorio)

Minimo una API key valida.

macOS/Linux:

```bash
printf "OPENAI_API_KEY=tu_api_key_aqui\n" > .env
```

Windows PowerShell:

```powershell
"OPENAI_API_KEY=tu_api_key_aqui" | Set-Content -Encoding ascii .env
```

Variables adicionales soportadas:

- `ANTHROPIC_API_KEY`
- `OPENROUTER_API_KEY`
- `CEREBRAS_API_KEY`
- `EXA_API_KEY`
- `VOYAGE_API_KEY`

### Paso 4: levantar OpenPlanter

```bash
docker compose up --build
```

Notas:

- Primer arranque: tarda por build de imagen.
- Modo interactivo: dejarlo en primer plano.
- Salida: `Ctrl+C`.

## 4) Prueba de funcionamiento

Cuando aparezca el prompt del agente, enviar un objetivo simple:

```text
Analyze files in /workspace and summarize key risks
```

Cerrar desde el agente con:

```text
/quit
```

## 5) Modo headless (1 tarea y salir)

```bash
docker compose run --rm agent --task "Cross-reference vendor payments against lobbying disclosures and flag overlaps" --workspace /workspace
```

## 6) Operacion diaria

Apagar:

```bash
docker compose down
```

Actualizar y levantar:

```bash
git pull
docker compose up --build
```

Rebuild completo:

```bash
docker compose build --no-cache
docker compose up
```

## 7) Errores comunes

`env file .env not found`:

- Crear `.env` en la raiz del repo (`OpenPlanter/.env`).

`No API keys are configured`:

- Cargar una API key real en `.env`.

`permission denied /var/run/docker.sock` (Linux):

- Volver a ejecutar `sudo usermod -aG docker $USER` y reabrir sesion.

El contenedor se cierra rapido:

- Ejecutar `docker compose up` en primer plano (sin `-d`).

## 8) Resumen copy/paste

macOS/Linux:

```bash
git clone https://github.com/ShinMegamiBoson/OpenPlanter.git
cd OpenPlanter
mkdir -p workspace
printf "OPENAI_API_KEY=tu_api_key_aqui\n" > .env
docker compose up --build
```

Windows PowerShell:

```powershell
git clone https://github.com/ShinMegamiBoson/OpenPlanter.git
cd OpenPlanter
mkdir workspace -Force
"OPENAI_API_KEY=tu_api_key_aqui" | Set-Content -Encoding ascii .env
docker compose up --build
```
