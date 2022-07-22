# Modularity

These notes are based on PointFree's video series, in particular [episode 171](https://www.pointfree.co/episodes/ep171-modularization-part-1) and [episode 172](https://www.pointfree.co/episodes/ep172-modularization-part-2).

## What is *modularity*

In this context, modularity means a Swift ***package***. A Swift package is also called a ***module***.

Using modules means we can create reusable modules and that we can explicitly describe a public interface that is accessible from the outside.

## Types of ***modularity***

There are two important types of modularity in a code base. One of these styles is very easy to get started with but it makes a relatively small impact on your code base, whereas the other can be quite difficult to achieve but has the biggest impact.

#### First type

The first style is to extract out commonly used code out of the main application target and into a module on its own. 

An example of this are the models used by the application or utility methods.

Typically these modules are just a bunch of simple structs representing plain data or pure functions, with few to none dependencies on other parts of the application. Thus, they are the easiest to extract out into their own modules.

Another example are services (or API wrappers), such as network clients or a wrapper around `LocationManager`.

This kind of modularity can be called "**model and helper**" modularity.


#### Second type

Even though "**model and helper**" modularity has some limitations and lower impact in terms of benefits (compilation time, boundaries between layers, etc), it is still a pre-requisite for the second type, more powerful, albeit more difficult, style of modularity, called “**feature**” modularity. 

A feature module defines a single, atomic feature of the application and bundle it up into a module.

The main benefits of building feature modules are the boundaries of the modules as well as compilation time benefits achieved by Swift optimisations.


