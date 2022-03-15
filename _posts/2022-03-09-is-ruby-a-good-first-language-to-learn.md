---
layout: blog
title:  "Is Ruby a good first language to learn?"
date:   2022-01-09
categories: ruby
---

# Is Ruby a good first language to learn?

I've been working with Ruby for 10+ years. It is elegant, flexible, powerful, and kind. Some other languages I've used are Java, PHP, Python and JavaScript.
I think if you are new to programming it _is_ a good first language to learn and I will try to explain why.

## Readable

To me this is the most important factor. You will spend a lot more time reading code than writing it, so it is important that the language you are learning is easy to read. Ruby when written properly reads a lot like english. Let me show you:

#### Blocks

If you are writing an `if` statement you will have to close it with `end`. That makes it easier to visualize that part of the program and it's scope.

```ruby
if condition
  do_something
end
```

Some languages use _significant spaces_, which means that not every whitespace is treated the same way. An accidental misalignment of whitespace can cause a mysterious, hard-to-detect bug and it also can make blocks of scope confusing to identify.

#### Looping

```ruby
list.each do |item|
  item.do_stuff
end
```

Pretty easy to guess what this does right?

#### Last one:

If you want to get the sum of all the digits of an integer, you can do it in Ruby like this:

```ruby
12345.digits.sum
```

This is how you would do it in Java:

```java
import java.math.BigInteger;
public class SumDigits {
    public static int sumDigits(long num) {
	return sumDigits(num, 10);
    }
    public static int sumDigits(long num, int base) {
	String s = Long.toString(num, base);
	int result = 0;
	for (int i = 0; i < s.length(); i++)
	    result += Character.digit(s.charAt(i), base);
	return result;
    }
    public static int sumDigits(BigInteger num) {
	return sumDigits(num, 10);
    }
    public static int sumDigits(BigInteger num, int base) {
	String s = num.toString(base);
	int result = 0;
	for (int i = 0; i < s.length(); i++)
	    result += Character.digit(s.charAt(i), base);
	return result;
    }

    public static void main(String[] args) {
	System.out.println(sumDigits(1));
	System.out.println(sumDigits(12345));
	System.out.println(sumDigits(123045));
	System.out.println(sumDigits(0xfe, 16));
	System.out.println(sumDigits(0xf0e, 16));
	System.out.println(sumDigits(new BigInteger("12345678901234567890")));
    }
}
```

And here is how you would do it in JS:

```javascript
function sumDigits(n) {
	n += ''
	for (var s=0, i=0, e=n.length; i<e; i+=1) s+=parseInt(n.charAt(i),36)
	return s
}
for (var n of [1, 12345, 0xfe, 'fe', 'f0e', '999ABCXYZ']) document.write(n, ' sum to ', sumDigits(n), '<br>')
```

There are more examples [in this link.](http://rosettacode.org/wiki/Sum_digits_of_an_integer) You will see that no one accomplishes that as gracefully as Ruby does.

## Flexibility

If you need to do something in Ruby - loop through an array and find an element for example - there are a lot of different ways you can accomplish that.
That flexibility can be welcome sometimes. Altough if you are an anxious person you may struggle trying to figure out if the way you solved a problem is really the _best_ way haha.

## Ruby on Rails and strong community

Ruby on Rails has been around for 15+ years and up to this date I still don't think there is a better tool to get a (web) product up and running. There are _a ton_ of [gems](https://rubygems.org/) - if you think of a piece of functionality or problem, there is probably a gem solving it for RoR. Some of the philosophies it introduced were popularized and are now used in many other frameworks. Plenty of companies started (and still run) Ruby on Rails in production, with lots and lots of daily users, that means that you won't find hard to find jobs if you know the tool.

There is also a plethora of places where Ruby devs hang out and help one another, some of them:

* [r/rails](https://www.reddit.com/r/rails)
* [r/rubyonrails](https://www.reddit.com/r/rubyonrails)
* [r/ruby](https://www.reddit.com/r/ruby)

Because the language is mature is very unlikely you won't find an answer to your question when you need help.

## Speed

Is it the fastest language? No. Is it plenty fast? Yes.
There is obviously some use cases where Ruby won't be the best candidate, as the programmer you need to know what tool to pick for the job. Ruby can get you pretty far though and in my experience if your program is too slow that usually means you did something wrong.

## Conclusion

Am I biased? Probably. However I hope I helped you see a few of the reasons I think Ruby would be a good first language to learn programming. Once you learn some of the basic concepts of scripting and maybe [Object Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming) changing languages is just a matter of learning the syntax. I'm pretty sure you will have a good time if you decide to learn how to code in Ruby.
