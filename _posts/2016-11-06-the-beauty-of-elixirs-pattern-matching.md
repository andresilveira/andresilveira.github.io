---
title: The beauty of Elixir's pattern matching.
date: 2016-11-06 09:45:28.000000000 +02:00
categories: code
tags: code functional-programming elixir
img: /assets/elixir-purple-dna.gif
excerpt: "For someone coming from a more traditional <a href='https://en.wikipedia.org/wiki/Object-oriented_programming'>Object Oriented Programming</a> background, <a href='https://en.wikipedia.org/wiki/Functional_programming'>Functional Programming</a> might seem quite exotic. For me it looks really smart."
---

![]({{ site.baseurl }}/assets/elixir-purple-dna.gif){:alt="Pattern matching magic."}

[I'm playing around]({% post_url 2016-06-01-rookie-scheme-mistake %}) with functional programming and decided to give [Elixir](http://elixir-lang.org/) a try. In order to get familiar with the language I decided to solve some exercises from a great source called [Exercism](http://exercism.io/languages/elixir/about). They support several languages including Ruby, C++, F#, and many others. This post will cover part of my submission to an exercise called [Nucleotide Count](http://exercism.io/submissions/ad4fca9cc9294fed9cbefcacaa123c71).

## Starting from the basics

```elixir
defmodule NucleotideCount do
  @nucleotides [?A, ?C, ?G, ?T]
  ...
end
```

It's funny when you first dive into a language and start asking yourself "whatheheck are these symbols for?". I first assumed that `@nucleotides` was some kind of global variable from the module `NucleotideCount` and that was correct, there are also some reserved `@` words like `@docs`, so far so good. What about `?A` and the other question-mark prefixed letters? The `?` operator (I'm not sure if it's truly an operator but let's move on) gives you the integer representation of the character right next to it:

    iex(1)> ?A
    65
    iex(2)> to_string [65]
    "A"

Something that still intrigues me is the fact that you have to pass 65 as an `List` (`[65]`) to `to_string` in order to get `"A"` back, if instead we simply pass 65, the function returns `"65"`. Weird...

Now to the first part of the problem. Given a char list (DNA strand) like `'AAGCTA'`, I would like to count how many occurrences it has of a given char (nucleotide). Pretty straightforward, right? In ruby that could be done with something like `"AAGCTA".scan(/A/).count`, but we're not programming in Ruby, are we?

## Iterating in a list (without `.each` to help us out).

In Elixir `[1,2,3]` and `'ABC'` are [linked lists](http://elixir-lang.org/getting-started/basic-types.html#linked-lists) (the first contains integer and the later is a list of chars `'A','B','C'`) and they have a head (first element) and a tail (all the rest).

    iex(1)> [head|tail] = 'ABC'
    'ABC'
    iex(2)> head
    65 <-- remember this is the integer equivalent to 'A'
    iex(3)> tail
    'BC'

Cool, so what? If we add this and a bit of recursion (a function calling itself) we can *navigate* in a list.

```elixir
def count(strand, nucleotide) do
  _count(strand, nucleotide, 0)
end

def _count([head|tail], nucleotide, accumulator) do
  if head == nucleotide do
    _count(tail, nucleotide, accumulator + 1)
  else
    _count(tail, nucleotide, accumulator)
  end
end

def _count([], nucleotide, accumulator) do
  accumulator
end
```

Let's break this code into pieces. First we notice there are two functions with the same name `_count` and receiving both the same number of arguments, how is this possible? When you call a function, Elixir will pattern match the arguments into one of the functions defined. That is, if you call `_count` passing an empty list as the first parameter, the `_count([], ...)` will be executed, otherwise the other definition, `_count([head|tail], ...)`, will be called.

Also notice that in the first signature, `_count([head|tail], ...)`, we not only expect a list, we expect a list that has a head and a tail. And more, `head` and `tail` will be assigned exactly that, the first element of the list and the rest, respectively.

Now to the recursion part. As long as the list has a head, `_count` will keep calling itself passing as parameter the list minus the first element (aka tail). If the head of the list is equal to the nucleotide we're looking for, we call `_count` again but this time we increment the `accumulator` by 1. *So, are we going to call `_count` foreva?*, you ask. We would, if we didn't have what's called a [guard clause](https://en.wikipedia.org/wiki/Guard_(computer_science)). In our case, our guard clause is the `_count([]...)` which simply returns the value of `accumulator`, thus breaking the chain of calling `_count` over and over.

## Demo time \o/

I added some `IO.puts` around so we can inspect what's inside the variables during run time.

![]({{ site.baseurl }}/assets/recursion_in_action_elixir.gif){:alt="Recursion in action Elixir."}

## One last thing ™
I wasn't completely happy with that conditional `if head == nucleotide`. Then I had one of those *What if...* moments and tried the following:

```elixir
def count(strand, nucleotide) do
  _count(strand, nucleotide, 0)
end

def _count([head|tail], nucleotide = head, acumulator) do
  _count(tail, nucleotide, acumulator + 1)
end

def _count([_|tail], nucleotide, acumulator) do
  _count(tail, nucleotide, acumulator)
end

def _count([], _, acumulator) do
  acumulator
end
```

Did you notice what's happening here? We have introduced (yet) one more `_count` function, this time `_count([head|tail], nucleotide = head, ...)`. Yes, this function will only be called if:

1. first parameter is not an empty list
2. the nucleotide is equal to the first element of the list (head)

Isn't it awesome? We embedded the conditional checking right into the functions signatures!

### Update 09 Nov 2016
[@dcarral](https://github.com/dcarral) is a awesome colleague from Babbel and he was patient enough to review this post. And here are his suggestions (apart from typos):
* Maybe the usage of the term *guard clause* is not correct in the context I've mentioned. Oftentimes they are represented by some kind of conditional **inside** the function itself rather than outside. I still believe this is just an implementation detail and the idea of guard clause is to "guard" you from some undesirable result (in case of recursion, never leaving the recursion chain).
* [A link to pattern matching from Elixir's documentation](http://elixir-lang.org/crash-course.html#pattern-matching). I don't know if pattern matching is the exact term for what I meant (maybe function matching is a better definition).
* A much shorter way to count occurrences in ruby would be `"AAGCTA".count("A")` (well done 👍)
## Conclusion

Later on I've checked others' solutions and they mostly use `Enum.reduce`, `Enum.count` or something similar. But since I don't have any knowledge of the built in modules, I'm quite happy with that approach (using no more than functions). There's the drawback of somewhat *polluting* the module's implementation with several functions with apparently the same signature (`_count`) but I think this is a minor issue when we look on how simple they are to read and (even better) to test.

Are you banging your head against functional programming as well? Have you tried [Elixir](http://elixir-lang.org/)? Do you find this whole thing bullshit? **Let me know what you think!**
