+++
title = "Fastify Adoption Guide"
date = "2024-08-06"
author = "Ukeje Goodness"
description = "In this adoption guide, we’ll discuss some reasons to choose Fastify, key features, compare Fastify to some popular alternatives, and more"
[taxonomies]
tags = ["JavaScript", "Web development", "Framework", "Technical", "APIs"]
+++

---

_**[LogRocket](logrocket.com) made this piece possible. They provide AI-first session replay and analytics that shows
you what's wrong.**_

# Fastify Adoption Guide: Overview, Examples, and Alternatives

As your frontend applications receive increasing demand, they require fast and responsive backends to deliver the
optimal user experiences. You’ll need a framework that plays nicely with the frontend tooling and frameworks you’re
building with.

There are many backend frameworks to choose from across the various server-side languages. If you’re looking to use
JavaScript, especially on your app’s frontend, you can consider using Fastify.

In this adoption guide, we’ll discuss some of the reasons you should (or shouldn’t) choose Fastify, key features you
should know, and more. We’ll also compare Fastify to some popular alternatives to equip you to make an informed decision
for your project.

## What is Fastify?

[Fastify](https://fastify.dev/) is a performant, open source Node.js framework for building efficient and fast web
applications. Its modular architecture is based on a rich, easy-to-integrate plugin system.

![](https://paper-attachments.dropboxusercontent.com/s_6AE55FF9FC0A98BCF75595E793430DD1FE445567DC6E29681C604770A8067B17_1719335648997_Untitled+10.png)

Fastify also plays nicely with popular modern frontend frameworks and libraries like React, Vue, and Angular. It
provides great DX via schema-based validation, detailed documentation, and full TypeScript support, making it a popular
choice for developers in the JavaScript ecosystem who need to spin up fast, reliable backend services.

The project contributors built Fastify to create a framework with high throughput and low latency that was secure with a
minimal footprint. Since its inception, Fastify has undergone several iterations that added new features and improved
its performance to stay updated with the trend in server-side app development.

Fastify leverages plugins to extend its core functionality. You build your application by registering plugins that
provide specific functionalities. These plugins can define routes, handle server decorations, or add custom logic.

<h4 style="color: #bebebe;"><em>Further reading:</em></h4>

- [Fastify-vite: Serving Vite apps with SSR and client-side hydration](https://blog.logrocket.com/fastify-vite-serving-vite-ssr-hydration/)

## Why you should choose Fastify

As usual, there are multiple factors you should consider when choosing a server-side framework for your application.
Here's an overview of the factors that make Fastify a great tool to use:

- **Performance** — Fastify has a reputation for high performance, and it can handle thousands of requests per second.
  It’s also efficient when it comes to server resources. You should consider Fastify if you’re looking to lower
  infrastructure costs and improve responsiveness under load
- **Security —** Fastify supports JSON schemas for validating routes and serializing outputs for data integrity. It also
  uses Pino, a low-overhead logger, to minimize logging costs
- **Ease of use/DX** — Fastify is quite expressive in contrast to other server-side frameworks in the market. It’s
  designed to be expressive for devs without compromising performance and security. Also, Fastify has an intuitive API
  design that’s easy to grasp, so you can spend more time on what matters: building!
- **Bundle size** — Fastify’s focus on minimalism and efficiency suggests a leaner package compared to other server-side
  frameworks
- **Community & ecosystem** — Thanks to Fastify’s large community of developers, it has a super-rich plugin system that
  covers multiple functionalities from caching to authentication and database integration. You can tailor these plugins
  to build apps that match your project’s specifications
- **Learning curve** — Just like JavaScript itself, Fastify is easy to learn and adopt. Its intuitive design also makes
  it easy for developers from another ecosystem to easily adopt it.
- **Documentation** — Fastify is well documented on [Fastify.dev](http://Fastify.dev) and GitHub. An active community of
  developers uses, supports, and maintains the Fastify codebase and documentation
- **Integrations/plugins** — Through its rich plugin ecosystem, Fastify provides many integrations and support for
  multiple platforms and technologies, including Platformatic, tRPC, Clerk, WebSockets, Prisma, and more

Of course, Fastify isn’t perfect. Depending on your use case, you may find other frameworks better suited for your
project. Check out the comparison table between Fastify and other popular frameworks below to learn more.

## Getting started with Fastify

Here's a quickstart guide to get you started with Fastify, including installing, building a route, and creating a
server.

First, ensure you have Node.js installed. You can install Fastify using npm or Yarn:

```javascript

    # NPM
    npm init -y
    npm install fastify
    
    # YARN
    yarn init -y
    yarn add fastify
```

To create a server with Fastify, create a new file named `server.js` and add this import for the Fastify library:

```javascript

    // Import the Fastify library
    const fastify = require('fastify')({ logger: true });
```

Next, define a simple route at the root directory to which you can send requests:

```javascript

    fastify.get('/', async (request, reply) => {
      return { hello: 'world' };
    });
```

The route returns a JSON with `{"hello":"world"}` when you visit the root URL with the server running locally on
port `3000`.

Here’s a function definition that starts the server on port `3000` with error handling to catch any errors while running
the server:

```javascript

    // Run the server
    const start = async () => {
      try {
        await fastify.listen(3000);
        fastify.log.info(`Server listening on ${fastify.server.address().port}`);
      } catch (err) {
        fastify.log.error(err);
        process.exit(1);
      }
    };
```

Finally, call the `start` function at the bottom of the file:

```javascript

    start();
```

If you’re using Node like me, you can run the server with this command:

```javascript

    node server.js
```

Request the server or open the endpoint on your browser. Here's the output you should see:

![](https://paper-attachments.dropboxusercontent.com/s_6AE55FF9FC0A98BCF75595E793430DD1FE445567DC6E29681C604770A8067B17_1719335747497_Untitled+11.png)

You can add more routes by defining additional handlers. Here's an example POST route:

```javascript

    // Define a POST route
    fastify.post('/data', async (request, reply) => {
      const { name } = request.body;
      return { hello: name };
    });
```

For the POST route, you’re using the `post` function. The POST route returns the body of the request with `request.body`
function.

## Key Fastify features to know

Fastify is a supercharged backend framework that can probably take whatever you throw.
Let’s go over some important operations you’d explore with Fastify if you adopt it as your app's backend framework.

<H3>Fastify plugins

Plugins are how you get to extend Fastify’s functionality. There are many plugins out there that you can try out for
what you’re building. Explore the [Ecosystem page](https://fastify.dev/ecosystem/) and check out the 296+ plugins
available to use. If you can’t find a plugin, [writing one](https://fastify.dev/docs/latest/Reference/Plugins/) for your
use case is very easy.

Here’s how easy and intuitive it is to use Fastify plugins:

```javascript

    const fastify = require('fastify')();
    const cors_plugin = require('@fastify/cors')
    
    // Example: Registering a CORS plugin
    
    fastify.register(cors_plugin, {
        origin: '*'
    });
```

The `@fastify/cors` plugin helps you use CORS easily in your Fastify app. You’ll have to register plugins
with `fastify.register` to use them.

<H3>Auth

You can achieve various forms of authentication and authorization with Fastify. Here’s an example use of a JWT plugin
for authentication with Fastify:

```javascript

    const fastify = require('fastify')();
    fastify.register(require('@fastify/jwt'), {
        secret: 'supersecret'
    });
    
    // JWT authentication hook
    fastify.addHook('onRequest', async (request, reply) => {
        try {
            await request.jwtVerify();
        } catch (err) {
            reply.send(err);
        }
    });
    
    fastify.post('/login', async (request, reply) => {
        const token = fastify.jwt.sign({ user: 'example' });
        return { token };
    });
    
    fastify.listen({ port: 3000, host: '0.0.0.0' });
```

After registering the plugin, the program adds a JWT authentication hook to verify JWTs. On visiting the `/login` route,
the endpoint should be authenticated.

<H3>Validation

You can validate incoming data from requests with a schema where you’d specify required values. Here’s how:

```javascript

    const fastify = require('fastify')();
    
    fastify.post('/data', {
        schema: {
            body: {
                type: 'object',
                properties: {
                    name: { type: 'string' },
                    age: { type: 'integer' }
                },
                required: ['name', 'age']
            }
        }
    }, async (request, reply) => {
        return { status: 'Data is valid' };
    });
    
    fastify.listen({ port: 3000, host: '0.0.0.0' });
```

The program ensures that POST requests to `/data` have a JSON object with two properties: a `name` of type `string` and
an `age` of type `integer`. On receiving valid data, the server responds with a success message.

<H3>Serialization

Serialization is an operation you’d be doing a lot if you’re using Fastify to build APIs. Here’s how you can serialize
data with Fastify:

```javascript

    const fastify = require('fastify')();
    
    fastify.get('/user', {
        schema: {
            response: {
                200: {
                    type: 'object',
                    properties: {
                        id: { type: 'integer' },
                        name: { type: 'string' }
                    }
                }
            }
        }
    }, async (request, reply) => {
        return { id: 1, name: 'John Doe' };
    });
    
    fastify.listen({ port: 3000, host: '0.0.0.0' });}
```

The program defines a route that responds to GET requests on the `/user` endpoint. When clients make GET requests to the
endpoint, the server validates the response and ensures the response has the expected structure with the `id` (integer)
and `name` (string) properties.

<H3>Logging

Fastify also has built in functions for logging in different levels:

```javascript

    const fastify = require('fastify')({ logger: true });
    
    fastify.get('/', async (request, reply) => {
        request.log.info('Hello world route called');
        return { hello: 'world' };
    });
    
    fastify.listen({ port: 3000, host: '0.0.0.0' });
```

The program defines a simple web server that responds to GET requests on the root path with the message `hello world`
after logging the message `Hello world route called` to the specified output.

<H3>Adapter

Adapters make it easy to use Fastify to work with different server environments:

```javascript

    const fastify = require('fastify')();
    const awsLambdaFastify = require('aws-lambda-fastify');
    
    fastify.get('/', async (request, reply) => {
        return { hello: 'world' };
    });
    
    const proxy = awsLambdaFastify(fastify);
    exports.handler = proxy;
    
```

The example program above acts as an adapter for a Fastify app that runs within AWS Lambda. The `aws-lambda-fastify`
bridges wrap the Fastify app to make it compatible with the Lambda execution model.

<H3>Error handling

You can handle request-related errors with Fastify and error handlers. Here’s how:

```javascript

const fastify = require('fastify')();

fastify.setErrorHandler((error, request, reply) => {
        reply.status(500).send({ error: 'Something went wrong!' });
    });
    
    fastify.get('/', async (request, reply) => {
        throw new Error('Oops!');
    });
    
    fastify.listen({ port: 3000, host: '0.0.0.0' });
```

Here, we’re created custom error handlers for the Fastify app. All uncaught errors, like the ones from the GET requests
to the root path, trigger the handler.

The handler sends a generic error message with a 500 status code to the client, but in a real application, you'd likely
provide more specific error responses.

## Use cases for Fastify

You can use Fastify for most of the use cases you would use a backend framework for, from building web backends to APIs
and real-time apps. Let’s dive into some of Fastify’s popular use cases for you to gain insights into what you can use
Fastify for:

1. **Building high-performance web APIs and microservices** — Fastify's low overhead and optimized request/response
   handling are great for building efficient, scalable APIs and microservices that handle high amounts of requests
2. **Developing real-time applications using WebSockets** — You can build chat apps, games, and collaborative tools with
   Fastify since there’s built-in support for WebSockets
3. **Building server-side rendered web apps** — You can use Fastify to create server-side rendered web applications and
   deliver content to your audience fast
4. **Handling high traffic loads efficiently** — Fastify handles loads nicely and is really resource-efficient. It’s
   great for building concurrent apps if you expect high backend traffic
5. **Business/practical use cases —** Fastify‘s speed and low latency make it a great choice for building
   high-performance API, microservices, and real-time apps. It can also handle data-intensive applications, efficiently
   processing data from IoT devices and ensuring fast response times for high-traffic ecommerce platforms. Fastify makes
   it easy to build prototypes and focus on business logic in development. You can also use it to serve static content
   and build serverless functions

You can check out the Fastify website and forums to learn how the community is using Fastify to power various use cases.
There are also multiple popular organizations already using Fastify across multiple projects for different use cases.
You can check out [this page](https://fastify.dev/organisations/) for a comprehensive overview of organizations using
Fastify.

<h4 style="color: #bebebe;"><em>Further reading:</em></h4>

- [How to build a blazingly fast API with Fastify](https://blog.logrocket.com/how-to-build-a-blazingly-fast-api-with-fastify/)
- [Using WebSockets with Fastify](https://blog.logrocket.com/using-websockets-with-fastify/)

## Deploying your Fastify Project

Using various methods, you can deploy your Fastify projects locally or to cloud providers. Deployment options for your
Fastify project include:

- **Cloud Platforms (PaaS)** — You can deploy your Fastify apps on Heroku, AWS, Google Cloud and many other platforms.
  These platforms offer easy deployment with little to no configurations
- **Containerization:** You can use containerization tools like Docker and Kubernetes to containerize your app for
  consistency across environments and cloud providers
- **Virtual private servers (VPS)** — VPS providers offer more control over the server environment. You'll need to
  manage the server infrastructure yourself, but you’ll gain more flexibility
- **Serverless functions (AWS Lambda, Google Cloud Functions)** — You can use serverless functions for event-driven
  microservices architectures and pay-per-execution, making it cost-effective for applications with variable traffic

There are also some best practices you should follow for deploying your Fastify project, such as:

- Use environment variables to store sensitive configuration data like API keys and database credentials to keep them
  away from third parties and unauthorized access
- Use process managers to keep your apps running smoothly so they can handle restarts, logs and scaling without breaking
- Set up monitoring and logging tools to track your app’s health and performance metrics. Implement proper logging to
  easily diagnose issues
- Follow best security practices like input validation and sanitization, user authentication and authorization to
  protect your account from vunerabilities
- Use version control systems like Git actively to maintain a deployment history that’s easy to roll back to
- Depending on the complexity of your application, you can explore using CICD pipelines to automate testing and
  deployment processes for consistency

Following these practices while using these deployment options sets your project up for success.

## Fastify vs. Express.js vs. Koa vs. Hapi

Fastify isn’t the only popular backend framework in the JS/TS ecosystem. There are more, like Express, Hapi, and Koa.
Here’s a comparison table that overviews how Fastify compares to these other frameworks:

| Aspect        | Fastify                                                                                         | Express.js                                               | Koa                                         | Hapi                                                             |
|---------------|-------------------------------------------------------------------------------------------------|----------------------------------------------------------|---------------------------------------------|------------------------------------------------------------------|
| Features      | Built-in plugins, schema validation, automatic JSON parsing, HTTP/2 support, TypeScript support | Middleware system (large ecosystem), routing, templating | Lighter, more modular middleware system     | Extensive built-in features (routing, validation, auth), plugins |
| Performance   | Fastest (fewer callbacks, less boilerplate)                                                     | Good (may require optimization, callback-heavy)          | Competitive (requires optimization)         | Good (mature framework, optimized code)                          |
| Community     | Growing, active development                                                                     | Large, established                                       | Moderate, growing                           | Established, active                                              |
| Resource/Docs | Excellent documentation, active GitHub, tutorials                                               | Extensive documentation, community support, tutorials    | Good documentation, active community        | Comprehensive documentation: plugins often have separate docs    |
| Philosophy    | Performant, modern, built-in features                                                           | Flexible, large ecosystem                                | Lightweight, modular, control over requests | Enterprise-grade, extensive features, plugins                    |

It’s safe to conclude that Fastify is ideal for high-performance APIs, whereas Express.js has a greater community with
more middleware, and Koa is an alternative to Express for small projects that need a custom setup, and Hapi is great for
enterprise-grade applications.

Now, you can make a choice for your project based on this table and bring your project to life.

<h4 style="color: #bebebe;"><em>Further reading:</em></h4>

- [Forget Express.js — opt for these alternatives instead](https://blog.logrocket.com/node-js-alternative-frameworks-express-js/)
- [NestJS vs. Express.js](https://blog.logrocket.com/nestjs-vs-express-js/)
- [Comparing top Node.js frameworks for frontend developers #Fastify](https://blog.logrocket.com/comparing-top-node-js-frameworks-frontend-developers/#fastify)

## Conclusion

In this guide, you’ve learned about Fastify, and hopefully, you’ve had your doubts cleared or have reasons to adopt
Fastify for your projects.

You always have the Fastify community of developers if you get stuck. Also, there are many Fastify resources on the
LogRocket blog that you can use to build your projects. Explore and enjoy!

