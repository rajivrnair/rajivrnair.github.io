---
layout: post
title: Lambdas vs Closures
comments: true
---
A ***Lambda Expression*** is merely an anonymous function - a language construct. Giving a lambda a name makes it a named function.

A ***Closure*** on the other hand is a technique to implement first-class functions. They are functions that *'close over the environment in which they are defined'* which just means that they have access to variables present in the context where they were created, not necessarily just those in their parameter list.

As with most things in life, the best way to understand something is through examples. So here goes:

**Std disclaimer:** *closures/lambdas are language-dependent. They don't mean quite the same thing in different languages nor are they implemented in the same way. For e.g: Java implements lambda expressions via closures. Hence, these terms are used interchangeably.*

###Lambdas
{% highlight ruby %}
def select_runners(people)
  return people.select { |p| p.can_run? }
end
{% endhighlight %}

Here the block of code passed to the select method is a lambda. This lambda is executed for each element in the collection.

###Closures
{% highlight ruby %}
def select_runners(people)
  min_speed_cutoff_kph = 10
  return people.select { |p| p.can_run? && p.speed > min_speed_cutoff_kph }
end
{% endhighlight %}

Notice how the lambda refers to *min\_speed\_cutoff\_kph*, a variable defined outside of it, in the scope of its enclosing function. It is now said to be closed over the environment it was defined in.

The above code can also be written as:

{% highlight ruby %}

def can_run_fast_enough?(cutoff_speed_kph)
  return lambda { |p| p.can_run? && p.speed > cutoff_speed_kph}
end

can_run_faster_than_10_kph? = can_run_fast_enough?(10)

milkha_singh = Person.new
milkha_singh.speed = 20
can_run_faster_than_10_kph?.call(milkha_singh)

{% endhighlight %}

Now the *can\_run\_faster\_than\_10\_kph?.call(milkha_singh)* checks if Milkha Singh can run faster than 10 kph by calling the lambda in *can\_run\_fast\_enough?*. The value of the *cutoff\_speed\_kph* is bound to 10 which is the value it was created with. Even if this went out of scope, the binding will remain.

###TL;DR;
**To put it simply, a closure is a lambda paired with the environment in which it was created, which then closes over the lambda.**

###Excellent Reads:
[Martin Fowler on Lambdas](http://martinfowler.com/bliki/Lambda.html)  
[StackOverflow](http://stackoverflow.com/questions/220658/what-is-the-difference-between-a-closure-and-a-lambda)  
[The Java Lambda FAQ](http://www.lambdafaq.org/)  
[Closures in Ruby](https://innig.net/software/ruby/closures-in-ruby)  
