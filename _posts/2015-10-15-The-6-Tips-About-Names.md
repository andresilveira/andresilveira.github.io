---
title: The 6 Tips About Names
date: 2015-10-15 09:45:28.000000000 +02:00
categories: code
tags: [clean-coders]
img: /assets/shortening-all-the-names.jpg
excerpt: Some tips from Uncle Bob about naming conventions and sanity saving in programming.
---

![]({{ site.baseurl }}/assets/shortening-all-the-names.jpg){:alt="Shortening all the names!"}

I've started watching the videos of [CleanCoders](https://cleancoders.com/). It's a set of videos by Uncle Bob (Robert C. Martin), in which he talks about various programming topics as well as design concepts, OOP theory and so on. It's just a amazing source of wisdom and joy. Anyway, I decided to write one post per video that I watch, hopefully it will help me to understand better the concepts.

### 1. Choose your names thoughtfully
The first step here is to understand that names play an important role in you code. They are not only random letters put together, they should **communicate their intent** clearly. If you have to use a comment to explain what that name means, you failed.

### 2. Avoid Disinformation
For me, this is **the worst sin a programmer can commit**. Think about the following code:

{% highlight ruby %}
class Pair
  attr_accessor :first, :second, :third

  def initialize(first, second, third)
    self.first = first
    self.second = second
    self.third = third
  end

  def to_s
    puts "#{first}, #{second}, #{third}"
  end
end
{% endhighlight %}

Yes, I know it's a contrived example but you get the point. Now imagine you see an instance of `Pair` somewhere down the road without knowing about its implementation. That would take you some time until you realize how disgraceful the name `Pair` is for an object that takes 3 values.

But let's assume you're not so evil (no one is). What normally happens, as the code grows, is that some methods might change and at some point what they do no longer represent what their names communicate. **A name should say what it means and mean what it says**. Don't forget that if you have to read the code to understand what the name means, you failed.

### 3. Pronounceable Names
Raise your hand if you've already named a variable something like `test_qty` or `usr_name`, \o/. Why don't we write `test_quantity` or `user_name`? It can't be because we're lazy, right?
We're writing code that other people will read and possibly talk to us about it so make it easy for them, pick names that they can pronounce.

### 4. Avoid Encodings
Have you ever seen a variable called `name_str` just because it was String typed? Or a class called `IAccount` because it's an interface? With the IDE's we have full of tips about our variables and autocomplete stuff, it doesn't make much sense to use these name encodings, even more for folks that program in a statically typed language, the compiler will get you if you're wrong. For those who don't (ruby anyone?), let your tests do the job.

### 5. Use the Parts of Speech well
According to [Grady Booch](https://en.wikipedia.org/wiki/Grady_Booch), clean code should read like an well written prose. It means, we're telling a story with our methods, functions, classes, variables, etc. What do we need to write a story? Yes, you're right, we need **nouns**, **verbs**, **predicates** and so on. For example:

|       type       |      example      | grammar stuff |
|:----------------:|:-----------------:|:-------------:|
|      boolean     |    isPublished    |   predicates  |
| class / variable | Account / account |     nouns     |
|      methods     |      get_name     |     verbs     |

<br />
The goal here is to make sure that each line of your code reads like a normal phrase.

### 6. Scope rules
Variables name should be short (even to the size of one letter), if their scope is tiny and should be long if their scope is long. For example:
{% highlight ruby %}
  users.map { |u| u.name.downcase }
{% endhighlight %}
In this case, the one-letter variable `u` is not that harmful because we can directly see that it's an user. But if you stick an `u` 20 lines bellow where it was declared, the person who's reading the code will probably have problems to remember what that variable represents.

Method and classes names follows the opposite rule. Their names should be short if they have a long scope (in this sense, if they are used widespread in your code), and should be long and descriptive if they have a short scope. For example:

{% highlight ruby %}
def serve(socket)
  begin
  try_process_instructions(socket)
  rescue Exception => e
  close_enclosing_service_in_separate_thread
  end
end
{% endhighlight %}

Notice that the method `serve` has a short name and might be used in many places through out the code, but the `close_enclosing_service_in_separate_thread` is used in a short scope and thus can be long a explain its intent with more words.


Remember, we're not (maybe you are) programming in C and we're not in the 90s anymore. Our tools became powerful with code analysis, type checking and coffee making and so on. We also don't need to save 5 bytes by shortening all the names.
