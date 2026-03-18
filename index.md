---
layout: default
title: Home
nav_order: 1
description: "Puko Framework, fullstack PHP framework for Rapid Application Development."
permalink: /
---

# Puko Framework
{: .fs-9 }

Welcome to the Puko Framework, a full-stack PHP framework designed for Rapid Application Development.
{: .fs-6 .fw-300 }

[![Latest Stable Version](https://poser.pugx.org/puko/framework/v/stable)](https://packagist.org/packages/puko/framework)
[![Total Downloads](https://poser.pugx.org/puko/framework/downloads)](https://packagist.org/packages/puko/framework)

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/Velliz/puko){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Getting started

### Dependencies

Like other modern PHP frameworks, the Puko Framework requires **PHP 7.0 or newer**. Standard extensions include `Composer`, `php-json`, `php-pdo`, `php-pdo-mysql`, and `php-gd`. You may also require additional extensions depending on third-party libraries installed via Composer. You can run this framework using tools like [Laragon](https://laragon.org/download/index.html) or [XAMPP](https://www.apachefriends.org/download.html).

### Quick start

1. **Install via Composer:**
```bash
composer create-project velliz/puko <project-name>
```
<small>*Note: You must have PHP and Composer installed and accessible via your terminal. If the command is not recognized, ensure they are correctly set in your PATH or Environment Variables.*</small>

2. **Run with the built-in PHP web server:**
```bash
php puko serve 4000
```

3. **Open your browser:**
Navigate to [http://localhost:4000](http://localhost:4000).

<small>Puko can also be deployed using Apache, Nginx, or Docker.</small>

If you plan to create a Docker Image, you can review the provided `Dockerfile` and build it using `docker build .`.

<small>Follow our [~15-minute quick start guide]({{ site.baseurl }}{% link docs/quick-start.md %}) to build your first web application.</small>

### Configure Environment

To set up a universal configuration for your project, rename the `.env.example` file to `.env` and adjust the settings to match your development environment.

---

## About the project

Puko Framework is &copy; 2016-{{ "now" | date: "%Y" }} by [Didit Velliz](https://velliz.github.io).

### License

The Puko Framework is distributed under the [MIT license](https://github.com/pukoframework/pukoframework.github.io/tree/master/LICENSE.txt).

### Contributing

Before making a contribution, please discuss the changes you wish to make via an issue, email, or any other preferred method with the maintainers. Learn more about becoming a contributor in [our GitHub repository](https://github.com/pukoframework/pukoframework.github.io#contributing).

#### Thank you to the contributors of Puko Framework!

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>

### Code of Conduct

The Puko Framework is committed to fostering a welcoming and inclusive community.

[View our Code of Conduct](https://github.com/pukoframework/pukoframework.github.io/tree/master/CODE_OF_CONDUCT.md) on GitHub.
