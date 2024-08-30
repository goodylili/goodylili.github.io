+++
title = "How to Use Import Attributes in JavaScript and TypeScript"
date = "2024-01-03"
author = "Ukeje Goodness"
description = "Import attributes are gaining traction in JavaScript and TypeScript. Let’s explore how they improve app efficiency, stability, and security."
[taxonomies]
tags = ["JavaScript", "TypeScript", "Releases", "Technical", "Programming", "Tutorial"]
+++
---

_**[LogRocket](logrocket.com) made this piece possible. They provide AI-first session replay and analytics that shows
you what's wrong.**_





# How to use Import Attributes in TypeScript and JavaScript
`import` is a fundamental keyword in JavaScript and TypeScript that enables you to include code from external modules, promoting code organization and reusability. Import attributes are gaining traction in both JavaScript and TypeScript, marking a new era in developing sophisticated, high-quality web apps.

In this guide, we’ll discuss how to leverage import attributes in TypeScript and JavaScript. We’ll explore how import attributes work, why they matter, their impact on efficiency, stability, and security in web apps, and more.

## A brief overview of import attributes

Import attributes are the latest feature addition to JavaScript’s continually evolving module system. They enable you to include additional metadata for specifying import actions and behaviors.

As the complexity and diversity of web applications increase, managing and loading conditions become crucial for scalability, making import attributes more important now than ever.  JavaScript and TypeScript’s module system fell short at handling these requirements for web applications.

TypeScript v5.3 builds on its JavaScript foundation by adding import attributes with the usual type safety and tooling benefits inherent to the language. You can follow [the TypeScript proposal for import attributes on GitHub](https://github.com/tc39/proposal-import-attributes).

Incorporating import attributes in TypeScript and JavaScript projects creates new possibilities for developing efficient, secure, and manageable apps by catering to the sophisticated needs of modern web architectures.

## Using import attributes in TypeScript

You’ll need to install TypeScript v5.3 to use import attributes. Run this command on your terminal to install TypeScript v5.3 globally:


    npm install -g typescript@5.3

Next, run these commands to create and initialize TypeScript configurations in your project with the name `ts-import-attributes` and enter the directory:


    mkdir ts-import-attributes
    cd ts-import-attributes
    tsc --init

Then, initialize a new Node.js project and install TypeScript in your project as a dependency:


    npm init -y
    npm install typescript --save-dev

Now, you can initialize your TypeScript project by executing this command:


    npx tsc --init

The command above creates a `tsconfig.json` file in your project. Edit the file to match the following `compilerOptions` setup:


    // tsconfig.json
    
    {
      "compilerOptions": {
        "moduleResolution": "node",
        "target": "es6",
        "module": "esnext",
        "esModuleInterop": true,
        "forceConsistentCasingInFileNames": true,
        "strict": true,
        "skipLibCheck": true,
        "outDir": "./dist",
        "resolveJsonModule": true,
      }
    }
    

Next, create a `config.json` file in your project’s directory and add this JSON to the file:


    // config.json
    
    {
      "appName": "TS Import Demo",
      "version": 1.0
    }

Finally, add this to your `package.json` file:


    "type": "module",

Here’s how you can use import attributes to import the `config` from the `config.json` file:


    // index.ts
    import config from './config.json' assert { type: 'json' };
    
    console.log("The unique feature from config.json is:", config.uniqueFeature);

In this case, the `import` statement ensures that the `config.json` file is treated as a JSON file.

Note that we’re using the `assert` keyword here, and not the `with` keyword. Import attributes evolved from import assertions, with the main difference being the keyword.

Another difference is that runtimes can use attributes to guide the resolution of import paths. In comparison, assertions only assert characteristics after loading modules.

As an additional note, Node.js doesn’t currently support import attributes in TypeScript. That’s why we’re using the `assert` keyword in this case. You can learn more about this in the [TypeScript 5.3 release documentation.](https://arc.net/l/eakqxewq)

Now, execute this command to compile the program:


    npx tsc

The command should transpile your TypeScript program into JavaScript code that looks like this:


    import config from './config.json' with { type: 'json' };
    console.log("The unique feature from config.json is:", config.uniqueFeature);

Notice that the code is transpiled into the `with`  keyword in JavaScript and the behaviours are the same.

Since we’re working with the Node.js runtime, execute the compiled JavaScript code with the following `node` command:


    node dist/index.js

Here’s the output you should expect after running the program:


![](https://paper-attachments.dropboxusercontent.com/s_675D480C677C9D2D5AB48D507E2D66DF7CC79EAB7B497B472D3EDFE5F610CDDD_1703784765834_Screenshot+2023-12-28+at+18.30.40.jpg)

## How import attributes work under the hood

Now that we’ve seen a simple example of import attributes in a TypeScript app, let’s briefly review how they work from a technical standpoint.

After specifying the import, the TypeScript compiler handles import statements by preserving the attributes for runtime with minimal checks. TypeScript doesn’t process or validate attributes, but it performs basic checks — for example, ensuring you use the `with` keyword correctly and that attributes are valid expressions.

The JavaScript runtime or bundler interprets the import attributes depending on the specific features and behaviors they support. Webpack and Rollup already recognize attributes like `type:` `"``json``"`. If the import attribute’s format doesn’t match the expectations of the runtime or bundler, it may throw an error.

As of the time of this writing, many browsers lack support for import attributes. However, transpilers like Babel can handle them during compilation and convert them to runtime checks that browsers understand.

## Custom import attribute configurations

You can [customize the behavior of import attributes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-3.html) to configure their usage. Specifically, import attributes have a `resolution-mode` attribute you can use to specify how modules are resolved. The attribute allows you to specify whether modules are resolved traditionally using the `require` call or as an ES6 import.

Here’s an example of an import configured to be resolved with the `require` keyword:


    import type { SomeType } from "some-module" with { "resolution-mode": "require" };

Here’s one that should be resolved as an ES6 import:


    import type { SomeType } from "some-module" with { "resolution-mode": "import" };

The `resolution-mode` attribute is supported in all module modes and also works with all other `moduleResolution` options, such as `bundler` or `node10`. 

Currently, the `resolution-mode` attribute is the only stable way you can customize import attributes, although more customization options may be added in the future. You may also be able to explore other ways to extend or customize features in your projects.

## Benefits of import attributes for security and stability

Web apps can benefit from some stability and security feature gains when using import attributes in TypeScript.

The security improvements that import attributes offer emphasize intentional imports, sub-resource integrity, and compliance with CSPs: 


- Since import attributes enforce structured and intentional imports, they help mitigate the risks associated with malicious imports, especially where dependencies are nested or complex
- Ensuring imported modules function as expected helps reduce supply chain attacks
- TypeScript’s explicit import attributes align with CSPs (Content Security Policies) by controlling and limiting content-loading sources
- Integrity checks within import attributes — e.g., Subresource Integrity (SRI) — ensure that externally outsourced content is free from any third-party interference
- With import attributes, TypeScript simplifies codebase auditing by allowing developers to specify specific imports. This clarity helps address potential harm or necessary imports and streamline security reviews

Meanwhile, the stability benefits of import attributes emphasize better module management and ergonomics with clarity during development: 


- Providing strict typing and specific import types helps TypeScript minimize the risk of runtime errors from mismatched module formats and ensure reliable, consistent behavior at runtime
- Reducing uncertainty and debug time associated with dynamic modules enhances the overall development experience
- Specifying exact versions of modules facilitates control over module versions, which is crucial for maintaining compatibility and avoiding issues from module version mismatches
- Streamlining update processes allows efficient dependency management in large projects by making update processes predictable and controllable
- Enhancing the clarity of import attributes in TypeScript enables straightforward updates and modifications with reduced risk of unintended consequences during code refactoring

In summary, TypeScript’s import attributes feature significantly improves security and stability. They offer a more secure, controlled, and predictable environment for developing and maintaining robust web applications.

## Drawbacks and common pitfalls of using `import` attributes

As useful as import attributes are, they have some limitations and tradeoffs. For example, you may face compatibility issues while using import attributes in your new projects since this new feature may not be supported by many development environments.

In addition, the `resolution-mode` attribute adds complexity to the module resolution process. You need to understand underlying concepts to take advantage of this feature.

Furthermore, while import attributes do provide some security improvements to your project, using them improperly may result in some potential security vulnerabilities:


- Incorrectly specifying import attributes may result in security vulnerability and ineffective access especially when the attribute affects the modules’ loading and execution
- Vulnerabilities can spread in cases where the external resources and configurations an import attribute relies on are compromised
- TypeScript doesn’t check the content of the attributes since they’re host-specific, so you’d have to verify imports and handle errors on them manually

You may also encounter some of these pitfalls while working with `import` attributes:


- **Error** **f****inding** **m****odule**: This is where TypeScript cannot find the module, either because it doesn’t exist or is incorrectly specified
- **Error** **f****rom** **i****mporting** **t****ype as** **v****alue**: This may lead to runtime errors
- **Duplicate** **i****dentifier** **e****rror**: This is where two imported modules export items with the same name
- **Type** **m****ismatch** **errors**: This can happen if you assign the wrong type to a variable

There are various ways to resolve each of these errors that are not specific to import attributes. You can read about [resolving common TypeScript module problems](https://blog.logrocket.com/common-typescript-module-problems-how-to-solve/) or [resolving errors during runtime type-checking](https://blog.logrocket.com/methods-for-typescript-runtime-type-checking/), but we won’t go into detail about solutions in this article.

## Alternatives to `import` attributes

While import attributes can help improve security and scalability in your TypeScript projects, there may be cases where you want to avoid the limitations and errors associated with them. Instead of using import attributes, you can use a few different methods to achieve similar functionality.

For example, you can explicitly separate type imports from module imports to differentiate type-only imports from code imports. In addition, depending on your project’s requirements, you might consider leveraging dynamic imports through `require` statements or organizing your codebase to minimize the need for import attributes. 

## Conclusion

In this tutorial, we saw how the straightforward process of setting up and using import attributes in TypeScript can enhance scalability and security. We covered why import attributes matter, how they work from a technical perspective, and how to configure your TypeScript environment to implement import attributes.

While import attributes provide many benefits, we also explored some of their drawbacks and common pitfalls. To avoid these issues, we introduced a few alternate methods that can help you achieve similar outcomes.

Leveraging import attributes can improve your program quality and development experience. I hope this guide is useful as you explore ways to implement them in your projects. Happy coding!





