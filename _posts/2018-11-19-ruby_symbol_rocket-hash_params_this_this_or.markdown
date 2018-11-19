---
layout: post
title:      "Ruby: Symbol, Rocket-Hash, params...  (:this, this:, or =>)??"
date:       2018-11-19 15:10:36 -0500
permalink:  ruby_symbol_rocket-hash_params_this_this_or
---



Months ago, I remember this issue first confronting me, and was fairly stymied, when do you put the colon before, when after, and why?  I tried looking around for answers and couldn't find the information I was looking for, likely because I didn't know what exactly I was asking.  They are all symbols, right? But some of the time one format worked, and another format didn't, but I had no idea why?  I tried looking around for the answer, but when I finally got it to work, and was unable to find an answer online after some genuine effort, I gave it up as a lost cause, if it works, why bother, right?  But this is something I seemed to run into again and again, so I eventually decided I should do some digging and investigate.  What was I trying to figure out?

The problem was I did not know what I was trying to create, and I was conflating a bunch of separate concepts.  I knew I was trying to pass information to a function, but not whether I was trying to pass in symbols, or use keyword arguments, or hashes, or some combination of all of these things.

**Symbol**

2 important things about symbols.  They are 1) unique and 2) constant throughout your program.  A symbol is the most basic Ruby object that you can create.  It is just the name of the symbol, and an internal identifier.  The internal idenfier, is a unique object_id so it is not necessary to pre-declare or assign it a value (unlike a variable).  In terms of format, a symbol looks like this:

```
:name
```
-or-
```
:symbol
```
-or-
```
:thing
```

It's just a variable, prefixed with a colon.

Symbols are particularly useful when creating hashes.

**Hashes**

Hashes are similar to arrays in that they are an indexed collection of object references.  They are also described as associative arrays, maps, or dictionaries.  In a hash there are two related objects, an index (or key) and a value.  You are able to use one thing (the key or index) to look up the other.  The values in a hash can be any type of object.  **Symbols** are commonly used as hash keys (*but the key does not have to be a symbol*).  And they can be represented in two different ways.


```
dog = {:name => 'Kenobi', :breed => 'Shiba Inu', :owner => 'Monica'}
```
With a :symbol, a hash rocket (=>), and then a value (here strings), separated by commas.  
*(This was the only way that worked in earlier versions of Ruby.)*

-or-

```
dog = {name: 'Kenobi', breed: 'Shiba Inu', owner: 'Monica'} 
```
This is the shortened version, that creates symbol keys, and yields the exact same output.  However, this format will **only** work when you are trying to create a hash where the keys are symbols.  

If the key is a number, or a string, the hash rocket formatting is required.
```
{ 1 => "uno", 2 => "dos", 3 => "tres" }
```
```
{ "name" => "Kenobi", "breed" => "Shiba Inu", "owners" => ["Monica", "Jacob"] }
```

To look up the values within a hash you would provide the name of the hash, and then put the key (in this case a symbol) in brackets.

```
dog[:name]
```
(if the key is not a symbol it would be something like this - `dog["name"] `)

In a similar fashion you can assign additional values to a hash, or re-assign values.
```
dog[:age] = 7
```
```
dog[:coloring] = "black and tan"
```
-or-
```
dog[:owner] = "Jacob"
```


**Class Objects**

Classes are blueprints (or recipes, instructions, etc.) for the creation of objects.  An object can be *instantiated* by a Class, and will inherit the methods specific to that class.  (The Object is an instance of the Class).  While class objects are not a hash, there are some similarities that can be confusing.  Class methods are output as **Symbols**.

```
Dog.methods
=> [:name, :name=, :breed, :breed=, :owner_id, :owner_id=, :age,  :age=, etc.]
```
Yep, the method names are represented as Symbols.

This concept may also be familiar from utilizing attr_accessor:
```
attr_accessor :name, :breed, :owner
```


**Keyword Arguments**

Keyword arguments are a special way of passing arguments into a method. They behave like hashes, pairing a key that functions as the argument name, with its value.  (keyword arguments, i.e., a hash of attributes)

Keyword arguments allow us to switch the order of the arguments, without affecting the behavior of the method

By using keyword arguments, we also know what the arguments mean without looking up the implementation of the called method
```
obvious_total(subtotal: 100, tax: 10, discount: 5)
```
With keyword arguments defined in the method signature itself, we can immediately discover the names of the arguments without having to read the body of the method.

Within our method body, we can reference the keyword argument as though it is a bareword even though it is a key in our argument hash. 

Implementing keyword arguments creates cleaner, clearer and more maintainable code by simplifying future refactors, explicitly stating what arguments should be passed in and outputting more meaningful argument errors.  All in all, it tends to make the code more robust and explicit.  Usually, the code clarity and maintainability gained from keyword arguments outweighs the brevity offered by positional arguments.


```
class Person
  attr_accessor :name, :age

  def initialize(name:, age:)
    @name = name
    @age = age
  end
end
```


Keyword arguments also enable us to use mass assignment to instantiate new objects. If a method is defined to accept keyword arguments, we can create the hash that the method is expecting to accept as an argument, set that hash equal to a variable, and simply pass that variable in to the method as an argument.  This is often accomplished by utilizing a params hash that is created from a form (such as in Sinatra, Rails, etc.).

```
params = {
  :name => "Fira",
  :breed => "boxer"
}
```

**Putting it all Together**

If you run into this issue, it's best to go through the above concepts and determine what you are working with, and then answer the below questions.

1. What kind of data does the function expect (does the function contain keyword arguments?)
2. What are the parameters being passed to the function?  (what is the source and what format is the data already in? is it a hash? are the keys symbols?)
3. Do the function arguments (what the function wants) and parameters (the data passed to the function) align?

