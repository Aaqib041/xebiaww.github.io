---
layout: post
header-img: img/default-blog-pic.jpg
author: saket_vishal
description: 
post_id: 7790
created: 2011/02/10 09:37:34
created_gmt: 2011/02/10 04:37:34
comment_status: open
---

# What makes me love Spring Web MVC 3.0

<p>There is a plethora of presentation frameworks, Spring Web MVC is just one of them, but with the recent versions, the changes introduced make it a mark above the rest. Following are few positives that caught my eye.</p>
<p><em>Annotation based programming model for MVC Contollers</em>
With the introduction of @Controller, @RequestMapping ... creating controllers is just so easy and clean. Give any flexible names to method names and parameters. No dependency on the Servlet API, but access to Servlet API is always available.
<!--more-->
<em>Less code and Programming ease</em>
Whether you talk about simple cases such as using request parameters or session attribute, and accessing Request headers, or complicated cases such multipart(fileupload) support, and multiformat response, all are made easy using annotations.</p>
<p><em>Painfree exception handling</em>
HandlerExceptionResolver provides powerful programmatic exception handling mechanism, while the SimpleMappingExceptionResolver provides the same exception handling facility as provided by the Servlet API. Not to forget is the @ExceptionHandler, put it inside the contoller on a standard method, and do the needful, clean and simple.</p>
<p><em>Enhanced look and feel with theming</em>
Define the theme characterstics in a properties file or any other source, and then go ahead and use these properties through spring:theme in your jsps. ThemeResolvers makes it easy to figure out which theme to be used.</p>
<p><em>Simple Restification</em>
A combination of URI Templates and @PathVariable method parameter annotation is what is all enough for adding REST nature to the application.</p>
<p><em>Speedy prototyping with covention over configuration</em>
Stick to some naming conventions, and what you get is freedom from most of the configuration of Spring Web MVC. This can be really very helpful in case of rapid prototyping.</p>
<p>All these above mentioned attributes make a compelling case for use of Spring Web MVC as a preferred presentation framework. Though there can be many arguments against this choice depending upon the respective usage, still it remains my first choice.</p>