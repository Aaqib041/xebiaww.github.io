---
layout: post
header-img: img/default-blog-pic.jpg
author: swatijain85
description: 
post_id: 17799
created: 2013/12/19 11:28:42
created_gmt: 2013/12/19 06:28:42
comment_status: open
---

# Validation of Required Query Parameters in REST Resource (exposed via Jersey JAX-RS provider)

Recently, I had exposed my application resources on web via JAX-RS Jersey provider - so that it can be consumed as a Restful Web service. Thereafter, I got a new request from client which is stated below:

"_**Query parameters that are required to invoke any operation on rest resource shall be validated for its existence and expected pattern value in request (in JSON format) before resource method is actually invoked**_"

i.e. my web service (resource) methods shall return results only when they are properly invoked with all the mandatory parameters and that too when their value matches the expected pattern value.

To implement the above request, initially I thought that if any required query parameter is not supplied in request, then its value shall be NULL and my resource class (or method - to be more specific) should deal with it accordingly. But as I implemented above thought in the code, I realized that above statement is not completely true as the value for missing query parameters is not always NULL, it is an empty collection for List or Set and default Java value for primitive types (as specified in JAX-RS 1.0 specification).

Adding @DefaultValue annotation and setting the parameter to an appropriate value whenever it's missing was also an option but that was also ruled out because it is always not possible to define the default value. Also, I had to write code to validate the passed in value for each query parameter in my resource method so that it matched the expected pattern value. Implementing such kind of validation at each resource method would be cumbersome and hard to manage as it may lead to boiler plate code problem.

After much thought process and brain storming session, I decided to implement above validation in a generic way. Firstly, I created an name-binding annotation as mentioned below:

[sourcecode] @NameBinding @Retention(RetentionPolicy.RUNTIME) public @interface ValidateQueryParams { public abstract String[] value(); } [/sourcecode]

@ValidateQueryParams annotation's value is the array of query parameters that need be validated for its existence in request along with expected value pattern (as defined by standard regular expression). The name of query parameter and its expected value pattern are separated by ':'

Above annotation is used on resource method in following manner:

[sourcecode] @POST @Consumes(MediaType.APPLICATION_JSON) @Produces(MediaType.APPLICATION_JSON) @ValidateQueryParams({"id:[0-9]+", "version:[0-9]+"}) public Response getEntity(@Context UriInfo ui, @Context HttpHeaders hh, String data); [/sourcecode]

Above definition means that before invoking my resource method "getEntity", I need to check for presence of following query parameters in request:

  * Parameter 'id' which shall contain only numeric value as specified by regex pattern [0 -9]+
  * Parameter 'version' which shall also contain only numeric value as specified by regex pattern [0 -9]+

If any of the above query parameters are not present in my request or its value doesn’t match the specified regex pattern, the application shall throw BAD_REQUEST exception.

Thereafter, I created a new filter that was bound to this annotation in following manner:

[sourcecode] /__ _ Filter to perform request parameters validation on REST resource _ _ _/ @Provider @ValidateQueryParams("") public class ValidationFilter implements ContainerRequestFilter { @Override public void filter(ContainerRequestContext requestContext) { try { validateQueryParams(requestContext); } catch (Exception e) { throw new WebApplicationException(Status.BAD_REQUEST); } }

... ... [/sourcecode]

All the code for validating the query parameters present in request is now implemented in above filter. For reader reference, query parameters map can be retrieved from ContainerRequestContext object in following manner (inside request filter):

[sourcecode] MultivaluedMap<String, String> requestQueryParamMap = requestContext.getUriInfo().getQueryParameters(); [/sourcecode]

Now, there is a common place where I need to write the code for validating the query parameters - where I check for their existence and if the supplied value matches the expected pattern value or not. If the validation fails, then I just throw BAD_REQUEST exception. I've attached the sample code for creating such filter with this blog [ValidationFilter.java][1]. Since, this validation filter is called before any resource method is invoked, so it served my use case to perfection. Finally, all is well now :).

Happy Reading!

   [1]: http://xebee.xebia.in/wp-content/uploads/2013/12/ValidationFilter.java_.txt