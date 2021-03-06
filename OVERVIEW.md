## Controllers
Responsible for handling incoming requests and returning responses to the client.
A controller's purpose is to receive specific requests for the application.
The routing mechanism controls which controller receives which requests.
Frequently, each controller has more than one route, and different routes can perform different actions.
In order to create a basic controller, we use classes and decorators.
Decorators associate classes with required metadata and enable Nest to create a routing map (tie requests to the corresponding controllers).

## Providers
Many of the basic Nest classes may be treated as a provider – services, repositories, factories, helpers, and so on.
The main idea of a provider is that it can be **injected** as dependency; this means objects can create various relationships with each other,
and the function of "wiring up" instances of objects can largely be delegated to the Nest runtime system.
Controllers should handle HTTP requests and delegate more complex tasks to **providers**.
Providers are plain JavaScript classes that are declared as `providers` in a module.

## Modules
A module is a class annotated with a `@Module()` decorator. The `@Module()` decorator provides metadata that Nest makes use of to organize the application structure.
Each application has at least one module, a root module. The root module is the starting point Nest uses to build the application graph - the internal data structure
Nest uses to resolve module and provider relationships and dependencies. While very small applications may theoretically have just the root module, this is not the typical case. We want to emphasize that modules are strongly recommended as an effective way to organize your components.
Thus, for most applications, the resulting architecture will employ multiple modules, each encapsulating a closely related set of capabilities.

## Middleware
Middleware is a function which is called before the route handler. Middleware functions have access to the `request` and `response` objects,
and the `next()` middleware function in the application’s request-response cycle.
Middleware functions can perform the following tasks:
- execute any code
- make changes to the request and the response objects
- end the request-response cycle
- call the next middleware function in the stack

If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.

## Exception filters
Nest comes with a built-in exceptions layer which is responsible for processing all unhandled exceptions across an application.
When an exception is not handled by your application code, it is caught by this layer, which then automatically sends an appropriate user-friendly response.
Out of the box, this action is performed by a built-in **global exception filter**, which handles exceptions of type `HttpException` (and subclasses of it).
When an exception is **unrecognized** (is neither `HttpException` nor a class that inherits from `HttpException`),
the built-in exception filter generates the following default JSON response:  
```
{ "statusCode": 500, "message": "Internal server error" }
```

## Pipes
A pipe is a class annotated with the `@Injectable()` decorator. Pipes should implement the `PipeTransform` interface.  
Pipes have two typical use cases:
- transformation: transform input data to the desired form (e.g., from string to integer)
- validation: evaluate input data and if valid, simply pass it through unchanged; otherwise, throw an exception when the data is incorrect

## Guards
A guard is a class annotated with the `@Injectable()` decorator. Guards should implement the `CanActivate` interface.
Guards have a single responsibility. They determine whether a given request will be handled by the route handler or not,
depending on certain conditions (like permissions, roles, ACLs, etc.) present at run-time. This is often referred to as authorization.
Guards have access to the `ExecutionContext` instance, and thus know exactly what's going to be executed next.
They're designed, much like exception filters, pipes, and interceptors, to let you interpose processing logic at exactly the right point
in the request/response cycle, and to do so declaratively.

## Interceptors
An interceptor is a class annotated with the `@Injectable()` decorator. Interceptors should implement the `NestInterceptor` interface.
Interceptors have a set of useful capabilities which are inspired by the Aspect Oriented Programming (AOP). They make it possible to:
- bind extra logic before / after method execution
- transform the result returned from a function
- transform the exception thrown from a function
- extend the basic function behavior
- completely override a function depending on specific conditions (e.g., for caching purposes)

## Custom decorators
Nest is built around a language feature called decorators. Decorators are a well-known concept in a lot of commonly used programming languages,
but in the JavaScript world, they're still relatively new. In order to better understand how decorators work,
we recommend reading this [article](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841).
Here's a simple definition:

> An ES2016 decorator is an expression which returns a function and can take a target, name and property descriptor as arguments.
You apply it by prefixing the decorator with an `@` character and placing this at the very top of what you are trying to decorate.
Decorators can be defined for either a class, a method or a property.
