---
layout: post
title: "Exercism \U0001F47E: Part I"
---
# Exercism
As a developer, it's essential to keep learning new concepts and techniques to improve our skills. One way to do that is by practicing on coding exercises, such as the ones provided by [Exercism](https://exercism.org). In this article, we will go through some of the challenges that I have encountered and the solutions that I came up with for a few of the exercises on the platform.
## Assembly Line
Welcome to the AssemblyLine!

At first, I had implemented the following hash to store the success rates based on different speed ranges:
```Ruby
SUCCESS_RATE = { 1 => 1..4,
                   0.90 => 5..8,
                   0.80 => 9..9,
                   0.77 => 10..10 }.freeze
```
and then created a method to find the appropriate success rate based on the current speed:
```Ruby
def calculate_rate
    SUCCESS_RATE.select { |_, rate| rate.include?(@speed) }.first[0]
end
```
However, I later realized that there could be a more efficient way to achieve this.

I continued to research and found that using a switch case statement could improve the readability and maintainability of the code. So, I rewrote the success rate method to use a switch case statement, like this:
```Ruby
def success_rate
  case @custom_speed
  when 1..4
    1.0
  when 5..8
    0.90
  when 9..9
    0.80
  when 10..10
    0.77
  end
end
```
I also found that using the find method could return the first match in the form of key-value pair, like this:
```Ruby
def success_rate
  SUCCESS_RATE.find { |_, range| range.include?(@speed) }.first
end
```
I am also interested in the functional programming approach and have been impressed by its ability to solve problems. Although I am not yet ready to implement it, I believe it could be a valuable addition to my skillset.

Here's an example of how I might use functional programming to calculate the production rate and items per minute:
```Ruby
class AssemblyLine
  def initialize(speed)
    @speed = speed
  end

  def production_rate
    success_rate = lambda { |speed|
      case speed
      when 1..4 then 1
      when 5..8 then 0.9
      when 9..9 then 0.8
      when 10..10 then 0.77
      end
    }
    rate = ->(speed) { speed * 221 }
    rate.call(@speed) * success_rate.call(@speed)
  end

  def items_per_minute
    items = ->(rate) { (rate / 60).to_i }
    items.call(production_rate)
  end
end
```
I believe that functional programming is an art form and I am looking forward to learning more about it and implementing it in my work.
## Acronym
In this exercise we need extract the first character of a string. One of the most straightforward ways to accomplish this is by using the String#first method. However, I discovered that this method is only available in the Active Support library, which is not always included in projects or platforms, Which is the case on Exercism

This prompted me to search for other methods to return the first character of a string. I looked into passing arguments to the (&:slice) or (&:[]) methods, but found that these methods required a more complex implementation.

One option was to use a lambda function, like this:
```Ruby
FIRST_CHAR = ->(word) { word[0] }
def self.abbreviate(string)
  words(string).map(&FIRST_CHAR).join.upcase
end
```
This code uses a lambda function, which is assigned to the `FIRST_CHAR` constant. The lambda function takes a word as an argument and returns the first character of that word using the array index notation. The abbreviate method uses the map method to apply the `FIRST_CHAR` function to each word in the string, and then uses the join method to concatenate the first characters of each word, and the upcase method to convert the result to uppercase.

However, I eventually found a more efficient solution using the String#chr method, like this:
```Ruby
def abbreviate(phrase)
  phrase.chr.upcase
end
```

This code uses the `String#chr` method to return the first character of a string, and the upcase method to convert the result to uppercase.

Alternatively, I found a more intelligent and concise solution using the `String#scan` and join methods, like this:
```Ruby
def abbreviate(phrase)
  phrase.scan(/\b\w/).join.upcase
end
```
This code uses the `String#scan` to scan the string for word characters, then the `Array#join` to join them together in a single string, and the `String#upcase` to convert the result to uppercase.

In conclusion, there are multiple ways to return the first character of a string in Ruby, and each approach has its own set of advantages and disadvantages. It's important to consider your specific requirements and constraints when choosing a method for your project. In my case, I found that using the `String#chr` method was the most efficient solution, but I regret not discovering it earlier in my search.
## Matrix
First, I needed to detect the rows in the matrix. I wrote the following code to accomplish this:
```Ruby
@rows = matrix.split(/\n/).map(&:split).map { |row| row.map(&:to_i) }
```
I later found a more elegant solution in the community solutions:
```Ruby

@rows = text.lines.map { |l| l.split.map(&:to_i) }
```
Both of these methods achieve the same thing, but the latter is arguably more readable.

Next, I needed to find a way to transpose the matrix (i.e. turn the rows into columns and vice versa). I wrote the following method:
```Ruby
def transpose
  result = []
  (0...self[0].size).each do |col|
    new_row = (0...size).map { |row| self[row][col] }
    result << new_row
  end
  result
end
```
However, I later discovered that there is a built-in method for this called `Array#transpose`. Using this method, I was able to simplify my code and make it more efficient. put everything together in the Matrix class:
```Ruby
class Matrix
  attr_reader :rows, :columns
  def initialize(matrix)
    @rows = matrix.split(/\n/).map(&:split).map { |row| row.map(&:to_i) }
    @columns = @rows.transpose
  end
end
```
This class takes in a matrix as a string and separates it into rows and columns, which can then be accessed using the rows and columns methods. Overall, I found this exercise to be a great learning experience, and I hope that my solution can help others who are working on this exercise.
## Series
My initial solution was simple and easy for me to understand, but it was not as efficient or elegant as some of the community solutions. I found one solution in particular to be particularly impressive:
```Ruby
class Series
  def initialize(string)
    @string = string
  end

  def slices(n)
    raise ArgumentError if n > @string.length

    @string         # assuming n == 2 :
      .each_char    # %w(s t r i n g)
      .each_cons(n) # [%w(s t), %w(t r), %w(r i), %w(i n), %w(n g)]
      .map(&:join)  # %w(st tr ri in ng)
  end
end
```
This solution was impressive because it only uses built-in methods, has meaningful comments, and uses one error-raising condition instead of three (which was my solution). I also learned that the `Enumerable#each_cons` method is an Enumerable method, not an Array or String method. It calls the block with each successive overlapped n-tuple of elements.

Here's my basic solution using basic iterations over an array:
```Ruby
class Series
  def slices(series, result = [])
    raise ArgumentError if series > @digits.size || series < MIN_SERIES || @digits.empty?

    @digits.size.times do |i|
      break if (i + series) > @digits.size

      result << @digits[i..(i + series - 1)]
    end
    result
  end

  def initialize(input)
    @digits = input
  end

  MIN_SERIES = 1
end
```
## Luhn
I found this exercise to be quite challenging, despite it being labeled as an "easy" level. I struggled to fully understand the algorithm and had to spend a lot of time debugging and refactoring my code. One of the main issues I encountered was a small mistake in my code that caused several tests to fail.

Here's the initial version of my code:
```Ruby
digits = digits.chars.map.with_index do |char, index|
  if index.even?
    char.to_i
  else
    char = char.to_i * 2
    char -= 9 if char > 9
  end
end
```
The problem with this code is that I forgot to return a value from the last statement in the map block, which resulted in a lot of nil values. Specifically, I forgot to add char in the last line, so the Ruby interpreter evaluated it as a return statement.

I later came across a solution that uses a pipeline approach, which I found to be very elegant and concise. Here's the final version of my solution:
```Ruby
def self.valid?(input)
  input
    .gsub(/\s/, '')
    .tap { |s| return false unless s[/\A\d\d+\z/] }
    .chars
    .reverse
    .map.with_index { |d, i| i.odd? ? d.to_i * 2 : d.to_i }
    .map { |d| d > 9 ? d - 9 : d }
    .sum % 10 == 0
end
```
This version of the solution uses a pipeline of method calls to process the input and check for its validity. It eliminates the need for guard clauses and makes the code more readable.
## Clock
I really enjoyed this exercise as it was a great opportunity to practice object-oriented programming (OOP) and use some of Ruby's built-in features such as overloading unary operators and the Comparable class. I also used recursive methods to handle overflow scenarios. Here is my initial solution:
```Ruby
class Clock
  include Comparable
  attr_reader :minute, :hour

  MAX_HOUR = 24
  MAX_MINUTES = 60
  TWO_DIGIT = 10

  def initialize(hour: 0, minute: 0)
    @minute = minute_rolls(minute)
    @hour = hours_rolls(hour)
  end

  def minute_rolls(minute, over_hour = 0)
    if minute.negative?
      over_hour -= 1
      minute += MAX_MINUTES
      return minute_rolls(minute, over_hour) # recursion
    elsif minute >= MAX_MINUTES
      over_hour += 1
      minute -= MAX_MINUTES
      return minute_rolls(minute, over_hour) # recursion
    end

    @over_hours = over_hour
    minute_format(minute)
  end

  def hours_rolls(hour)
    hour += @over_hours
    @over_hours = 0 # After adding the over hours to the total hours we must empty it, in case of recursion

    if hour.negative?
      hour += MAX_HOUR
      return hours_rolls(hour) # recursion
    elsif hour > MAX_HOUR
      hour %= MAX_HOUR
    elsif hour == MAX_HOUR
      hour = 0
    end

    hour_format(hour)
  end

  def hour_format(hour)
    return "0#{hour}" if hour < TWO_DIGIT

    hour.to_s
  end

  def minute_format(minute)
    return "0#{minute}" if minute < TWO_DIGIT

    minute.to_s
  end

  def to_s
    "#{@hour}:#{@minute}"
  end

  def +(other)
    minute = @minute.to_i + other.minute.to_i
    hour = @hour.to_i + other.hour.to_i
    Clock.new(hour: hour, minute: minute)
  end

  def -(other)
    minute = @minute.to_i - other.minute.to_i
    hour = @hour.to_i - other.hour.to_i
    Clock.new(hour: hour, minute: minute)
  end

  def <=>(other)
    if @hour > other.hour
      1
    elsif @hour < other.hour
      -1
    elsif @minute > other.minute
      1
    elsif @minute < other.minute
      -1
    else
      0
    end
  end
end
```
I realized that my solution is a bit "concretely abstract", which is not the most efficient or elegant way to implement it. I came across another solution that uses basic mathematical calculations and is much more concise and elegant:
```Ruby
class Clock
  attr_reader :minutes

  TIME_FORMAT = '%02i:%02i'
  HOURS_PER_DAY = 24
  MINUTES_PER_HOUR = 60
  MINUTES_PER_DAY = HOURS_PER_DAY * MINUTES_PER_HOUR

  def initialize(hour: 0, minute: 0)
    @minutes = ((hour * MINUTES_PER_HOUR) + minute) % MINUTES_PER_DAY
  end

  def +(other)
    self.class.new(minute: minutes + other.minutes)
  end

  def -(other)
    self.class.new(minute: minutes - other.minutes)
  end

  def ==(other)
    minutes == other.minutes
  end

  def to_s
    TIME_FORMAT % minutes.divmod(MINUTES_PER_HOUR)
  end
end
```
This one uses basic mathematics calculation -which I'm awful at- to get the overall minutes, Then when `to_s` sent to the object, Object will do another mathematics calculation but, This time using `divmod` -Just knew about it!-.

For example, let's say we have the following code:
```Ruby
minutes = 125
hours, minutes = minutes.divmod(60)
```
Here, divmod(60) will divide minutes by 60 and return the result in an array: [2,5]. This is equivalent to 2 being the quotient and 5 being the remainder. And then, we can unpack the array using multiple assignment, so hours will have 2 and minutes will have 5
So In the line of code above, it is used to divide the minutes by `MINUTES_PER_HOUR` constant, which probably is 60, it returns an array with two elements, the quotient and the remainder, which can be used to format the string with hours and minutes in *hh:mm* format.
