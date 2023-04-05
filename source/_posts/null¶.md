---
title: 官方文档
tag: django
categories: 2023
---


https://stackoverflow.com/questions/19428572/django-templatesyntaxerror-could-not-parse-the-remainder

<!-- more -->

模型字段参考

字段选项

https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#validators

以下参数对所以字段类型均有效，且是可选的。



### `null`[¶](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#null)

- `Field.``null`[¶](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.Field.null)

  

如果是 `True`， Django 将在数据库中存储空值为 `NULL`。默认为 `False`。

避免在基于字符串的字段上使用 [`null`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.Field.null)，如 [`CharField`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.CharField) 和 [`TextField`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.TextField)。如果一个基于字符串的字段有 `null=True`，这意味着它有两种可能的“无数据”值。`NULL`，和空字符串。在大多数情况下，“无数据”有两种可能的值是多余的，Django 的惯例是使用空字符串，而不是 `NULL`。一个例外是当一个 [`CharField`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.CharField) 同时设置了 `unique=True` 和 `blank=True`。在这种情况下，`null=True` 是需要的，以避免在保存具有空白值的多个对象时违反唯一约束。

无论是基于字符串的字段还是非字符串的字段，如果希望在表单中允许空值，还需要设置 `blank=True`，因为 [`null`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.Field.null) 参数只影响数据库的存储（参见 [`blank`](https://docs.djangoproject.com/zh-hans/4.1/ref/models/fields/#django.db.models.Field.blank) ）。