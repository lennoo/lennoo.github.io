---
title: Django调整
date: 2012-5-25 23:03:08
---
Django == 1.10

1. 配置

2. Tune your AUTH_PASSWORD_VALIDATORS setting by removing the django.contrib.auth.password_validation.CommonPasswordValidator out there.

Password validation is a new feature introduced in Django 1.9.

```
This password is too short. It must contain at least 8 characters.
This password is entirely numeric.
```
