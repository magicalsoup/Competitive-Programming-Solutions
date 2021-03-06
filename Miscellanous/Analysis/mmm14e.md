The first step is obviously to parse the input. There are various ways to do this. Here is one straightforward way: First, 
keep reading characters until a non-whitespace character is found. If it's a quotation mark, then this begins a literal token; 
keep reading until the next quotation mark is found (to close the literal). Otherwise, the next token is simply the entire word 
(that is, all characters up to but not including the next whitespace or the end of file). 
If a token is not a literal then examine its first character; if it's an angle bracket then this token is a symbol; 
if it's a colon then this is the ```::= sign```; if it's a vertical bar then this token separates expressions in a union; 
and likewise check also for braces and square brackets.

Having tokenized the input, we can split the token sequence into a list of definitions. Each token to the left of a ::= sign is the 
beginning of a definition. We can also replace the symbol ```<EOL>``` with a single-character literal (this just simplifies things a bit) and
replace each token by a numerical ID. Take note of the ID of the <essay> token. For a given symbol, split the definition into individual
expressions that are unioned together (i.e., split on the | character). 
In each expression, simply discard any optional sequence of tokens 
(that is, sequences enclosed in braces and square brackets) since an optimal solution will never have nonempty optional sequences. 
For each expression, count the total number of characters in all literals and the number of symbols of each type (ID) that occur in the 
expression; these are the only details that matter from here on in. For example, if ```<foo>``` has been assigned the ID 0, <bar> has been 
assigned the ID 1, and ```<baz>``` has been assigned the ID 2, then the definition

```
<foo> ::= <bar> " " <bar> | "." <baz> "." { <baz> }
```
basically becomes something like
```
<0> ::= 2<1> + 1 | <2> + 2
```

What this really represents, of course, is the statement ```L(0) = min(2L(1) + 1, L(2) + 2)```, where L(i) 
denotes the minimum length of a string accepted by the symbol with ID i.

We proceed by an algorithm that combines characteristics of Dijkstra's algorithm and topological sort. First we will visit the symbol i
such that L(i) is as small as possible, then we will visit the symbol j such that L(j) is the second smallest value possible, and so on.
If we visit the <essay> symbol, then we are done. If we run out of symbols to visit, then we give up and print -1.

To determine the first symbol to visit, 
notice that it must be a symbol one of whose definition alternatives consists purely of literals.
Otherwise there is no base case for the language. If all of a symbol's definition alternatives contain non-literals, 
then any string accepted by that symbol must include (directly or indirectly) some symbol that has a definition alternative that does
consist purely of literals; therefore it cannot be strictly the shortest possible string. For example, in Sample Input 2, 
the symbols <noun> through ```<article>``` are candidates to be visited first; all other symbols'
definitions all have at least one non-literal. Of course, the actual first symbol to visit is the one that has the shortest sequence of 
literals; in this case we can visit either 
<pronoun> first (1 character, "I") or ```<article>``` first (1 character, "a").

After a symbol has been visited, some other symbols may become newly eligible to be visited next, because (at this point) they now have 
at least one definition alternative that consists entirely of either literals or previously visited symbols. This is the part of the 
algorithm that is similar to topological sort: symbols have dependencies on other symbols and a symbol can only be visited once all the
dependencies in one of its definition alternatives have been visited. Thus, we should keep track of all definition alternatives that use
a given symbol, so that after a symbol is visited, we can quickly update any definition alternatives that depend on it and determine 
whether any are eligible to be visited next (in the same way that, in topological sort, we keep track of edges leaving a vertex, and 
after visiting that vertex, we decrement the incoming edge count of the other endpoint of each such edge).
Once a definition alternative has had all its symbols visited, we can compute the minimum length of a string that would be accepted by 
that definition alternative. (This is the same as saying, for example, 
that once L(1) becomes known, we can compute the value of 2L(1) + 1.) So we add this minimum length to the heap. 
Once we've added as many definition alternatives to the heap as possible without visiting another symbol, we take the smallest length 
from the heap, and visit the corresponding symbol.

Last but not least, this question requires the use of bignums inorder to store the result, so either use the already built-in classes
like in java, and python, or create your own in c++ and other languages that don't support bignums
