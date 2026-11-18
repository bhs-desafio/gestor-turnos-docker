# gestor-turnos-docker

Levanta la solución completa de **Gestor de Turnos** (SQL Server + API .NET 9 + frontend Angular) con Docker Compose.

## Cómo ejecutar

Desde la carpeta `gestor-turnos-docker`:

```bash
docker compose up --build -d
```

Luego abrir el front-end con http://localhost:4200

## Carga de turnos

En este repositorio se incluye la plantilla [`Turnos.xlsx`](Turnos.xlsx), que puede usarse para cargar turnos desde el front-end (opción de carga de turnos por Excel).

## Requisitos

- Docker Desktop (Compose v2)
- Los tres repositorios clonados como carpetas hermanas:

```
<carpeta>/
├── gestor-turnos-backend/
├── gestor-turnos-frontend/
└── gestor-turnos-docker/     <- este repo
```

```bash
git clone https://github.com/bhs-desafio/gestor-turnos-backend.git
git clone https://github.com/bhs-desafio/gestor-turnos-frontend.git
git clone https://github.com/bhs-desafio/gestor-turnos-docker.git
```

## Ejecución

```bash
cd gestor-turnos-docker
cp .env.example .env        # ajustar SA_PASSWORD si se desea
docker compose up --build -d
```

| Servicio | URL |
|---|---|
| Frontend | http://localhost:4200 |
| API (Scalar) | http://localhost:5032/scalar |
| SQL Server | `localhost,14333` (usuario `sa`) |

La base de datos `GestorTurnosDb` se crea automáticamente al iniciar la API.

## Arquitectura

```
navegador ──► web (nginx :80)
                ├── /        → Angular (estáticos)
                └── /api/*   → api:8080 (.NET 9)
                                   └──► sqlserver:1433 (volumen sqldata)
```

## Comandos útiles

```bash
docker compose logs -f api      # logs de la API
docker compose down             # detener
docker compose down -v          # detener y borrar la base de datos
docker compose up --build api   # reconstruir solo la API
```
