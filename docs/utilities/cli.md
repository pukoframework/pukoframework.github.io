---
layout: default
title: Puko CLI
parent: Utilities
nav_order: 1
---

# Puko CLI
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

The Puko Framework is bundled with a Command Line Interface (CLI) named **Puko Console**. This tool serves as a development helper to streamline your workflow. You can view all available commands by running:

```bash
php puko help
```

### Available Commands

```text
setup    Installation & Configuration
         [db]
         [secure]
         [auth] [name]
         [controller] [view/service] [name]
         [model] [add/update/remove] [name] [schema]
         
routes   Routing Management
         [view/service/console/list/error/lost] [add/update/delete/crud] [url]

generate Automatic Schema Generation
         [db]

serve    Start the development server on localhost
         [port]
         
tests    Run unit tests (Preview)

element  Generate or download view elements (Beta)
         <name> [add/download]
         
cli      Execute code directly from the console
         <router path>
         
help     Show the help menu

version  Show the console version
```

### Command Breakdown

#### Setup

*   **setup db** & **setup secure**: Please refer to the [Configuration]({{ site.baseurl }}{% link docs/configuration.md %}) page for details.
*   **setup auth**: Please refer to the [Authentication]({{ site.baseurl }}{% link docs/basics/authentication.md %}) page.

#### Routes

*   **routes view**: Refer to the [View]({{ site.baseurl }}{% link docs/basics/view.md %}) documentation.
*   **routes service**: Refer to the [Service]({{ site.baseurl }}{% link docs/basics/service.md %}) documentation.

#### Generate

The `php puko generate db` command creates database tables based on the model schemas defined in `plugins\model`.

#### Serve

You can run your Puko project without a standalone web server using the built-in development server:

```bash
php puko serve [PORT_NUMBER]
```

#### Tests

The `php puko tests` command is used to run all unit tests in your project. It automatically scans the `tests/unit/` directory and executes test cases to verify your controller and model logic.

#### Element

The `php puko element` command allows you to manage modular view components. You can create new elements locally or download them from the official Puko elements repository.

Example usage:
```bash
php puko element download adminlte_description
php puko element add user_profile
```

For more details, see the [Elements]({{ site.baseurl }}{% link docs/views/elements.md %}) section.

#### CLI

The `php puko cli [ROUTE_URL]` command allows you to execute controllers directly from the command line, which is useful for background tasks and automation.

#### Help

Displays the full list of available commands and their usage.

#### Version

Displays the current version of the Puko Console: `php puko version`.
