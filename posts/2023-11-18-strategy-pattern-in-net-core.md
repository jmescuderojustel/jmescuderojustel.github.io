---
title: Strategy Pattern in .NET Core
date: 2023-11-18 19:08:13
tags: [.NET Core, Patterns, C#]
categories: [Software development]
---

In software development, patterns help us solve common problems or situations by following a well-known set of approaches. Those approaches have been tested for a long time and have proven useful in some scenarios. Patterns also allow developers to speak the same language, and refer to some development techniques by the pattern name, instead of explaining their behavior.

Patterns are language agnostic, which means they are not bound to a programming language or a stack. However, some languages make easier the implementation of some of them than others.

There are lots of software development patterns, being the most famous the ones published by the Gang of Four. You have a summary of them [here](https://www.dotnettricks.com/learn/designpatterns/gang-of-four-gof-design-patterns-in-net).

In this post, we are talking about how to implement the “strategy pattern” in a .NET Core API.

## What is the strategy pattern?

Have you found yourself adding `case`s to a `switch` statement, thinking that every new `case` adds more and more complexity to your code? Do you find lots of `else if` nested in your code? Well, that means that your code behaves differently depending on some input data, which is fine.

Take a look at the following code:

```csharp
if (input == "books")
{
    // Do something about books
}
else if (input == "movies")
{
    // Do something about movies
}
else if (input  == "albums")
{
    // Do something about albums
}
```

The problems here are:

- Every new behavior means adding another branch to your code, be it a case or an else if, which makes your code difficult to read and prone to errors, due to the complexity of ensuring your code goes only through the ways you want and expect.
- In the same way, any new behavior requires that you change the same piece of code. Of course, you could move the code to another method, or even file, but the branch will still need to be added.

The strategy pattern helps us to solve this problem. It will allow us to add new functionality in an easy and modular way.

## An example of the strategy pattern in .NET Core

> You can take a look at the GitHub repository to find this example, and run it on your local machine: https://github.com/jmescuderojustel/blog-resources/strategy-pattern-net-core

Take a look at the following piece of code.

```csharp
app.MapGet("/summary/v1/{topic}/{title}", (string topic, string title) =>
{
    if (topic.ToLower() == "books")
    {
        // Get a summary from Amazon
        // Get reviews from Goodreads
        return new SummaryModel(title, "Book excerpt", 4.5, new List<string> { "Amazon" });
    }
    else if (topic.ToLower() == "movies")
    {
        // Get reviews and summary from IMDB
        // Get the place where the movie can be found
        return new SummaryModel(title, "Movie summary", 3.9, new List<string> { "Netflix" });
    }
    else if (topic.ToLower() == "albums")
    {
        // Get reviews from Metacritic
        // Get a track listing from Metacritic to include in the summary
        // Get the place where the album can be found
        return new SummaryModel(title, "Album track listing", 4.3, new List<string> { "Spotify", "LastFM" });
    }

    throw new NotImplementedException("We still do not support that topic. Come again later!");
});
```

This is just an endpoint (developed using a minimal API) that returns a summary of a movie, book, or music album. You just provide the topic (movie, book, or music album) and the title of the item, and you get the summary.

Of course, depending on the topic, the summary will be made of information from different sources (IMDB, Metacritic…) that will require different code for each situation.

You are already noticing some caveats. Every time we add another topic, we need to modify this endpoint. We also have a set of branches in the code, made up of `if` and `else if`, which make the code more difficult to follow as we add more and more options.

## A first approach to the strategy pattern

Wouldn’t it be great to have a list of strategies that would enter into play when the topic is the one they know about?

We are refactoring our endpoint to have a list of strategies.

```csharp
app.MapGet("/summary/v2/{topic}/{title}", (string topic, string title) =>
{
    var strategies = new List<IStrategy>
    {
        new BooksStrategy(),
        new MoviesStrategy(),
        new AlbumsStrategy(),
    };

    var strategy = strategies.SingleOrDefault(s => s.CanHandle(topic));

    if(strategy is null)
    {
        throw new NotImplementedException("We still do not support that topic. Come again later!");
    }

    return strategy.Handle(title);
});
```

As you see, we have a list of strategies, and all of them meet the same: `IStrategy`.

This interface is quite straightforward:

```csharp
public interface IStrategy
{
    bool CanHandle(string topic);
    SummaryModel Handle(string title);
}
```

Any strategy needs to provide a method to know if it can handle a request. In this case, we only have the topic to make the decision, but you could make it as complex as your requirements need. It also provides a method that finally handles the request and generates the response.

This means that, for example, strategies would be like the following:

```csharp
public class AlbumsStrategy : IStrategy
{
    public bool CanHandle(string topic) => topic.ToLower() == "albums";

    public SummaryModel Handle(string title)
    {
        // Get reviews from Metacritic
        // Get a track listing from Metacritic to include in the summary
        // Get the place where the album can be found
        return new SummaryModel(title, "Album track listing", 4.3, new List<string> { "Spotify", "LastFM" });
    }
}
```

```csharp
public class BooksStrategy : IStrategy
{
    public bool CanHandle(string topic) => topic.ToLower() == "books";

    public SummaryModel Handle(string title)
    {
        // Get a summary from Amazon
        // Get reviews from Goodreads
        return new SummaryModel(title, "Book excerpt", 4.5, new List<string> { "Amazon" });
    }
}
```

```csharp
public class MoviesStrategy : IStrategy
{
    public bool CanHandle(string topic) => topic.ToLower() == "movies";

    public SummaryModel Handle(string title)
    {
        // Get reviews and summary from IMDB
        // Get the place where the movie can be found
        return new SummaryModel(title, "Movie summary", 3.9, new List<string> { "Netflix" });
    }
}
```

As you can see, each of them handles a different topic and does it in a different way.

Logic regarding each strategy is located inside each corresponding class. Also, testing each of the strategies is now much easier, as we can test each piece of code independently.

Last but not least, we have freed up our endpoint of knowing the strategies. Now, the endpoint does not know about what happens under the hoods. It just looks for the appropriate strategy and asks it to handle the request. Adding a new case (a topic for “games”, for example) would just require a new strategy that handles that topic, and adding it to the strategies list.

## Dependency injection and the strategy pattern

Dependency injection is quite a powerful topic. It is out of the scope of this article, but, to sum up, it allows us to define the dependencies in the class constructors and rely on someone else (in this case the .NET Core dependency injection) to provide the required instances. This is a very simplistic way of explaining it. If you want to go deeper, which I really encourage you to, please go to https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-7.0 to start.

Here we are using the .NET Core dependency injection to provide us with the strategies. This way, we won’t need to manage a list of strategies in the endpoint. We will have the full list injected in the endpoint method.

```csharp
var builder = WebApplication.CreateBuilder(args);
// We added the injection of the strategies
builder.Services.AddTransient<IStrategy, BooksStrategy>();
builder.Services.AddTransient<IStrategy, MoviesStrategy>();
builder.Services.AddTransient<IStrategy, AlbumsStrategy>();

var app = builder.Build();

app.MapGet("/summary/v3/{topic}/{title}", (string topic, string title, IEnumerable<IStrategy> strategies) =>
{
    var strategy = strategies.SingleOrDefault(s => s.CanHandle(topic));

    if (strategy is null)
    {
        throw new NotImplementedException("We still do not support that topic. Come again later!");
    }

    return strategy.Handle(title);
});
```

If you take a look at the code, we are injecting all strategies with the `IStrategy` interface. This allows us to inject a `List` of strategies in the endpoint, access all of them, and look for the one we require, depending on the output.

Adding another strategy is trivial. Just add the class with the strategy, based on the `IStrategy` interface, and add it to the dependency injection. Now, it automatically will be available in the list of strategies injected in the endpoint and will be ready to be used.

## Conclusion

Patterns help solve well-known problems we may find while developing. The strategy pattern helps us by simplifying the way we manage situations in which we have several paths depending on an input.

Here we have seen how to make use of the power of .NET Core to implement this pattern and make our code more readable and testable.
