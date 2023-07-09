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


Sometimes, when coding, we don't always rely on web servers like **Apache** or **Nginx**. 
There may be a need to execute code outside the web server world. 
To meet these requirements, Puko comes with a feature called **Console**. 
So, what are the benefits of using it?

* Security:

Since the console doesn't rely on a web server, you no longer communicate via the HTTP protocol. 
Your process runs inside the server and is secure by default.

* No timeout

Say goodbye to request timeouts raised by web servers. Now you can run very heavy scripts that can run all day long without any issues.

* Event-driven

Move away from PHP's blocking I/O behavior. With the **console**, you can enter the reactive world of PHP.

* Message Queue Consumer

It is now possible to listen to messages broadcasted by message-broker servers.

* WebSocket Server

You can write a WebSocket server using the **console**.

And many other possibilities that default PHP cannot provide.

---

Working with the **console** is as easy as working with **view** or **service** controllers. The noticeable difference can be found in the **extends** section of the controllers:

```php
class students extends Console {}
```

You can also generate it with the Puko console using the command `php puko routes console add <route_name>`. 
However, all console routes will be set by default to accept the GET verb. 
For example:

```text
php puko routes console add message/forgot/password/subscribe

controller name: console\forgot\password
method: subscribe
```

You can place code in the console controller just like in regular controllers.

To run your script from the CLI/PowerShell/Terminal, use the following command:

```
php cli message/forgot/password/subscribe <arguments>
```

<small>For now, the Puko framework allows only 1 argument.</small>
