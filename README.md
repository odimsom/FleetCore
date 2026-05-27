---
name: project-fleetcore-core
description: FleetCore — plataforma EMM B2B para flotas Android empresariales en LATAM. Arquitectura, stack, repo y estructura de carpetas.
metadata:
  type: project
---

FleetCore es una plataforma EMM (Enterprise Mobility Management) de Synset Solutions, orientada al mercado B2B latinoamericano para gestión de flotas Android en operaciones de campo (talleres automotrices, logística, retail, servicios técnicos).

**Why:** Compite en el vacío que Citrix/Jamf/ManageEngine dejan: empresas medianas LATAM con flotas de 10-100 dispositivos, sin dependencia de GMS ni Google Play.

**Repo GitHub:** https://github.com/odimsom/FleetCore (privado)

## Arquitectura — 4 capas

1. **agent/** — Android DPC APK. Kotlin, AOSP puro, DevicePolicyManager. Sin GMS. Módulos: kiosk launcher, hardening, heartbeat, policy sync, event recorder (SQLite), sync engine.
2. **backend/** — .NET 9, Clean Architecture, MediatR/CQRS, EF Core. Módulos: enrollment gateway, push channel (WebSocket + Redis backplane), event ingestion, policy service, license guard (JWT RS256, expiración 12 meses). Persistencia: PostgreSQL + TimescaleDB + Redis.
3. **dashboard/** — Angular 19, WebSocket tiempo real, JWT, roles por empresa/departamento. Módulos: fleet manager, policy editor, observabilidad básica.
4. **infra/** — Docker, CI/CD.

## Hardware homologado v1.0
- Samsung Galaxy A / Tab A (Android 10-14)
- Lenovo Tab M / Tab P (Android 10-13)
- Zebra TC / EC series (Android Enterprise Recommended)

## Roadmap
- Fase 1 MVP (meses 1-4): enrollment QR/código, kiosk mode, whitelist, heartbeat, policy sync, remote lock, dashboard básico
- Fase 2 (meses 5-9): GPS tracker, geofencing, silent install, remote wipe, observabilidad avanzada
- Fase 3 Enterprise (meses 10+): on-premise Docker con licencia JWT, LDAP/AD, auditoría forense

## Modelo de negocio (USD)
- SaaS por dispositivo: $3-5/device/mes
- Planes fijos: Pyme $79 (20 dev), Flota Media $179 (50 dev), Corporativo $320 (100 dev), Enterprise custom
- On-premise: setup fee $1500-3000 + iguala mensual $300-500

## Estructura de archivos en repo
```
FleetCore/
├── README.md, LICENSE, CHANGELOG.md, CONTRIBUTING.md, SECURITY.md
├── .gitignore, .gitattributes
├── docker-compose.yml, docker-compose.override.yml
├── .github/ISSUE_TEMPLATE/, .github/workflows/ (ci-agent, ci-backend, ci-dashboard)
├── agent/app/src/main/AndroidManifest.xml
├── backend/FleetCore.sln + src/{Api,Application,Domain,Infrastructure}/ + tests/
├── dashboard/
├── infra/docker/
└── docs/
```

**How to apply:** Usar esta estructura como referencia para cualquier trabajo de código, CI/CD, o decisiones de arquitectura en el proyecto FleetCore.
