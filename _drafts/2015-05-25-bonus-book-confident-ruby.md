---
title: 'BONUS BOOK: Confident Ruby'
date: 2015-05-25 18:21:19.000000000 +02:00
categories: []
---
![]({{ site.baseurl }}/assets/confident-ruby-book-cover.jpg){:alt="Confident Ruby Book Cover"}

Yes, I know I said I'd start with the [10 most upvoted books]({% post_url 2015-05-25-the-next-10-books %}) but I just finished [Confident Ruby](http://www.confidentruby.com){:target="_blank"} by Avdi Grimm. I found it to be really interesting mainly because of its goal.

  > Bring back the joy of programming in on going projects.
  
Yep, I'm sure we all already experienced the feeling of "nah this feature/project is driving me nuts :/ ". The author makes an analogy between a application's code and a story in a sense that our code should tell a coherent narrative rather than a pile of *nil* checking and conditionals here and there.
The book is structured as a "easy to apply" patterns catalog. And, in turn, those patterns are can be used in one or more of the four stages of a method: **gathering inputs, performing work, deliver response and handling errors**.

Here are a short list of the ones I could apply the most and also made my life easier:

#### Always use built in conversors on 'boundary' methods

Methods that receive inputs from outside its class should take an extra care about the inputs it accepts without being too obsessive. [Principle of Robustness](http://en.wikipedia.org/wiki/Robustness_principle){:target="_blank"}, be conservative in what you send, be liberal in what you accept). So, time to pull out those old friends `#to_s` and `#to_i`, for example, and use them whenever you need a input to behave like a String or a Integer in a method.


