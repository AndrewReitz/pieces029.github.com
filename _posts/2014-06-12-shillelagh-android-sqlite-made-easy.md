---
layout: post
title: "Shillelagh, Android Sqlite Made Easy"
description: ""
category:
tags: []
---
{% include JB/setup %}

I just released [Shillelagh](https://github.com/pieces029/shillelagh) about a week
ago and I realized I hadn't said much about it. So here are my explanations
of the how and why I wrote it. The library is still in it's early stages, but I
do plan on making a lot of updates. If you would like a feature please request it.

## The problem

Writing SQL inside of strings in Java is tedious and likely to cause errors.
Setting up a Sqlite database and throwing some models in it in Android always
felt cumbersome and took way longer than it should, and thus, I created
Shillelagh. Don't get me wrong I know there are ORMs for Android but I still
felt like they took too much effort. I wanted something fast and easy, that I could
just throw into my application.

## How it works

It's really straight forward, use the `@Table` annotations on your model objects.
Add a `@Id` to a field with the type long. This will be used as the identifier.
I could have done this automatically but then you could never query by id.
I choose explicit adding of this for simplicity, things could get weird without
it. Finally, add `@Field` to the fields you want in the database. This was more
done for my sake of making it easier to parse which fields should and should not
be included rather than including them all and making an annotation to exclude.

At compile time classes annotated with `@Table` are searched for, when they are
found the annotation processor goes through the class and looks for `@Id` and
`@Field` once these have all been parsed, the processor generates an internal
class that has standard database CRUD operations and a few other auto generated
convenience methods. Since these are generated at compile time, when you are
coding you won't really know anything about them. That's where the Shillelagh
class comes in. It uses reflection to grab these internal classes (it caches
them after the first lookup) and calls functions on it. Allowing you to write code
rather than worry about creating SQL strings.

The create and drop methods are static since they are used to construct the database
the Shillelagh instance requires. I recommend using [Dagger](https://github.com/square/dagger)
to manage only having one instance of Shillelagh, but you can, like in the
sample project, create a variable in the Application class and access it that way
(I prefer this over singletons, singletons are hard to test, are equivalent
to global objects, and encourage poor programming practices, I avoid them at
all costs).

## The future

Right now only

- Integer
- Double
- Float
- Long
- Short
- String
- Date
- Boolean

and corresponding primitive types are supported. I plan to add in mapping of all
other types soon. One to many relation ships, constraints, and RxJava support.
The reason for RxJava, is currently the map functionality of Shillelagh maps all
objects retrieved into a list. For large sets returned from a query this might be
problematic taking up to much memory. Also currently Shillelagh does not offload
it's work to a background thread. Wrapping everything with RxJava would allow for
easy background thread operations.

Well that's all. Hope you like the library, and I will be updating it as soon
as I get some free time.
