# Better deploying experience for web frameworks at the edge

As developers, we've all thought about the accelerated, everchanging world of web frameworks. It may seem a little confusing sometimes, because to excel at using one of this frameworks, it's necessary to have a deeper knowledge that goes beyond knowing the language itself. One of the the obstacles, and a important one is to know how to configure the project to different environments.

 Is it going to run on the cloud?
 What platform? 
 
 What about running your project on the edge of the network, laveraging low latency, security and availability thinking more about the project itself, instead of worrying about how to adapt it to a specific environment? 

## Adpating a project to run at the edge 

A project must be structured in a way to avoid friction with the host platform. It's not unusual to be caught in a situation where the settings necessary to run the application on one platform don't look anything like the settings to run on a different platform. It generates something called vendor lock-in, where the cost of changing the host, which includes time and money, is high in a way customers keep stuck with the current service provider. 

At Azion, we value and appreciate the power of open source software. With this thoughts in mind and the goal of empowering modern web development, our platform enables the initialization, deployment of projects in different web frameworks, such as: 

- Next.js
- React
- Vue
- Astro
- Angular
- Hexo
- Vite 

And Javascript itself. 

Azion has its own [Edge Runtime](), projected to provide optimal experience when it comes to running applications at the edge. This runtime empowers [Azion Edge Functions]() and opens up a world of possibilities for developers and companies. Alongside its own Edge Runtime, [Vulcan]() was developed to fill in the gaps and adpat the web frameworks to run natively at the edge. 

Vulcan simplify the integration of *polyfills* for Edge Computing, thereby revolutionizing the process of creating Workers, especially for the Azion platform.

> In case you're asking yourself what are these so called **polyfills**... In JavaScript, polyfills are code snippets used to provide modern functionality on environments that do not natively support it. They fill in the gaps, hence the term "polyfill", to ensure consistent behavior across different browsers. For example, let's say a runtime doesn't support a Node.js API and a project uses this specif API. During build time, Vulcan recognizes the signature of this API and replaces it for a relative API, with no need of adapting the project manually.

Vulcan is  proficient in setting up an intuitive and streamlined protocol for the creation of *presets*. This feature enhances customization and adaptation to unique project requirements, granting developers the flexibility they need to optimize their applications in a highly effective and efficient manner.

## Configuring a project

[Azion CLI]() comes as a outstanding tool to boost the developer experience. 

With the CLI installed on your environment, to initialize a process you just need to run: 

```bash
azion
```

Then, an interactive journey is presented and you choose the template.

![list of available templates](template-list.png)

After the template is selected, each framework will present a specif set of steps.

These web frameworks go through this adapting process, and the necessary configurations are applied, make them apt and optimal to run at the edge. 

These process is carried by an open-source framework adapter called [Vulcan]()