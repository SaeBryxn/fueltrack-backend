# FuelTrack — API

API REST para la logística de pedidos y despacho de combustible. Los clientes registran pedidos; el proveedor los aprueba, les asigna camión, conductor y compartimentos, y consulta métricas.

**Stack:** .NET 8 · ASP.NET Core · Entity Framework Core 8 · PostgreSQL · Swagger · Docker

## Arquitectura

El código está organizado por capas y por contextos del dominio (DDD):

```
FuelTrack.Backend/
├── Domain/           Entidades, value objects, eventos y validadores
│   ├── Orders/       Order, OrderProduct, OrderStatus, Payment, OrderApprovedEvent
│   ├── Dispatch/     Truck, Tank, Driver, CompartmentAssignment, DispatchValidator
│   ├── Pricing/      FuelPrice
│   ├── Terminals/    Terminal, TerminalStatus
│   └── Analytics/    Servicios de métricas para cliente y operador
├── Application/      Casos de uso y DTOs (Orders, Pricing, Analytics)
├── Infrastructure/   DbContext y repositorios con EF Core
└── Interfaces/       Controladores REST agrupados por rol (Client / Provider)
```

## Endpoints principales

| Método | Ruta | Descripción |
|--------|------|-------------|
| `POST` | `/api/orders` | Crear un pedido |
| `GET` | `/api/orders`, `/api/orders/{id}`, `/api/orders/all` | Listar pedidos y ver su detalle |
| `PUT` | `/api/orders/{id}/approve` | Aprobar un pedido |
| `PUT` | `/api/orders/{id}/assign` | Asignar camión, conductor y compartimentos |
| `PUT` | `/api/orders/{id}/release` | Liberar el despacho |
| `DELETE` | `/api/orders/{id}` | Eliminar un pedido |
| `GET` | `/api/dispatch/lookup` | Camiones, tanques y conductores disponibles |
| `GET` · `POST` · `PUT` | `/api/pricing`, `/api/pricing/create`, `/api/pricing/update` | Precios de combustible |
| `GET` | `/api/analytics/summary` | Resumen de pedidos y consumo |

La documentación completa está en Swagger (`/swagger`) al ejecutar la API.

## Cómo ejecutarlo

Requisitos: .NET 8 SDK y PostgreSQL.

```bash
# Cadena de conexión por variable de entorno (no la guardes en el repositorio)
export ConnectionStrings__DefaultConnection="Host=localhost;Port=5432;Database=fueltrack;Username=postgres;Password=<tu-contraseña>"

dotnet run --project FuelTrack.Backend
```

Las migraciones se aplican al arrancar. También incluye un `Dockerfile` para desplegarla en un contenedor.

## Repositorios relacionados

- [fueltrack-frontend](https://github.com/SaeBryxn/fueltrack-frontend) — aplicación web en Vue 3
- [fueltrack-landing](https://github.com/SaeBryxn/fueltrack-landing) — landing page del producto

---

Desarrollado por [Bryan Espejo](https://bryanespejo.pages.dev) · Ingeniería de Software, UPC
