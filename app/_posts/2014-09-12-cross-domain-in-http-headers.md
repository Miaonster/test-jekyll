---
layout: post
title: "Cross Domain in http headers"
description: "Access-Control-Allow-Origin and Content-Security-Policy"
category: 
tags: [cross domain, http header, Access-Control-Allow-Origin, Content-Security-Policy]
---
{% include JB/setup %}

In morden browsers, we have more methods to deal with **cross domain** problems. Here're 2 methods to play with http headers to set the way how **corss domain** works.

### Access-Control-Allow-Origin

With this http header, we can set which origin can access our resource. For example, if we want to restrict access to the resource to be only from `http://foo.com`, we should use:

	Access-Control-Allow-Origin: http://foo.com
	
### Content-Security-Policy

This header defined which location or which type of resources are allowed to be loaded. For example, if we want to allow a cross domain request to `http://bar.com`, we should use:

	Content-Security-Policy: bar.com