# Tracker QR — Gestión y rastreo de pedidos (Spring Boot + React)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17+-orange)](#)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F)](#)
[![React](https://img.shields.io/badge/React-18.x-61DAFB)](#)
[![Node](https://img.shields.io/badge/Node-18+-339933)](#)
[![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248)](#)
[![Prettier](https://img.shields.io/badge/code%20style-prettier-ff69b4)](https://prettier.io/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

Sistema web para que tus clientes consulten el estado de sus pedidos escaneando un código QR único, mientras el personal logístico registra movimientos y la administración genera reportes de trazabilidad y métricas mensuales. 

---

## ✨ Objetivo y alcance

* **Propósito:** mejorar la trazabilidad de paquetes y dar a clientes información en “tiempo casi real” de ubicación/estado, con registro operado por logística y validación por supervisión. 
* **Alcance principal:**

  * Cliente: consultar estado, fecha/hora y *historial* del paquete vía QR.
  * Empleado de logística: registrar etapas (recolección, tránsito, entrega) y actualizaciones de ubicación.
  * Supervisor/Administrador: generar reportes (PDF, programables) y visualizar entregas por empleado (mensual). 

---

## 🧑‍🤝‍🧑 Roles del sistema

* **Cliente:** consulta estado y movimientos del paquete.
* **Empleado de logística:** crea eventos (etapas, ubicación; con timestamp del servidor).
* **Administrador/Supervisor:** gestiona usuarios/roles, reportes y visualizaciones. 

---

## 🧩 Funcionalidades clave

**Módulo Cliente**

* Generación de **QR único** por paquete.
* Visualización de **estado actual**, **fecha/hora** del último movimiento (ISO-8601) y **historial** ordenado (más reciente primero).
* **Confirmación de recepción** (firma digital/escaneo/aceptación en app) para marcar “Entregado”. 

**Módulo Logística**

* Registro de **etapa actual** y **ubicación** (evento inmutable con timestamp del servidor).
* **Validación de usuarios** (autenticación/autorización por rol; respuestas 401/403 en intentos no autorizados). 

**Módulo Administrador**

* **Reportes de trazabilidad** manuales y programados (**PDF**: diario/semanal/mensual).
* **Gráficas mensuales** de entregas por empleado (filtro por persona o totales).
* **Gestión de usuarios** (alta/edición/activar-desactivar, roles y contraseñas cifradas). 

**Inicio de sesión**

* Autenticación por **correo + contraseña**, bloqueo tras 3 intentos fallidos, redirección por rol. 

---

## 📐 Requisitos no funcionales (RNF)

* **Disponibilidad:** ≥ 90% anual con monitoreo y planes de respaldo.
* **Seguridad:** **2FA** obligatorio (además de la contraseña).
* **Escalabilidad:** ≥ **1,000** consultas simultáneas (validadas con pruebas de carga).
* **Compatibilidad:** navegadores modernos + móviles (diseño responsivo).
* **Usabilidad:** interfaz clara con iconos/colores de estado y mensajes de confirmación/error.
* **Rendimiento:** mostrar datos del paquete en ~**7 s** tras escanear el QR. 

---

## 🏗️ Arquitectura y stack

* **Arquitectura:** orientada a servicios (SOA).
* **Backend:** Java **Spring Boot** (APIs, seguridad, generación/lectura QR, reportes PDF).
* **Frontend:** **React** (portal cliente y panel interno).
* **Base de datos:** **MongoDB** / Firebase (según despliegue).
* **Clientes:** Web y dispositivos móviles (responsive). 

---

## 🔌 API (borrador propuesto, alineado al DFR)

**Clientes**

* `GET /api/shipments/{qr}/status` → estado actual + timestamp último movimiento.
* `GET /api/shipments/{qr}/timeline` → historial (estado, ubicación, fecha/hora; reciente→antiguo).
* `POST /api/shipments/{id}/confirm` → confirma recepción (marca “Entregado” si procede).

**Logística**

* `POST /api/shipments/{id}/events` → registra etapa/ubicación (timestamp del servidor; inmutable).
* `GET /api/shipments/{id}` → detalle con estado actual + últimos eventos.

**Administración**

* `POST /api/reports/tracking?period=monthly` → genera **PDF** de trazabilidad.
* `GET /api/metrics/deliveries?month=YYYY-MM&employeeId=...` → entregas mensuales (gráfica).
* `POST /api/users` / `PATCH /api/users/{id}` / `PATCH /api/users/{id}/toggle` → gestión de usuarios/roles.

**Auth**

* `POST /api/auth/login` → JWT; **2FA** requerido según política.
* Respuestas 401/403 estandarizadas para accesos no autorizados. 

---

## 🧭 Flujo principal

1. **Generar QR** al crear el paquete → imprime/adjunta al envío.
2. **Cliente** escanea QR → ve estado actual, fecha/hora, historial.
3. **Logística** registra movimientos (recolección → tránsito → entrega).
4. **Cliente** confirma **recepción** (firma/aceptación) → estado pasa a **Entregado**.
5. **Administración** genera reportes **PDF** y consulta métricas mensuales. 

---

## 👥 Contacto y control de cambios

* Cliente: **Nelida Baron Perez** (Representante: Andrea Aquino Flores).
* Documento base: DFR “Tracker QR”, versiones y cambios del 18–19 Sep 2025. 

---
