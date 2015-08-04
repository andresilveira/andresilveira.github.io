---
categories: code
---

Without looking at the answer, think for a second what would be the values of `a` and `b` in the code bellow.

{% highlight ruby %}
a = true and false
b = true && false
{% endhighlight %}

Yep, if you thought "hmm, if he's asking me to think, the answer probably is not the obvious one", you are right.
In the code above, `a` evaluates to `true` and `b` evaluates to `false`. This happens because of the operators' precedence. The operator `and` has lower precedence than the operator `=`, while the `&&` has higher precedence over `=`. 
Then you think "alright, and what does that mean?". To answer you, let's rewrite the same code in another way.


{% highlight ruby %}
(a = true) and false
b = (true && false)
{% endhighlight %}

Now it's easier to see that `a` is assigned to `true` **before** the conditional (the `and` part) happen.
It doesn't stop there. You smartly ask me, "André I tried this code on irb and after I typed `a = true and false` the console printed `false`". And I think it's nice that you tried the code yourself. The "trick" is that the **expression** is evaluated to `false` but the variable `a` is **assigned** to `true` beforehand. 

I've seen some developers that switch rapidly to the english logical operator since it's more readable. I find it in turn worse to read, I need some time to parse, for example, `and` to `&&`, but that might be because of my background with languages such as C and Java.

For a nicer (and more complete) view of the differences between the english and the symbolic versions of these logical operators I recommend [this post from Avdi Grimm](http://devblog.avdi.org/2014/08/26/how-to-use-rubys-english-andor-operators-without-going-nuts/)

