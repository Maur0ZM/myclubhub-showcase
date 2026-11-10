# MyClubHub

**Software de gestión para clubes y ligas deportivas.** Está en producción en [myclubhub.cl](https://myclubhub.cl) con clubes y ligas de Chile que lo usan de verdad.

Lo diseñé, programé y opero yo solo: frontend, backend, base de datos, despliegue y soporte.

> El código fuente es privado porque es un producto comercial con datos de clientes. Este repositorio explica qué hace y cómo está construido.

---

## Qué problema resuelve

Muchos clubes y ligas amateur llevan pagos, asistencia, fixture y estadísticas en planillas Excel y grupos de WhatsApp. MyClubHub junta todo eso en un solo sistema web con un portal para cada rol: administradores, entrenadores, apoderados y representantes de equipo.

## Funcionalidades principales

### Clubes
- **Mensualidades y cobros:** generación mensual, pagos en línea (Flow y Mercado Pago), carga y revisión de comprobantes de transferencia, morosidad y conciliación bancaria.
- **Asistencia:** registro por entrenamiento, con QR y modo kiosco.
- **Deportistas y apoderados:** fichas, documentos, carnet digital y formularios personalizados.
- **Portales** para entrenadores y apoderados (PWA instalable en el celular).

### Ligas
- **Fixture:** generación de fechas, reglas de días y partidos, reprogramación y partidos suspendidos.
- **Tabla de posiciones y playoffs** con generador de llaves.
- **Partido en vivo:** marcador público en tiempo real alimentado desde la mesa de control.
- **Estadísticas** por jugador y equipo.
- **Portal de representantes** para gestionar la nómina y los pagos de cada equipo.

### MyClubHub Mesa
Aplicación de escritorio en Python para la mesa de control: registra el partido en vivo (puntos, faltas, tiempos y cambios), muestra un tablero para el público y sincroniza con la web. Se distribuye con instalador y sistema de licencias.

## Stack

| Capa | Tecnologías |
|---|---|
| Frontend | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4, Recharts |
| Backend | Python, FastAPI, SQLAlchemy 2, Pydantic 2, Alembic, APScheduler |
| Base de datos | PostgreSQL |
| Pagos | Flow y Mercado Pago (cuentas conectadas por cada club) |
| Correo | Resend, con cola y límites de envío |
| IA | API de Claude para moderar las imágenes que suben los usuarios |
| Infraestructura | Docker, Vercel (frontend), Render (API), Supabase (PostgreSQL y almacenamiento) |
| Respaldos | Raspberry Pi 5 propia con respaldo nocturno de la base |

## Arquitectura

```
 Navegador / PWA ──► Next.js (Vercel) ──► API FastAPI (Render) ──► PostgreSQL (Supabase)
                                              │
 MyClubHub Mesa (escritorio) ─────────────────┤──► Flow / Mercado Pago (pagos)
                                              ├──► Resend (correos)
                                              └──► Raspberry Pi (respaldo nocturno)
```

## Seguridad y buenas prácticas
- Autenticación con JWT y permisos por rol; segundo factor (TOTP) en el panel de administración de la plataforma.
- Límites de peticiones (rate limiting) en los endpoints públicos.
- Migraciones versionadas con Alembic.
- Dependencias actualizadas y revisión de seguridad antes de cada despliegue.

## Autor

**Mauricio Zúñiga Medina** · Ingeniero Civil en Computación (UTEM)
[myclubhub.cl](https://myclubhub.cl) · mauro.zm02@gmail.com
