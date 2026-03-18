---
layout: default
title: Project Structure
nav_order: 4
---

# Project Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview

```text
|-- assets
|   |-- html              # View templates organized by language (en/id)
|   |-- master            # Master layouts and localized string JSONs
|   |-- system            # Default system templates (404, 500, etc.)
|-- bootstrap             # Infrastructure configuration (Docker/Nginx)
|-- config                # Application configuration files
|-- controller            # Primary web controllers (View/Service)
|-- model                 # Custom database model contracts
|-- plugins               # Framework-managed plugins (Auth, Elements, Generated Models)
|-- tests                 # Unit and integration tests
|-- .env.example          # Template for environment variables
|-- cli                   # Entry point for console-based controllers
|-- Dockerfile            # Containerization configuration
|-- index.php             # Web entry point
|-- puko                  # Puko CLI development tool
```

---

## Puko

The `puko` file located at the root of your project is the primary **development CLI**. It serves as a powerful scaffolding tool that automates repetitive tasks.

Key capabilities of the `puko` tool include:
*   **Scaffolding:** Creating new controllers, routes, and database models.
*   **Database Setup:** Wiring database tables to PHP Data Objects.
*   **Development Server:** Starting a built-in server for local testing.
*   **Authentication:** Generating boilerplate classes for custom auth logic.

---

## Cli

The `cli` file is the entry point for executing your Puko controllers directly from the command line, bypassing the web server. This is essential for:
*   **Background Tasks:** Running long-lived scripts or cron jobs.
*   **Message Consumers:** Listening to message brokers like RabbitMQ or Kafka.
*   **WebSocket Servers:** Running real-time communication servers.

Usage: `php cli <route_path> [argument]`

---

## Environment variables

Puko utilizes a `.env` file for environment-specific configurations. This file should never be committed to your version control system as it often contains sensitive data.

Standard variables include:
*   **DB_HOST/DB_USER/DB_PASS:** Database connection credentials.
*   **ENVIRONMENT:** Sets the application mode (e.g., `DEVELOPMENT` or `PROD`).
*   **ENCRYPTION_KEY:** The master key used for securing session and bearer data.

---

## Generated content

To maintain high performance and developer productivity, Puko automatically manages several directories and files:

*   **plugins/model/**: Contains the **Data Object Wiring** classes. These are read-only classes generated directly from your database schema.
*   **config/routes.php**: A central registry of all application routes. While it can be viewed, it is best managed via the `php puko routes` commands.
*   **assets/html/en/id**: Default template directories for your View controllers, generated automatically when a new view route is added.

---

## Architecture Pattern

Puko follows a strict **Hierarchical Model-View-Controller (HMVC)** pattern. This structure ensures that your application remains modular and easy to navigate as it scales.

*   **Controllers** are located in the `controller/` directory.
*   **Views** (HTML) are located in `assets/html/`.
*   **Models** are split between generated wiring (`plugins/model/`) and custom business logic (`model/`).
