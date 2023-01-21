---
title: 'Ruby''s new built-in class: Set ✨'
categories:
- Ruby
tags:
- built-in
- method
- library
- ruby
- function
- standard
- set
- hash
- array
- tutorial
- learn
image:
  path: /assets/preview.jpg
  alt: Photo by Joshua Fuller on Unsplash
date: 2023-01-21 12:07 +0400
---
# Ruby's new built-in class: Set.
Since the new release of Ruby v3.2.0 [Feature #16989](https://bugs.ruby-lang.org/issues/16989) It can be accessed via the Set constant or by calling `Enumerable#to_set` as a core ruby class from the core library, instead of standard library. So I thought its a good time to write about it.
In this article, we'll dive into the Set class and all it has to offer. Get ready to explore the power of sets in Ruby!
## What's Set btw?
A Set is a collection of objects that cannot contain any duplicates. The objects in a set can be of any type, such as strings, integers, arrays, or other sets. The uniqueness of the objects in a set is maintained through the use of a hash function, which assigns a unique key to each object in the set. This means that if an object is added to a set and an identical object already exists in the set, the new object will not be added and the set will remain unique.
> info: Set is now available as a built-in class in Ruby
{: .prompt-info }
```Ruby
require "set" # no longer required
```
## Creating a Set
In Ruby, the Set class provides a collection of unique elements. To create a set, you can use the `Set.new` constructor. This constructor takes an optional argument, which can be a collection object such as an _array_ or a _hash_. The objects in the collection will be placed individually in the set.

Alternatively, you can provide a code block to the constructor. In this case, each object in the collection will be passed through the block, and the resulting value will be added to the set.

Here are some examples of creating sets in Ruby:
```Ruby
# Create an empty set
empty_set = Set.new

# Create a set with the elements in an array of symbols
> countries = %i[ksa uae egypt]
> middle_east = Set.new(countries)
=> #<Set: {:ksa, :uae, :egypt}>

> asia = Set.new(%i[china japan thailand uae ksa])
=> #<Set: {:china, :japan, :thailand, :uae, :ksa}>

# Create a set using a code block to transform the elements
> names = %i[abdullah mahfouz saleh salem]
> name_set = Set.new(names) {|name| name.upcase }
=> #<Set: {:ABDULLAH, :MAHFOUZ, :SALEH, :SALEM}
```
## modifying a Set
To modify a set in Ruby, you can use the `<<` operator or the `Set#add` to add a single object to the set. If the object you are trying to add is already in the set or is content-equal to an object that is already in the set, it will not be added.

To remove an object from a set, you can use the `Set#delete`. If the object you are trying to remove is not in the set, it will not raise an error and nothing will happen.

You can also use the `Set#add?`, which returns `nil` if the set is unchanged after the operation.

Here is an example using the middle_east set:
```Ruby
> middle_east << :qatar
=> #<Set: {:ksa, :uae, :egypt, :qatar}>
> middle_east.add(:qatar) # same as "<<" operator
=> #<Set: {:ksa, :uae, :egypt, :qatar}> # unchanged
> middle_east.delete(:qatar)
=> #<Set: {:ksa, :uae, :egypt}>
> middle_east.add?(:ksa)
=> nil
```
## Set query
The `Set` class in Ruby is a collection of unique elements. You can perform various operations on sets, such as finding their intersection (elements that are present in both sets), union (elements that are present in either set), and difference (elements that are present in one set but not the other). These operations can be performed using the intersection method `&`, union method `+` or `|`, and difference method `-`.

For example, if you have two sets `middle_east` and `asia`, you can find their intersection by using the `&` operator or the intersection method:
```Ruby
> middle_east & asia # middle_east.intersection(asia)
=> #<Set: {:ksa, :uae}>
```
This will return a new set consisting of the elements that are present in both `middle_east` and `asia`.
To find the union of the two sets, you can use the `+` operator or the union method:
```Ruby
> middle_east + asia # middle_east.union(asia) || middle_east | asia
=> #<Set: {:ksa, :uae, :egypt, :china, :japan, :thailand}>
```
This will return a new set consisting of all elements that are present in either `middle_east` or `asia`.
To find the difference between the two sets, you can use the `-` operator or the difference method:
```Ruby
> middle_east - asia # middle_east.difference(asia)
=> #<Set: {:egypt}>
```
This will return a new set consisting of the elements that are present in `middle_east` but **not** in `asia`.

There is also an exclusive-or operator `^` which returns a set consisting of elements that are present in either `middle_east` or `asia`, **but not both**:
```Ruby
> middle_east ^ asia
=> #<Set: {:china, :japan, :thailand, :egypt}>
```
## Merging
It's important to note that these set operations do not modify the original sets. They return new sets with the resulting elements.

When you merge a hash into a set in Ruby, the resulting set will contain two-element, _key/value_ arrays based on the elements in the hash. This is because when you iterate through a hash, it breaks itself down into an array of two-element arrays, where each array represents a _key/value_ pair in the hash.

Here's an example that demonstrates this behavior:
```Ruby
middle_east.merge({ kuwait: :kuwait, iraq: :baghdad })
# => #<Set: {:ksa, :uae, :egypt, [:kuwait, :kuwait], [:iraq, :baghdad]}>
```
If you provide a hash as an argument to the `Set#new` constructor, the resulting set will also contain two-element arrays based on the elements in the hash.

Sometimes you may want to merge just the keys of a hash into a set, rather than the entire hash. This can be useful because set membership is based on the uniqueness of the keys in a hash. To do this, you can use the `Hash#keys` on the hash, and then pass the resulting array to the `Set#merge`.
Here's an example:
```Ruby
my_hash = { kuwait: :kuwait, iraq: :baghdad }
# => {:kuwait=>:kuwait, :iraq=>:baghdad}
middle_east.merge(my_hash.keys)
# => #<Set: {:ksa, :uae, :egypt, :kuwait, :iraq}
```
You can try out different permutations of set merging to see how it works, as long as the argument you pass to the merge method is an object with an `#each` or `#each_entry` method.
## Superset and Subset
Set objects have the ability to test for subset and superset relationships between sets. To do this, you can use the `Set#subset?` and `Set#superset?`, respectively. It's important to note that the arguments to these methods must be sets, not _arrays_, _hashes_, or any other kind of _enumerable_ or _collection_.
Here's an example of how you can use these methods:
```Ruby
middle_east = Set.new(%i[uae ksa])
# => #<Set: {:uae, :ksa}>
asia = Set.new(%i[china japan thailand uae ksa])
# => #<Set: {:china, :japan, :thailand, :uae, :ksa}>

asia.superset?(middle_east)
# => true
middle_east.subset?(asia)
# => true
```
In addition to subset and superset, the `Set` class also provides the `Set#proper_subset?` and `Set#proper_superset?`. These methods filter out the case where a set is a subset or superset of itself, because all sets are both subsets and super-sets of themselves.

A proper subset is a subset that is smaller than the parent set by at least one element. If the two sets are equal, they are subsets of each other, but they are not considered proper subsets.

A proper superset of a set is a second set that contains all the elements of the first set, as well as at least one element that is not present in the first set.

Here's an example of how you can use these methods:
```Ruby
set1 = Set.new(%i[a b c])
# => #<Set: {:a, :b, :c}>
set2 = Set.new(%i[a b])
# => #<Set: {:a, :b}>
set3 = Set.new(%i[a b c d])
# => #<Set: {:a, :b, :c, :d}>

set1.proper_subset?(set2)
# => false
set1.proper_subset?(set3)
# => true
set3.proper_superset?(set1)
# => true
set3.proper_superset?(set2)
# => false
```
You're set! Reach out if you have any questions.
> I highly recommend "The Well-Grounded Rubyist"[^1] to any Ruby developer looking to take their skills to the next level!
{: .prompt-info }
[^1]: [Amazon](https://www.amazon.com/Well-Grounded-Rubyist-David-Black/dp/1617295213)
