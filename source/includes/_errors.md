# Errors

The Zebpay API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- This response means that server could not understand the request due to invalid syntax
401 | Unauthorized -- Although the HTTP standard specifies "unauthorised", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response
404 | Not Found -- The server can not find requested resource.In an API, this can also mean that the endpoint is valid but the resource itself does not exist.
500 | Internal Server Error -- The server has encountered a situation it doesn't know how to handle
503 | Service Unavailable -- This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid 
