---
title: Engineering You
date: 2018-10-09 09:45:28
categories: code
tags: code software-engineering one-talk-a-day
excerpt: I've recently watched a talk from <a href="https://www.real-logic.co.uk/about.html">Martin Thompson</a> about what and how to learn and become a better programmer today.
---

**TL;DR;** People from the past knew things and so should you.

## People from the past knew things
We now know much more than the people who started software engineering. After all use TDD (test driven development), microservices, hot reloading, components design, hipster coffee and so on. Right?

> A software system can best be designed if the testing is interlaced with the design instead of being used after the design
>
> <cite><small>Alan J. Perlis, 1968</small></cite>

Woooow, that's TDD right there Mr. Perlis. And that's not 2003 when Kent Beck "rediscovered" it, and that's not 1999 when Extreme Programming begun [1].

![]({{ site.baseurl }}/assets/damn.gif){:alt="People from the past knew things"}

Whenever I watch a software engineering talk, I think - damn, those guys knew it all! - I know it sounds a bit silly but it's a recurring thought of mine.

I'm not really experienced in other industries but I feel that software engineering has a tendency to forget the "classics" and pay way too much attention to the new shiny javascript framework. Don't get me wrong, I love the new shiny js framework, but forgetting the last 30 or 20 years of research is just plain inneficient. And that's what the average programmer does, jumping from js framework to js framework, being a passive user of technology whithout understanding it's underlaying and motivations.

## So, what to learn

That's another recurring recommendation on software engineering talks:

> Learn the basics

### Algorithms & Data Structure

I know, I know. You're thinking, but when was the last time you implemented (or even used) a A* search algorithm. The truth is we're kinda brute forcing on Web Development. Back in the days®, the computing/space constraints were much higher so clever solutions were needed. Nowadays, however, we just throw more AWS instances at it and call it a day. Knowing Algorithms & Data Structure will most likely make you a better programmer by forcing you thinking on the constraints and keep an eye on optimising your code in order provide a good user experience.

### Design fundamentals

```ruby
def get_user(user)
  DB.get("users", user.id)
end

def get_user(user_id)
  DB.get("users", user_id)
end
```

The two methods above are very similar, there's just one tiny difference. The first is coupled with the object user while the second knows nothing about it.
Knowing design fundamentals, coupling, single responsability principles, code cohesion, is critical for a professional programmer. That's what makes your code expensive (time = money) or cheap (and not painful) to maintain.

### Programming Paradigms

You don't need to know Lisp inside out, or babble about Monads all the time, but know about the existence and trade-offs of other programming paradigms such as Functional Programming makes us richer programmers, it forces us to think in a different perspective and so will make our code better.

### Decomposition & Abstraction

We hear all the time, _DON'T REPEAT CODE!_, but sometimes you still don't have the complete picture in your head. We usually abstract something whenever you've seen enough use cases of this something and extracted repeating characteristics of it, patterns if I may. If you haven't seen enough of use cases, just duplicate, it's ok!

> No abstraction is far better than the wrong abastraction

I think composition and abstraction is hard to learn because it involves experience. Working on several projects, making mistakes and fixing them.

### Math

![]({{ site.baseurl }}/assets/math-antichrist.gif){:alt="Math representing anti-christ"}

Different than other niches, we programmers have at least a tendency not to hate maths. But still, the average programmer (including myself) lacks knowlodge on simple things like Arithmatics, Set theory, combinatorics. Those are becoming more and more important with the growing need of concurrent programms.

### Business Domain / Communication

> It's really hard to build something meaningful if you don't know what for.

I've worked in tech team inside the marketing department of a big startup. At the beginning it was really hard for me to come up with novel solutions simply because I didn't understand how marketing works. If you don't understand your business domain, you'll most probably implement code instead of solving people's problems.

Linked to that, comes a very important and equally overlooked skill. Communication. Knowing how to talk and listen to people is requirement number one if you want to implement a solution. Learn how to express yourself better, ask for feedback, put yourself in their shoes, remember not everyone had the same background as you and understand that they also have interesting ideas, be curious!

## In conclusion

Marthin also talks about how and where to learn those "classic" skills but I won't go into details here.

The full talk with his slides can be found at [InfoQ - Engineering You](https://www.infoq.com/presentations/engineer-practices-techniques)

[1]:https://en.wikipedia.org/wiki/Test-driven_development
