---
layout: post
title: Linked server alias
date: '2010-12-27 12:41:18'
tags:
- sql
---


Add a linked server using a different name **or** add a reference to the local server using a different name (handy if importing databases which used to be on different servers, but temporarily on the same server).

> sp_addlinkedserver 'Alias', '', 'SQLOLEDB', 'ServerName'


