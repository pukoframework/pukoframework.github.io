---
layout: default
title: Home
nav_order: 1
description: "Puko Framewok, fullstack PHP framework for Rapid Application Development."
permalink: /
---

# Puko Framework
{: .fs-9 }

Welcome to Puko Framewok, fullstack PHP framework for Rapid Application Development.
{: .fs-6 .fw-300 }

[![Build Status](https://travis-ci.org/Velliz/pukoframework.svg?branch=master)](https://travis-ci.org/Velliz/pukoframework)
[![Total Downloads](https://poser.pugx.org/puko/framework/downloads)](https://packagist.org/packages/puko/framework)
[![Latest Stable Version](https://poser.pugx.org/puko/framework/v/stable)](https://packagist.org/packages/puko/framework)

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/Velliz/puko){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Getting started

### Dependencies

Like another PHP framework out there, the standard PHP extension required for running Puko Framework is Composer, `php-json`, `php-pdo`, `php-pdo-mysql`, `php-gd` and PHP version required was `7.0` or newer. You may also required another extension from additionally thrid party library installed via composer. You can also running this framework with bundled tools like [Laragon](https://laragon.org/download/index.html) or [XAMPP](https://www.apachefriends.org/download.html).

### Quick start

1. Install via composer
```bash
composer create-project velliz/puko <project-name>
```
<small>You must have PHP and composer installed and accessible via command prompt/terminal. If command not is not recognized you may need to setup the PATH or Environment Variables.</small>

2. Run with default PHP web-server
```bash
php puko serve 4000
```

3. Point your web browser to [http://localhost:4000](http://localhost:4000)

<small>You can also run the code from Apache/Nginx/Docker.</small>

If you're planning to make a Docker Container Image, you can review the default `Dockerfile` configuration and build the image with `docker build .` 

### Configure Environment

If you want to setup a universal configuration for your project, you can rename `.env.example` file to `.env` file and make the configuration adjusment. so that you can more easily work in your development environment.

---

## About the project

Puko Framework is &copy; 2016-{{ "now" | date: "%Y" }} by [Didit Velliz](https://velliz.github.io).

### License

Puko Framework is distributed by an [MIT license](https://github.com/pukoframework/pukoframework.github.io/tree/master/LICENSE.txt).

### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/pukoframework/pukoframework.github.io#contributing).

#### Thank you to the contributors of Puko Framework!

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>

### Code of Conduct

Puko Framework is committed to fostering a welcoming community.

[View our Code of Conduct](https://github.com/pukoframework/pukoframework.github.io/tree/master/CODE_OF_CONDUCT.md) on our GitHub repository.
