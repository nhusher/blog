---

title: Son of GOTO

kerned_title: Son of <code class="k2-1">GOTO</code>

layout: post

---

In 1968, Edsger Dijkstra wrote a piece for *Communications of the ACM*, an academic magazine for computer scientists. This piece was published under the now-infamous title, "Go To Statement Considered Harmful."<sup>[1]</sup> Because of this essay, "considered harmful" has become a shorthand for announcing that a certain feature of a language or tool should be ignored, usually for stupid reasons. It became so profligate that [Eric Meyer](https://twitter.com/meyerweb) wrote a piece denouncing the practice&mdash;titled, of course, "[Conisidered Harmful Essays Considered Harmful](http://meyerweb.com/eric/comment/chech.html):"

> Typically, “considered harmful” essays gets written because someone has an axe to grind, and they feel like making that grinding process both public and dogmatic... Usually such “considered harmful” essays are intended to draw attention to a little-known subject about which the author is passionate, or to highlight what the author feels to be a poor decision by someone else.

There is something that Meyer doesn't bring up, but I think is a safe bet: Most developers who write "considered harmful" pieces have never read the original progenitor of the meme. I read it for the first time in preparation for writing this piece, I recommend you do do to&mdash;it's only [two pages](http://bioinfo.uib.es/~joemiro/teach/material/escritura/gotoharmfulCol.pdf).

Once we get past the title, what was Dijkstra actually getting at? I believe that the heart of it is this paragraph:

> Our intellectual powers are geared to master static relations and that our powers to visualize processes evolving in time are relatively poorly developed. For that reason we should do our utmost to shorten the conceptual gap between the static program and the dynamic process, to make the correspondence between the program (spread out in text space) and the process (spread out in time) as trivial as possible.<sup>[2]</sup>

Basically, our brains are bad at imagining state transition, so we should attempt to minimize these transitions and make them more obvious. I'm going to take this idea and run with it a little bit, and maybe convince you-the-reader of some of my weird ideas along the way.

----

One of my favorite things in modern javascript are the inclusion of the three legs of the functional programming tripod: `map`, `filter`, and `reduce`. These functions are also key to the [underscore.js](http://underscorejs.org/) library, and the foundation of an extremely rich set of tools offered by [clojure](http://clojure.org/sequences).



They've rapidly become my fa

I hear a lot of resistance to these constructs coming from people who aren't comfortable with them. At the same time, I think understanding and using these three functions is key for writing better, more composable, more testable software.



* one of my favorite things in javascript are the functions map, filter, and reduce
* resistance to map reduce because for loops are easy and good enough
* complecting tracking the size of a collection, iterating over it, with manipulating it -- COMPLECT
* it also composes well -- COMPOSITION


<ol class="footnotes">
  <li id="f2-1">Dijkstra originally titled the piece "A Case against the GO TO Statement." It acquired the more-inflamatory title during the publication process.</li>
  <li id="f2-2">Content edited slightly for conciseness.</li>
</ol>

[1]: #f2-1
[2]: #f2-2
