Ruby
====

This guide will focus on Ruby - the language - alone.

After finishing this guide you will

* have an overview of Ruby's type system
* be able to use list processing functions in Ruby
* be able to use blocks and `yield` in Ruby.

REPO: Fork the [learn_ruby](https://github.com/backend-development/learn_ruby) repository to try some test driven ruby learning


-----------------------------------------------------------------------

What is Ruby
------------

![Ruby Logo](images/Ruby_logo.png)

Ruby is an open source project.  It was started
in 1996 by Yukihiro 'Matz' Matsumoto. He is still
the "benevolent dictator" who decides on the future of the language.

In a colossal break with tradition he did not
choose a name starting with p for his scripting language
(think perl, python, php) but opted for r instead.

Ruby is a thoroughly object oriented scripting language.
Even basic data types are object:

``` ruby
1.to_s
=> "1"
```

In this example the number 1 is used as an object, the method `to_s` is
called on it.  The result is an Object of class String.

### What is Ruby on Rails

![Ruby on Rails Logo](images/Ruby_on_Rails_logo.png)

Ruby on Rails is a web framework written in Ruby.
It was created by David Heinemeier Hansson ('DHH') starting in 2005.

Rails is famous for the high productivity it gives to developers. It
is often used in startups, where speed of delivery is very important.
Rails moves fast, new versions with major improvement appear about every 18
months.  The Rails community values speed of development, DRY code, testing,
version control, ... when you learn Rails you also pick up a whole culture
surrounding it.

Some sites built with Rails are: [GitHub](https://github.com) and [GitLab](https://gitlab.com), [Shopify](https://shopify.com), [Airbnb](https://airbnb.com), [Twitch](https://www.twitch.tv/), [SoundCloud](https://soundcloud.com), [Square](https://squareup.com/) and of course [Basecamp](https://basecamp.com), DHHs own product.

### Why Ruby? Why Rails ?

Why should you use Ruby and Rails over other programming languages
and frameworks?  

* Because you want to be a highly productive web developer?
* Because you want to learn from the best?
* Because you want try out many different languages and many frameworks?

All these answers are equally valid.

Also, there are cool t-shirts:

![Ruby and Rails T-Shirts](images/ruby-and-rails-t-shirts.png)


Ruby Basics
-----------

To get to know Ruby you don't need to write
whole programes.  You can start out with **interactive ruby - irb**.
When you start irb you get a command line to type in Ruby code. When
you press enter the code is evaluated immediately and you get the result.
Use the commands `exit` or `quit` or the key combination CONTROL-D to get out.

In this guide we will show code run in irb by marking the prompt as `>>` and
the result as `=>`, for example:

``` ruby
>> 2 + 2
=> 4
```

### Some code conventions

Try to stick to [github's style for ruby](https://github.com/styleguide/ruby). 

When you choose names for your objects, classes and methods
you should stick to the following conventions to avoid
confusing other Ruby developers:

``` ruby
the_variable = SomeClass.new
             # variables are written in snake_case
             # classes in capital CamelCase

             # method names are written in snake_case
a = b.sugar  # a method that returns something
b.sweet?     # a method that returns true or false
             # ends in a question mark
b.sugar!     # a method that changes its object
             # ends in an exclamation mark
```

In the last two examples the punctuation marks are really
part of the method names!


The parantheses around the arguments of a method are optional.
Leave them off unless your code get's confusing:

``` ruby
puts("less code")
puts "less code"
```

# Data Types

All of Ruby's basic data types are Classes.  

* nil
* Numeric, Integer, Fixnum, Bignum, Float `# are converted automatically to each other`
* Ranges
* String
* true  `# TrueClass`
* false `# FalseClass`
* Symbol
* Array
* Hash
* Object
* Regex

Ruby is strict about data types, there is no automatic conversion except
between numeric types.

``` ruby
>> "a string" + "another"
=> "a stringanother"

>> "a string" + 2
TypeError: no implicit conversion of Fixnum into String

>> 42 + 3.141
=> 45.141
```

Everything is an Object, even Integers and Strings.
They have methods and properties, like other objects:

``` ruby
>> nil.class
=> NilClass

>> 2.class
=> Fixnum

>> "some text".length
=> 9
```

### Strings

``` ruby
s = 'just a string of characters'
s = "string with #{the_variable} embedded"
s = "string with #{a+b/c} a ruby expression embedded"
s = <<EOM
This is a so called "Here-Document"
it can contain many lines of text
and ends with the identifier EOM (that i chose!)
but only if it's alone on a line all by itself:
EOM
```

### Boolean Values

In Ruby only `false` and `nil` are treated as false. This might
be confusing for programmers used to other languages with
more complex rules for truthyness:

``` ruby
if 0
  puts '0 is true!'
end
if "false"
  puts '"false" (the string) is true'       
end
```

### Shorthand version of Conditions

The two conditions shown in the last
code block only have one statement inside
the block.  

This can also be written in another way by
appending the condition to the statement,
like so:

``` ruby
puts '0 is true!' if 0
puts '"false" (the string) is true' if "false"
```

This syntax should be familiar to you if
you understand English.  (yes, that's an English
sentence using the same syntax).

### Boolean Operators

When Ruby evaluates a boolean operator,
it does as little work as possible.  It
stops evaluation as soon as the result is clear:

``` ruby
# the second argument is not evaluated!
a =  true || ...
a = false && ...
```

The boolean operators don't just return true or
false, they return the argument last evaluated.
This is often used to set a variable:

``` ruby
default_value = "gray"
input_value = nil
# here input_value might be set...
a = input_value || default_value
```

## The 'Save Navigation' Operator

Imagine you have an Object a, that has a property b containing
another object, and b has a property c.  You can access c through `a.b.c`

But if a is `nil` then you will get an error:

``` ruby
> a.b.c
NoMethodError: undefined method `b' for nil:NilClass
```

The Operator `&.` avoids this error:

``` ruby
> a&.b&.c
nil
```

The Operator was introduced in Ruby 2.3.0 is is called
"save navigation operator" or sometimes "lonely operator".


### Symbols

A Symbol looks - at first glance - similar to a string: you can
invent it at any time (no 'declaration') and give it any name:

``` ruby
a = :foo
a = :bar

s = "foo"
s = "bar"
```

But: there is always just one instance of a symbol while
there can be several strings that have the same content, but are different objects:

``` ruby
>> :foo.object_id
=> 635528
>> :foo.object_id
=> 635528
>> "foo".object_id
=> 70099463087600
>> "foo".object_id
=> 70099463106400
```

Use symbols where you would enums in a database or another language,
or if you need distinct constants, when the value is not important.

### Methods

Methods in Ruby return the last expression - even
if no explicit `return` statement is given.

``` ruby
def f(a,b)
  "x"
end

f(1,42)  # returns "x"
```

### Arrays

There are several ways of writing literal arrays in Ruby.
The first one looks like JSON:

``` ruby
>> a = ["this", "that", "something"]
=> ["this", "that", "something"]
```

For creating an array of words (strings without whitespace in them)
you can use `%w`:

``` ruby
>> a = %w(this that something)
=> ["this", "that", "something"]
```

When creating an array of consecutive numbers you can
use a Range and convert it to an Array:

``` ruby
>> (1..4).to_a
=> [1, 2, 3, 4]
```






Hashes
----------------------------

A Hash is a datastructure similar to an array. An array uses integers as keys
while a Hash allows any type as the keys. Mostly strings and symbols are used:

``` ruby
h = Hash.new
h["alice"] = "beer"
h["chris"] = "tea"
h["bob"] = "mate"
```

But you can use other objects:

``` ruby
t = Date.new
h[t] = "recently"
```

The data structure behind a Ruby Hash is more complex
than an array: The key is sent through a function  (called hash function)
that returns a number. This number is used as the index
for an array.  If the hash function for two keys is the
same a linked list is built.


![How Hash(tabl)es work](images/hash_table.svg)

This datastructure seems like a serious waste of memory
at first. But it offers the following interesting features:

* looking up a key can be accomplished in constant time
* inserting a new key / value pair can be accomplished in constant time


Most scripting languages offer Hashes as a basic data type,
most compiled languages as a library.  Read more about
Hashes in Wikipedia:

* [Hashtables in Wikipedia](https://en.wikipedia.org/wiki/Hash_table)

(If you don't know what "in constant time" means above, you
should learn more about the analysis of algorithms. e.g. by
taking an algorithms and data structure course as offered in the second
semester of most computer science programs.

### Implicit Form

A Hash can be created by using its "implicit form":

``` ruby
roomnumber = { "Jane Doe" => 10, "Jim Doe" => 6 }
```

Since Ruby 1.9.3 Hashes allow an alternate syntax when the keys are symbols. Instead of

``` ruby
style = { :font_size => 10, :font_family => "Arial" }
```

you could write this  in  a JSON-like way:

``` ruby
style = { font_size: 10, font_family: "Arial" }
```

This style of hash is often used as an argument for a method.
Calling the method then reads like named arguments:

``` ruby
def apply_the_style( h )
  ...
end

apply_the_style(font_size: 10, font_family: "Arial")
apply_the_style font_size: 10, font_family: "Arial"
```



Enumerables and Piping Data
----------------------------

When working with a list of values Ruby
helps you think about data on a new, more abstract level
with Enumerables:

### "Piping Data"

From the UNIX shell you may know the concept of piping data
from one command to the next:

``` shell
# many httpd processes are running. as which user(s) ?
#
$ ps aux | grep httpd | cut -c1-8 | sort | uniq
```

Each of those programs reads data from "Standard Input" and
writes data to "Standard Output".  The vertical bar symbol (called "pipe") takes
the output of the preceding program and sends it input the next
program.  The data in question is plain text, consisting
of several lines.

Try it out on your commmand line by building up
the pipe step by step:

``` shell
$ ps aux | less
$ ps aux | grep httpd | less
$ ps aux | grep httpd | cut -c1-8 | less
$ ps aux | grep httpd | cut -c1-8 | sort | less
$ ps aux | grep httpd | cut -c1-8 | sort | uniq | less
```

### Piping Data in Ruby

When piping data in ruby you can start with
an Array

``` ruby
languages = %w[Fortran Ada C C++ Java Scala Haskell]
languages.sort.first(3)
```

The elements of the array are piped into the sort-method, which again outputs a list
of elements. These are piped into first, which only returns the first three and discards
the rest. The result is a list of 3 elements.

Here are some simple methods you can use on Arrays (and other Enumerables)
that return a new Enumerable.  You can connect theses methods to each other:

* sort
* first(n)
* drop(n)
* last(n)
* grep(/pattern/)
* chunk(method)
* reverse

Some other methods return just a single value, and thus end the pipe:

* count
* count("only this exact value")
* max
* min

More advanced methods take a Block (of code) as their argument.
The method `map` applys the Block to each piece of data, and
returns an Enumerable of the new data:

``` ruby
>> (1..10).map{ |x| 2*x }
=> [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

>> (1..10).map{ |x| 2*x }.reverse
=> [20, 18, 16, 14, 12, 10, 8, 6, 4, 2]
```

If the computation is more complex you can write
the Block on several lines, ending with `end`

``` ruby
>> (1..10).map do |x|
?>   x*2
>> end
=> [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

>> (1..10).map do |x|
?>   x*2
>> end.reverse
=> [20, 18, 16, 14, 12, 10, 8, 6, 4, 2]
```


Some other methods for Enumerables that take a Block:

* `map {|x|` new value computed from x `}`
* `select {|x|` should x be selected? `}`
* `reduce(:+)`
* `reduce{|memo, item|` compute new value for memo, using current item `}`


These methods should help you avoid loops and thus simplify
code considerably.

Read the Reference on

* [Enumerable](https://ruby-doc.org/core-2.5.3/Enumerable.html)
* [Array](https://ruby-doc.org/core-2.5.3/Array.html)


Blocks
------

Blocks of code are not just used in Enumerables, they are
a basic building block of ruby.  You can write functions
that take a Block as an argument:

### My Function takes a Block of Code

Any function you write can take an addictional block
of code as its last argument.  The block is only
called if and when you call `yield` inside the function:

``` ruby
def my_function_with_block_arg
  puts "code in the funtion"
  yield
  puts "more code in the function"
end

my_function_with_block_arg { puts "code in the block" }

# OUTPUT:
# code in the funtion
# code in the block
# more code in the function
```

There is an alternate syntax for calling the function: instead
of the curly braces you can use `do` and  `end`:

``` ruby
my_function_with_block_arg do
  puts "code in the block"
  puts "more code in the block"
end
```


Summary
-------

You now know about the basic data types, about enumerables and about blocks -   
features that distinguish Ruby from other scripting languages. 


If you want to get more pracitical with Ruby, you can
do the 
[Learn Ruby](https://github.com/backend-development/learn_ruby) test driven Ruby exercises.


This should be a good enough basis to start with Rails.  
But do take every
oppertunity you get to learn more about Ruby itself: if you are unsure about
a line of code, look it up in the Ruby documentation and use the
opportunity to read a bit more than strictly necessary.

Get an offline version of the documentation
installed on your development machine so you can look up stuff instantly.

* Mac Tool [Dash](https://kapeli.com/dash) for keeping offline copies of documentation

### Online Resources

* [try ruby](https://ruby.github.io/TryRuby/)
* [learningruby.com tutorial](http://rubylearning.com/satishtalim/tutorial.html)

### Books

* Flanagan, Matsumoto(2008): [The Ruby Programming Language](https://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177). O'Reilly. ISBN 0596516177.
* Thomas(2013): [Programming Ruby 1.9 + 2.0](https://www.amazon.com/Programming-Ruby-1-9-2-0-Programmers/dp/1937785491/). Pragmatic Porgrammers. ISBN 1937785491
* Olsen (2011): [Eloquent Ruby](https://www.amazon.com/Eloquent-Ruby-Addison-Wesley-Professional/dp/0321584104/). Addison-Wesley. ISBN 0321584104
* Tate(2010): [Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages](https://www.amazon.com/Seven-Languages-Weeks-Programming-Programmers/dp/193435659X/). Pragmatic Programmers.  ISBN 193435659X
