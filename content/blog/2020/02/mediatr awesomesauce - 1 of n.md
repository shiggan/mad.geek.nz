+++
author = "the mad geek"
categories = ["mediatr"]
tags = ["C#", "basic", "cqrs"]
date = "2020-02-15"
description = "" 
featured = "mediatr awesomesauce - 1 of n/featured.jpg" ## reletave to /static/img/YYYY/MM
featuredalt = "https://pixabay.com/photos/code-programming-hacking-html-web-820275/"
featuredpath = "date"
linktitle = "Link Title"
title = "MediatR Awesomesauce"
type = "post"
draft = false

+++

Full code example of what we buld here will here can be found [https://github.com/shiggan/mad.geek.nz-code/202002_mediatr-awesomesauce](https://github.com/shiggan/mad.geek.nz-code/202002_mediatr-awesomesauce) on GitHub. 

Were going to assume that you have the following bits installed

* [Visual Studio Code](https://code.visualstudio.com/Download)
* [DotNet Core](https://dotnet.microsoft.com/download)

As we move through the demo vs-code may prompt you to install/update other stuff (e.g. OmniSharp for debugging C# files); let the installations/updates proceed.

## What is MediatR

[MediatR](https://github.com/jbogard/MediatR) is an awesome little libary that lets you build solutions implementing the _mediator_ pattern. In a nutshell, the _mediatr_ pattern definition (stolen from [wikipedia](https://en.wikipedia.org/wiki/Mediator_pattern)) :
> The essence of the Mediator Pattern is to "define an object that encapsulates how a set of objects interact". It promotes loose coupling by keeping objects from referring to each other explicitly, and it allows their interaction to be varied independently. Client classes can use the mediator to send messages to other clients, and can receive messages from other clients via an event on the mediator class.

Broken down the Mediator pattern looks more like this :

* Its an object that encapsulates how objects interact via a messaging system.
* It promotes loose coupling keeping objects form directly referencing one another.
* Explicitly allows any interaction to be independently varied.

If you squint and slant your head to the side its sort of like a message hub of sorts; something declares that its interested in a message by subscribing to it; another thing 'publishes' messages; and some magic inbetween.

## Why is this even a thing

MediatR is a great messaging system; over at my day job we have been using it with great success particularaly with refactoring existing and building out new web services; these days almost all systems I build  utilizes  **CQRS** (command, query, responsibility, segregation), as it is a great fit for applications that do a lot of reads and writes.

Some of the tried and true blog posts around CQRS 

* [https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/)
* [https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/)
* [https://rob.conery.io/2014/03/03/repositories-and-unitofwork-are-not-a-good-idea/](https://rob.conery.io/2014/03/03/repositories-and-unitofwork-are-not-a-good-idea/)

## Show me some code

So now we got the why and the how lets move onto some code; at the end of this post we will have a **dead simple** handler; it will look mostly like the documentation but thats ok because in the next few pages we will be extending it with pipelines, and a declariative aproach for pipelining your handlers.

Full code example of what we buld here will here can be found [here](https://github.com/shiggan/mad.geek.nz-code/mediatr_awesomesauce_1_of_n) on GitHub.

## A Basic Handler

Create something that will run the demos; open up your favorate terminal and key in

```
md mediatr-awesomesauce
cd mediatr-awesomesauce
dotnet new xunit
dotnet add package MediatR --version 8.0.0
dotnet add package FluentAssertions --version 5.10.2
code .
```

> **cli** what this does is create a new directory, create a new project in that directory, add the mediatr package to it, and start visual studio code; code may prompt you to add packages 

Open up `UnitTest1.cs` and drop the following code into it.

```csharp
using System;
using System.Threading.Tasks;
using FluentAssertions;
using MediatR;
using Xunit;
```

Create a `Request`, and a `Response` object.

```csharp
public class RequestHandlerTests
{
  public class Ping : IRequest<Pong>
  {
    public string Message { get; set; }
  }

  public class Pong
  {
    public string Message { get; set; }
  }
```
... talk ...

Creating a `RequestHandler` which is a class that will handle all `Ping` requests; this particular one will return `Pong` responses.
```csharp
  public class PingHandler : RequestHandler<Ping, Pong>
  {
    protected override Pong Handle(Ping request)
    {
      return new Pong { Message = request.Message + " Pong" };
    }
  }
```
... talk ...

... the test
```csharp
  [Fact]
  public async Task Should_call_handler()
  {
    IRequestHandler<Ping, Pong> handler = new PingHandler();

    var response = await handler.Handle(new Ping() { Message = "Ping" }, default);

    response.Message.Should().Be("Ping Pong");
  }
}
```
... talk ...