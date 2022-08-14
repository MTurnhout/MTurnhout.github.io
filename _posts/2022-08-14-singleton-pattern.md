---
layout: post
title:  "Singleton pattern"
category: programming
tag: "software design patterns"
excerpt_separator: <!--more-->
---

The singleton pattern restricts a class to a single instantiation.

Was looking for a simple first post but it ended up being more extensive than expected. There are many ways doing something so simple. All with different complexity, pitfalls, and opinions. Wikipedia contains [examples][wikipedia-cs-example] in multiple programming languages. The C# example contains a lock and an object for the lock, but do you need those. Let's look at another example.
<!--more-->

Basic example:
```cs
namespace ConsoleTest;

public sealed class Singleton {
    public static Singleton Instance { get; } = new ();
    
    private Singleton() {}
}
```

MSIL:
```cs
.class public sealed auto ansi beforefieldinit
  ConsoleTest.Singleton
    extends [System.Runtime]System.Object
{

  .field private static initonly class ConsoleTest.Singleton '<Instance>k__BackingField'
    .custom instance void [System.Runtime]System.Runtime.CompilerServices.CompilerGeneratedAttribute::.ctor()
      = (01 00 00 00 )

  .method public hidebysig static specialname class ConsoleTest.Singleton
    get_Instance() cil managed
  {
    .custom instance void [System.Runtime]System.Runtime.CompilerServices.CompilerGeneratedAttribute::.ctor()
      = (01 00 00 00 )
    .maxstack 8

    // [4 40 - 4 44]
    IL_0000: ldsfld       class ConsoleTest.Singleton ConsoleTest.Singleton::'<Instance>k__BackingField'
    IL_0005: ret

  } // end of method Singleton::get_Instance

  .method private hidebysig specialname rtspecialname instance void
    .ctor() cil managed
  {
    .maxstack 8

    // [6 5 - 6 24]
    IL_0000: ldarg.0      // this
    IL_0001: call         instance void [System.Runtime]System.Object::.ctor()

    // [6 26 - 6 27]
    IL_0006: ret

  } // end of method Singleton::.ctor

  .method private hidebysig static specialname rtspecialname void
    .cctor() cil managed
  {
    .maxstack 8

    // [4 49 - 4 55]
    IL_0000: newobj       instance void ConsoleTest.Singleton::.ctor()
    IL_0005: stsfld       class ConsoleTest.Singleton ConsoleTest.Singleton::'<Instance>k__BackingField'
    IL_000a: ret

  } // end of method Singleton::.cctor

  .property class ConsoleTest.Singleton Instance()
  {
    .get class ConsoleTest.Singleton ConsoleTest.Singleton::get_Instance()
  } // end of property Singleton::Instance
} // end of class ConsoleTest.Singleton
```

Because of the private constructor the class can only be initialized from within the class. The class gets initialized in the static constructor which makes it thread safe. And static initialization is [guaranteed][static-init-source] to occur at some time before any static fields are accessed but not before a static method or instance constructor is invoked. So, the static initialization only occurs when "get_Instance" method is called which makes it lazy.

There are some pitfalls. When adding other static methods/properties and calling these will initialize the singleton instance. When classes are using static data from other classes in their static initialization you could get into scenarios where data will not be initialized yet because they are waiting for initialization to finish of the class calling them. The documentation [states][static-init-source] "Note that static initialization can occur at any time after a variable of the type is declared.". So, it might not be as lazy as hoped.

I never use singletons myself but if you want a safe implementation, you're probable best of using the one on [Wikipedia][wikipedia-cs-example]. But for most cases the one above will be fine.

[wikipedia-cs-example]: https://en.wikipedia.org/wiki/Singleton_pattern#C#
[static-init-source]: https://docs.microsoft.com/en-us/dotnet/fundamentals/code-analysis/quality-rules/ca1810