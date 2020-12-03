+++
author = "the mad geek"
categories = ["mediatr"]
date = 2020-03-07T11:00:00Z
description = ""
draft = true
featured = ""
featuredalt = ""
featuredpath = "date"
linktitle = "Link Title"
tags = ["cqrs", "basic", "c#"]
title = "MediatR Awesomesauce"
type = "post"

+++
Full code example of what we build here will here can be found \[[https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce](https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce "https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce")\]([https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce](https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce "https://github.com/shiggan/mad.geek.nz-code/202003_mediatr-awesomesauce")) on GitHub.

This post is a part of a series:

* MediatR Awesomesauce
* MediatR with Autofac

Were going to assume that you have the following bits installed

* Visual Studio Code
* DotNet Core

As we move through the demo vs-code may prompt you to install/update other stuff (e.g. OmniSharp for debugging C# files); let the installations/updates proceed.

## **What is MediatR**

[https://github.com/jbogard/MediatR](https://github.com/jbogard/MediatR "MediatR") is an awesome little libary that lets you build solutions implementing the _mediator_ pattern. In a nutshell, the _mediatr_ pattern definition (stolen from [https://en.wikipedia.org/wiki/Mediator_pattern](https://en.wikipedia.org/wiki/Mediator_pattern "wikipedia")) :

> The essence of the Mediator Pattern is to "define an object that encapsulates how a set of objects interact". It promotes loose coupling by keeping objects from referring to each other explicitly, and it allows their interaction to be varied independently. Client classes can use the mediator to send messages to other clients, and can receive messages from other clients via an event on the mediator class.

Broken down the Mediator pattern looks more like this :

* Its an object that encapsulates how objects interact via a messaging system.
* It promotes loose coupling keeping objects form directly referencing one another.
* Explicitly allows any interaction to be independently varied.

If you squint and slant your head to the side its sort of like a message hub of sorts; something declares that its interested in a message by subscribing to it; another thing 'publishes' messages; and some magic inbetween.

## **Why is this even a thing**

MediatR is a great messaging system; over at my day job we have been using it with great success particularaly with refactoring existing and building out new web services; these days almost all systems I build utilizes **CQRS** (command, query, responsibility, segregation), as it is a great fit for applications that do a lot of reads and writes.

Some of the tried and true blog posts around CQRS

* [https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/ "https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-command-side-of-my-architecture/")
* [https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/](https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/ "https://blogs.cuttingedge.it/steven/posts/2011/meanwhile-on-the-query-side-of-my-architecture/")
* [https://rob.conery.io/2014/03/03/repositories-and-unitofwork-are-not-a-good-idea/](https://rob.conery.io/2014/03/03/repositories-and-unitofwork-are-not-a-good-idea/ "https://rob.conery.io/2014/03/03/repositories-and-unitofwork-are-not-a-good-idea/")

## Show me some code

So now we got the why and the how lets move onto some code; at the end of this post we will have a dead simple handler; it will look mostly like the documentation but thats ok because in the next few pages we will be extending it with pipelines, and a declariative aproach for pipelining your handlers.

Full code example of what we buld here will here can be found [https://github.com/shiggan/mad.geek.nz-code/mediatr_awesomesauce_1_of_n](https://github.com/shiggan/mad.geek.nz-code/mediatr_awesomesauce_1_of_n "here") on GitHub.

## **A Basic Handler**

Create something that will run the demos; open up your favorate terminal and key in

    md mediatr-awesomesauce
    cd mediatr-awesomesauce
    dotnet new xunit
    dotnet add package MediatR --version 8.0.0
    dotnet add package FluentAssertions --version 5.10.2
    code .

> **cli** what this does is create a new directory, create a new project in that directory, add the mediatr package to it, and start visual studio code; code may prompt you to add packages

Open up \`UnitTest1.cs\` and drop the following code into it.

    using System;
    using System.Threading.Tasks;
    using FluentAssertions;
    using MediatR;
    using Xunit;

Create a \`Request\`, and a \`Response\` object.

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
    }

> \`Ping\` is the request, \`Pong \` is the response; the important bit here is that MediatR needs to be able to match up Requests and Responses; so that \`IRequest<Pong>\` is telling MediatR that it should match this Request <-> Response pair to a particular Handler which we will go into next.

Create a \`Handler\` object.

      public class PingHandler : RequestHandler<Ping, Pong>
      {
        protected override Pong Handle(Ping request)
        {
          return new Pong { Message = request.Message + " Pong" };
        }
      }

> We are creating a \`RequestHandler\` which will handle all \`Ping\` <-> \`Pong\` Messages.

Create a very simple test that calls our handler directly

      [Fact]
      public async Task Should_call_handler()
      {
        IRequestHandler<Ping, Pong> handler = new PingHandler();
    
        var response = await handler.Handle(new Ping() { Message = "Ping" }, default);
    
        response.Message.Should().Be("Ping Pong");
      }
    }

We have barely scratched the surface; at this stage we are not even invoking our Handler through MediatR itself; all of the cool stuff will start happenning in the next post where I will build on this trivial example with plugging in Autofac for Dependency Injection and some Pipeline handlers.