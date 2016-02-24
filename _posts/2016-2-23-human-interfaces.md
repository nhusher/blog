---

title: Human interfaces

layout: post

---

Most of my job is building interfaces for humans to interact with a complex data model. A lot of the time, that's laying out buttons inside a window, showing the right number or text at the right time, or simplifying a firehose of data down to a nice visualization.

User interfaces aren't just buttons and boxes, though. A user interface is a way to make a complicated thing simpler to manipulate. The way an idea is described also a user interface for thinking about it.

The more incidental complexity it takes to describe an idea, the more difficult it is to manipulate the idea in your head. Here's a fairly simple problem expressed by a great Persian mathematician al-Khwārizmī from the tenth century:<sup>[1]</sup>

> What is the square which when taken with ten of its roots will give a sum of thirty nine? Now the roots in the problem before us are ten. Therefore take five, which multiplied by itself gives twenty five, and amount you add to thirty nine to give sixty four. Having taken the square root of this which is eight, subtract from this half the roots, five leaving three. The number three represents one root of this square, which itself, of course, is nine. Nine therefore gives the square.

The above passage is extremely difficult to understand. A senior in high school might now represent this problem and its solution as:

    x^2 + 10x = 39
    x^2 - 3x + 13x - 39 = 0
    (x-3) * (x+13) = 0
    x = {3,-13}

(al-Khwārizmī missed the second solution of -13)

There are advantages to both "interfaces" of this math problem: The natural language version requires no special knowledge of any special notation. A person with no knowledge of mathematics, but a knowledge of language, could work their way through this problem with a dictionary.

The compact mathematical notation has significantly more advantages. Once you get past the learning curve of the concise notation, it becomes much easier to symbolically manipulate the problem. Not only is it easier to solve, it's easy to apply the same solution to many possible problems.

It's also possible to build higher-order concepts on top of a concise notation. Calculus is a natural byproduct of concise mathematical notation; it was discovered concurrently by both Liebniz and Newton about 15 after the vocabulary of mathematic notation had settled.<sup>[1]</sup>

The right abstraction/interface can produce higher-order insights. Fabricating the right kind of interfaces is critical to further innovations. This can be at the expense of easy learnability, which is a problem I don't know how to solve (or if it even needs to be solved).

<ol class="footnotes">
  <li id="f2-1">
    This example is lifted from Bret Victor's lecture, <em><a href="http://worrydream.com/MediaForThinkingTheUnthinkable/">Media for Thinking the Unthinkable</a></em>. It's an excellent talk on the nature of problem solving and building new affordances for our weak monkey brains.
  </li>
  <li id="f2-2">
    By "settled," I mean that the full set of symbols &equals;, &plus;, &minus;, &divide;, &times;, as well as notations for exponents, roots, grouping, and so forth. The division symbol was the last to be introduced in 1659, according to <a href="http://en.wikipedia.org/wiki/Table_of_mathematical_symbols_by_introduction_date">this awesome table I found on Wikipedia</a>. These operations existed for centuries before their symbols were commonly used. I don't think it's not a great leap to assume that innovations in notation drove the discovery of calculus, and not the other way around. 
  </li>
</ol>


[1]: #f2-1
[2]: #f2-2
