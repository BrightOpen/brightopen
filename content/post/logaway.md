---
title: "Logaway"
date: 2017-07-04T21:29:24+02:00
---

Hey! [BrightOpen.LogAway](https://github.com/BrightOpen/BrightOpen.LogAway) has been used in production for quite a while. It's especially fit for docker containers where all you want is to push something to console in a structured way with dependency injection at heart and mind. 

What about log4net and friends? I love log4net. I love it so much I have made a [JSON formatting extension](https://github.com/BrightOpen/log4net.Ext.Json) for it. Yet, I don't miss it (and all it's amazing features) in the docker world. It makes perfect sense for libraries to expose a minimalistic logging extension point in form of simple log interface. And it makes perfect sense for apps not to bloat up with feature rich configurable frameworks when all that's needed is log to console. In any case, it should be easy to just plug other logging frameworks in by creating an adapter... Alas, that's a little sore point for me. Log4net doesn't really make it easy for adapters to be made.

LogAway has grown pretty stable and useful on me. It embraces both fluent and DI approach while the ILog extension methods are just thin wrappers for adapters:

```csharp
[Fact]
public FunkyLogging FunkyExtensions()
{
    // fluent style
    var log =
        (null as ILog)
        .OrConsole()
        .Timed()
        .Seq()
        .Pid()
        .Host()
        .Evident("This","is","that")
        .For(this)
        .Member()
        .Section("wow");

    log.Write("That's funky, too");

    return this;
}
```

Each of those extension methods wraps the parent log with some added context or functionality. For instance Seq() adds statically unique sequentially incremented number to your logs. Combine that with host and pid information and you've got a neat unique log entry identifier that actually carries useful information (unlike GUIDs).

Ok, enough self-praise. Go check it out :o) [It's a nuget](https://www.nuget.org/packages/BrightOpen.LogAway/).

