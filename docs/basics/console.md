---
layout: default
title: Console
parent: Basics
nav_order: 8
---

# Console
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

When developing applications, you may not always want to rely on a web server like **Apache** or **Nginx**. 
There are situations where code needs to be executed outside of a traditional web request-response cycle. 
To meet these needs, the Puko Framework includes a feature called **Console**. 

### Benefits of Using the Console

*   **Security:** Since the console does not rely on a web server, it does not communicate via HTTP. The process runs directly on the server, making it inherently more secure.
*   **No Timeouts:** Avoid request timeouts imposed by web servers. This is ideal for heavy scripts or long-running processes.
*   **Event-Driven:** Break away from PHP's blocking I/O behavior. With the console, you can enter the reactive world of PHP.
*   **Message Queues:** Easily listen to and consume messages broadcast by message broker servers.
*   **WebSocket Servers:** Build a robust WebSocket server directly using the console.

---

Working with the **Console** is as straightforward as working with **View** or **Service** controllers. The primary difference is in the class extension:

```php
class students extends Console {}
```

You can generate a console route using the following command:

```bash
php puko routes console add <route_name>
```

By default, all console routes are set to accept the `GET` verb. For example:

```bash
php puko routes console add message/forgot/password/subscribe
```

This will result in:
*   **Controller Name:** `console\forgot\password`
*   **Method Name:** `subscribe`

### Running the Console Script

To execute your script from the terminal, CLI, or PowerShell, use the following command:

```bash
php cli message/forgot/password/subscribe <argument>
```

<small>*Note: Currently, the Puko Framework supports passing only one argument.*</small>
