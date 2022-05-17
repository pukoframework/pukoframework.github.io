---
layout: default
title: Controller
parent: Basics
nav_order: 2
---

# Search
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

All controller in puko mostly auto-generated by `pukoconsole` following HMVC (Hierarchical Model View Controller) patterns.
So, we can only focus on logic part of our controller.
This section of document will describe deeper what is 'controller' 
because in puko until now controller separated into 3 functionality `view` `service` and `console`.

Theoretically web application life-cycles is divided into requests and responds.
Also, all request by default stored by PHP in `$_POST` or `php://input` and responds back with **echo**.
So controller in puko is doing this request and returning responds from native PHP into another experience level in very easy way.

For responds that needs web page as output, your controller should extends the **View**. 
Otherwise maybe you at the moment only building a backend REST API or GraphQL endpoints 
that responds with data with JSON format,
your controller should extends the **Service**.
But, if you developing another feature without web server 
and executing PHP scripts directly from operating system terminal, 
your controller should extends the **Console**. 

Lets digging that one-by-one:

**View**

View is the first original feature of puko framework. Its utilize and extends **PTE** extensions as HTML render engine.
Because puko adopting HMVC pattern, you always have a HTML file with same directory structure as the controller 
and wired automatically in the _return_ statement in the end of controller function.
This is why `pukoconsole` is always bundling generated html file each view type of routes spawned.

> TODOC

**Service**

If main use case is API or backend applications, you can use `service` controller in puko framework.
By default, it will convert returned data at the end of function into JSON representational response data.
Also, you need `ext-json` enabled in PHP modules to utilize this functionality.

> TODOC

**Console**

Oftentimes we need a PHP script to doing something in background.
you can use `console` controller in puko fraamework. 
Because it can execute commands separately from web server. 
So you don't need to worry about server limitation parameter like timeouts.

> TODOC

> You can also manually create the controller :)

---

* Controller namespace in *Puko framework* is PSR-4 compatible.

```php
namespace controller;
```

```php
namespace controller\inventory;
```

* Controller in *Puko* must extends one of the following: **View** / **Service** / **Console**.

```php
class member extends View {
``` 

```php
class member extends Service {
```

```php
class member extends Console {
```

* Exposing response in controller is done by *return* keyword from *function*.

```php
public function member() {
    $data['Name'] = 'Didit Velliz';
    $data['Address'] = 'Bandung';

    return $data;
}
```

* Controller in *Puko* have additional feature named *docs tag*.

```php
/**
 * #Value Hobby Swimming
 * #Auth bearer true
 */
public function member() {
    $data['Name'] = 'Didit Velliz';
    $data['Address'] = 'Bandung';

    return $data;
}
```

> Notes: you can see *docs tag* further in **Puko Docs Command** sections.