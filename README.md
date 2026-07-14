# 🤖 Stocker: Gestión de Stock y Reabastecimiento Automatizado (RPA)

[![RPA](https://img.shields.io/badge/RPA-Platform-blue.svg)](https://learn.microsoft.com/es-es/power-automate/)
[![n8n](https://img.shields.io/badge/Orchestrator-n8n-orange.svg)](https://n8n.io/)
[![Database](https://img.shields.io/badge/Database-Supabase-green.svg)](https://supabase.com/)

**Stocker** es un sistema híbrido de automatización robótica de procesos (RPA) que gestiona de extremo a extremo el ciclo de inventario y compras de material de oficina y tecnológico. 

A través de una arquitectura integrada por **n8n**, **Supabase**, **Power Automate Desktop & Cloud**, y un flujo interactivo **Human-in-the-Loop** vía Microsoft Teams/Telegram, el sistema monitoriza el inventario en tiempo real, compara precios de múltiples proveedores de forma algorítmica y ejecuta la compra de manera autónoma previa aprobación de los responsables.

---

## 🗺️ Arquitectura de la Solución

El proyecto está diseñado bajo un modelo híbrido que combina la agilidad de la nube para la captura de eventos y bases de datos con la potencia de ejecución local para automatizaciones de interfaz de usuario (UI Scraping y Web Checkout).

```mermaid
graph TD
    A[Interfaz UI: Retirada de Material] -->|Confirma Acción| B(Orquestador: n8n local con Docker)
    B -->|Actualiza Stock y Verifica Mínimos| C[(Capa de Datos: Supabase)]
    B -->|POST JSON de Productos en Alerta| D[Dispatcher: Power Automate Cloud]
    D -->|Genera Cola de Trabajo| E[Work Queues de PAC]
    E -->|Monitorea ítems pendientes| F[Performer: Power Automate Desktop]
    F -->|Scraping en modo incógnito| G[Proveedores: Amazon, MediaMarkt, PCComponentes]
    F -->|Algoritmo Precio Mínimo| H[Reporte de Compra: Excel]
    H -->|Tarjeta de Aprobación| I[Responsable de Compras: Microsoft Teams/Telegram]
    I -->|APROBAR| J[Robot de Compra: PAD Checkout Automatizado]
    I -->|RECHAZAR| K[Fin del Proceso: Gestión Manual]
    J -->|Carga de Recibo y Actualiza Stock| C
