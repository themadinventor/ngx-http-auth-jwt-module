# Intro
This is an NGINX module to check for a valid JWT and proxy to an upstream server or redirect to a login page.

# Build Requirements
This module depends on the [JWT C Library](https://github.com/benmcollins/libjwt)

Transitively, that library depends on a JSON Parser called [Jansson](https://github.com/akheron/jansson) as well as the OpenSSL library.

# NGINX Directives
This module requires several new nginx.conf directives, which can be specified in on the `main` `server` or `location` level.

```
auth_jwt_key "00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF";
auth_jwt_loginurl "https://yourdomain.com/loginpage";
auth_jwt_enabled on;
```

So, a typical use would be to specify the key and loginurl on the main level and then only turn on the locations that you want to secure (not the login page).  Unauthorized requests are given 302 "Moved Temporarily" responses with a location of the specified loginurl.

```
auth_jwt_redirect            off;
```
If you prefer to return 401 Unauthorized, you may turn `auth_jwt_redirect` off.

```
auth_jwt_validation_type AUTHORIZATION;
auth_jwt_validation_type COOKIE=rampartjwt;
```
By default the authorization header is used to provide a JWT for validation.  However, you may use the `auth_jwt_validation_type` configuration to specify the name of a cookie that provides the JWT.

The Dockerfile builds all of the dependencies as well as the module, downloads a binary version of nginx, and runs the module as a dynamic module.

Have a look at build.sh, which creates the docker image and container and executes some test requests to illustrate that some pages are secured by the module and requre a valid JWT.
