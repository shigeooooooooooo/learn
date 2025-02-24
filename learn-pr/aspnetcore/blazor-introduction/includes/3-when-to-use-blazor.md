As a user interface framework, you can use Blazor to build web applications, but when should you use it, and how should you choose whether to run Blazor on a web server or deploy it with WebAssembly? Let's review the following criteria to help us decide whether to use Blazor and how we should host it:

* .NET familiarity
* Integration requirements
* Existing server configuration
* Complexity of the application
* Network requirements
* Code security requirements

## Decision criteria

Blazor helps you build applications quickly and reuse your existing .NET investments. You might not want to use Blazor if your application has code security concerns or if your organization isn't currently using .NET. Let's discuss some aspects of Blazor and help you make the right decision for your next application.

### .NET familiarity

You've already invested time into learning and building .NET applications. Whether for desktop, mobile, or the web, you're already a .NET pro and want to use your skills to build amazing applications with Blazor. That's great, and Blazor has many concepts like components, data-binding, event handling, dependency injection, and folder conventions that you're already familiar with.

If you haven't spent time with .NET and C# is new to you, Blazor will require you to learn some of these concepts before your productivity takes off with this framework.  

Perhaps you've already got significant experience and an investment with a JavaScript framework or two to build your web applications. The shift to Blazor can be significant, and might not be the best choice for you. If you'd like to learn C# and .NET, then you should still consider Blazor.

### Integration requirements

When you design and build a web application with Blazor, you'll probably want to integrate that application with one or more web services and other applications as part of a larger system. If some of these systems are already built with .NET, you can easily share libraries and NuGet packages between those applications and your Blazor application.

If you don't need to integrate with existing systems, that's okay too. In the future, you might want to share some of the content and business logic from your Blazor application, and that will be possible as your application grows into a larger system.

Maybe you have existing systems that you need to integrate with that support gRPC, OpenAPI, or other technologies besides .NET. You can use some of the existing tooling and client libraries from the vibrant .NET ecosystem to make your integration process easier.

### Existing web servers and configuration

Do you already have servers running web applications? Building and deploying a Blazor WebAssembly application as a static website to an existing server is a breeze. Hosting with existing ASP.NET Core applications is an option for you, as well.

If you don't already have web servers, there are plenty of hosting services available, including Azure. You can also place Blazor applications in a Docker container for ultimate flexibility in deployment and hosting, whether your application is in a WebAssembly or a Blazor Server configuration.

### Complexity of your application

Are you building an application that is computationally intensive? Would it benefit from running on a cluster of machines with distributed workloads? If the answer to these questions is yes, then you might want to consider building with Blazor Server.

If your application is rendering graphics for your consumers and could benefit from the real-time access to processor and memory, then we recommend you consider Blazor WebAssembly.

However, a simpler application without much complexity wouldn't see benefits from choosing a Blazor framework.

### Network requirements

You can build and deploy Blazor applications in two primary configurations that vary significantly in how they depend on network access. If your application should run in a configuration with minimal or zero network requirements, you could use Blazor WebAssembly with progressive web application bindings to allow your application to be installed from a web browser and run entirely on a user's machine.

Alternatively, you could choose to deploy zero code to your visitor's machine and keep all code on the server. In this configuration with Blazor Server, your application requires the network to function, and will send many small messages back and forth between the browser and the server.

### Code security requirements

For some industries, the deployment of an application and the geographical location of the application are requirements that cannot be circumvented. If your application needs to run in a validated configuration where application code isn't accessible to users and must execute from a specified location, we recommend considering Blazor Server for its web server hosted and isolated capabilities. You can secure the network between browser and server with TLS and various authentication frameworks to meet your security requirements.

## Guidance summary

Blazor is a great user interface framework for developers who are already familiar with .NET and want options in how they design and deliver HTML-based applications. Summarizing the criteria we've just explained, consider this table when deciding to use Blazor WebAssembly or Blazor Server in your next application:

| Criteria | You could choose Blazor Server because... | You could choose Blazor WebAssembly because... |
| --- | --- | --- |
| Developers are familiar with .NET | Building applications feel like an ASP.NET Core application | Building applications use your existing skills to run natively in the browser |
| Need to integrate with existing .NET investments | There's an existing model for integrating with ASP.NET Core applications | Allowing those resources to run natively in the browser gives better user interaction and performance than connecting to a web server |
| Existing web servers | Your existing web servers are running ASP.NET Core | You need to deploy your application to any server without need for server-side rendering |
| Complexity of the application | Your application has heavy processing requirements that benefit from distributed applications running in a data center | Your application can benefit from running with native processor speeds on the client |
| Network requirements | Your application will always be connected to a server | Your application can run in 'occasionally connected' mode without requiring constant interaction with a server |
| Code security requirements | Your application is validated or required to run in specific geographic locations that can be enforced with a server | Your application code can run anywhere on any device without this requirement |
