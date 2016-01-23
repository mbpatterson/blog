---
layout: post
title: "Getting Started with Elixir Programming"
date: 2016-1-12
categories: elixir
---
Today I began working through the `Programming Elixir` book by Dave Thomas. I have gone through the first 5 chapters and just finished learning about anonymous functions in Elixir. The book has been fantastic so far and the included exercises after each core concept have been very helpful. Read on for my takeaways so far...

The first major point in the book is that programming should be about transforming data and not worrying about state. Elixir stresses data immutability. It also handles assignment differently than I would have guessed. The equal sign in Elixir is not an assignment and is instead more like an assertion. It calls = a `match operator`. Looking at some examples this starts to make more sense.

    iex> a = 1
    1
    iex> 1 = a
    1
    iex> 2 = a
    ** (MatchError) no match of right hand side value: 1

Elixir only changes the value of a variable if it is on the left hand side of an equals sign, if the variable is on the right hand side it is replaced by its value. It will always try to make the value on the left hand side equal to the value on the right. This process is called `pattern matching`. If you don't need to capture a value during matching it can be excluded with the special _ (underscore) variable. This acts like a variable but immediately discards any value given to it making it act like a wildcard variable.

Data immutability is a core of Elixir and functional programming in general. Once a variable references a value it will always reference that value until the variable is rebound. If you need to modify that value it will make a copy of the original variable which will contain the new value. This makes concurrency much easier.

Value types represent numbers, names, ranges, and regular expressions. There is no fixed length on integer size and floating point calculations are accurate to 16 digits. Something new I learned is the `atom` type. An atom is a constant that represents something's name. Its name is its value and it is used most frequently to tag values. `PID`s are references to a process.

Collections can contain values of any type including other collections. The first is a `tuple`. This is an ordered collection of values that, once created, cannot be modified. It is written as

    {value1, value2, etc}

`Lists` are linked data structures and are written as

    [value1, value2, etc]

This can also be thought of as `[head, tail]` where `tail` can also be a list.

Last I learned about anonymous functions. These are created with the `fn` keyword.

    fn
      parameters -> body
    end

Anonymous functions can be assigned to variables and are called with var_name.(parameter1, pparameter2).

    sum = fn (int1, int2) -> int1 + int2 end
    sum(1,2)
    3

There is a shorthand way to write anonymous functions which is really cool. The below example is the same as above.


    sum = &(&1 + &2)
    sum(1,2)
    3


One function can also contain multiple different bodies. Whichever pattern matches successfully will run.

    two_bodies = fn
      (1) -> "One"
      (2) -> "Two"
    end
    two_bodies.(2)
    "Two"

Functions can also return other functions. Parameters can be passed to nested functions and functions can be used as parameters for other functions.

So far I have taken a lot away from this book and am looking forward to learning more.
