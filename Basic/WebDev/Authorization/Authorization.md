---
title: Authorization
description: 
published: true
date: 2023-05-22T00:40:38.490Z
tags: authorization, 授權
editor: markdown
dateCreated: 2023-05-18T07:27:01.461Z
---

# Authorization
- [ ] [How do we design a permission system?](https://blog.bytebytego.com/p/ep24-environment-friendly-languages?utm_source=profile&utm_medium=reader2)


![5 permission system.png](http://192.168.25.60:8000/files/file_storage/cf40ef35.png)

## ACL (Access Control List)
ACL is a list of rules that specifies which users are granted or denied access to a particular resource.

### Pros - Easy to understand.
### Cons - error-prone, maintenance cost is high

## DAC (Discretionary Access Control)
This is based on ACL. It grants or restricts object access via an access policy determined by an object's owner group.

### Pros - Easy and flexible. Linux file system supports DAC.
### Cons - Scattered permission control, too much power for the object’s owner group.

## MAC (Mandatory Access Control)
Both resource owners and resources have classification labels. Different labels are granted with different permissions.
### Pros - strict and straightforward.
### Cons - not flexible.

## ABAC (Attribute-based access control)
Evaluate permissions based on attributes of the Resource owner, Action, Resource, and Environment.
### Pros - flexible 
### Cons - the rules can be complicated, and the implementation is hard. It is not commonly used.

## RBAC (Role-based Access Control)
Evaluate permissions based on roles
### Pros - flexible in assigning roles.











