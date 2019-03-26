# CS500 design and analysis of algorithm

**Giải thuật sắp xếp Adaptive và Non-Adaptive**

Một giải thuật được xem như là adaptive, nếu nó tận dụng các phần tử đã được sắp xếp trong danh sách mà đã được sắp xếp. Đó là, trong khi sắp xếp nếu danh sách ban đầu có một số phần tử đã được sắp xếp, thì giải thuật dạng adaptive sẽ ghi nhận các phần tử này và sẽ cố gắng không thay đổi thứ tự của chúng.

Trái ngược với loại giải thuật trên, giải thuật dạng non-adaptive sẽ không ghi nhận các phần tử đã được sắp xếp trước đó. Giải thuật loại này sẽ vấn cố gắng sắp xếp lại từng phần tử trong danh sách ban đầu.

**Các khái niệm quan trọng trong giải thuật sắp xếp**

Dưới đây là phần giới thiệu ngắn gọn cho một số khái niệm xuất hiện trong khi thảo luận về các giải thuật sắp xếp:

`Thứ tự tăng`
Một dãy giá trị được xem như trong thứ tự tăng dần nếu phần tử đứng sau lớn hơn phần tử đứng trước. Ví dụ: 1, 3, 5, 6, 9.

`Thứ tự giảm`
Một dãy giá trị được xem như trong thứ tự giảm dần nếu phần tử đứng sau nhỏ hơn phần tử đứng trước. Ví dụ: 9, 6, 5, 3, 1.

`Thứ tự không tăng`
Một dãy giá trị được xem như trong thứ tự không tăng nếu phần tử đứng sau nhỏ hơn hoặc bằng phần tử đứng trước. Ví dụ: 9, 6, 5, 5, 1. Loại thứ tự này xuất hiện khi trong một dãy có chứa các giá trị giống nhau.

`Thứ tự không giảm`
Một dãy giá trị được xem như trong thứ tự không giảm nếu phần tử đứng sau lớn hơn hoặc bằng phần tử đứng trước. Ví dụ: 1, 5, 5, 6, 9. Loại thứ tự này xuất hiện khi trong một dãy có chứa các giá trị giống nhau.


**Insertion sort**

`why is Insertion sort best case big O complexity O(n)?`

If the input list is already sorted, the inner loop will terminate immediately for any i, i.e. the number of computational steps performed ends up being proportional to the number of times the outer loop is performed, i.e. O(n).

## Work and Span
### Work

The work of an algorithm corresponds to the total number of primitive operations
performed by an algorithm. If running on a sequential machine, it corresponds to the
sequential time. On a parallel machine, however, work can be divided among multiple
processors and thus does not necessarily correspond to time.

The interesting question is to what extent can the work be divided and performed in parallel. Ideally we would like to divide the work evenly. If we had W work and P processors to work on it in parallel, then even division would give each processor W/P fraction of the
work, and hence the total time would be W/P. An algorithm that achieves such ideal division is said to have **perfect speedup**. Perfect speedup, however, is not always possible.

Example 1.6. A fully sequential algorithm, where each operation depends on prior operations leaves no room for parallelism. We can only take advantage of one processor and the time would not be improved at all by adding more.

More generally, when executing an algorithm in parallel, we cannot break dependencies, if
a task depends on another task, we have to complete them in order.

### Span
The second measure, span, enables analyzing to what extent the work of an algorithm can be divided among processors. The span of an algorithm basically corresponds
to the longest sequence of dependences in the computation. **It can be thought of as the
time an algorithm would take if we had an unlimited number of processors on an ideal
machine.**

Definition 1.1 (Work and Span). We calculate the work and span of algorithms in a very
simple way that just involves composing costs across subcomputations. Basically we assume that sub-computations are either composed sequentially (one must be performed
after the other) or in parallel (they can be performed at the same time). We then calculate the work as the sum of the work of the subcomputations. For span, we differentiate
between sequential and parallel composition: we calculate span as the sum of the span
of sequential subcomputations or maximum of the span of the parallel subcomputations.
More concretely, given two subcomputations with work W1 and W2
and span S1 and S2,
we can calculate the work and the span of their sequential and parallel composition as
follows. In calculating the overall work and span, the unit cost 1 accounts for the cost of
(parallel or sequential) composition. 
Sequential composition W = 1 + W1 + W2 and S = 1 + S1 + S2
Parallel composition W = 1 + W1 + W2 and S = 1 + max(S1, S2)

*Note*. The intuition behind the definition of work and span is that work simply adds,
whether we perform computations sequentially or in parallel. The span, however, only
depends on the span of the maximum of the two parallel computations. **It might help to
think of work as the total energy consumed by a computation and span as the minimum
possible time that the computation requires. Regardless of whether computations are performed serially or in parallel, energy is equally required; time, however, is determined only
by the slowest computation**

## Genome Sequencing Problem
### Shotgun method
When we cut a genome into fragments we lose all the information
about how the fragments should be assembled. If we had some additional information
about how to assemble them, then we could imagine solving this problem. One way to get
additional information on assembling the fragments is to make multiple copies of the original sequence and generate many fragments that overlap. Overlaps between fragments can
then help relate and join them. This is the idea behind the shotgun (sequencing) method,
which was the primary method used in sequencing the human genome for the first time.

Example 3.2. For the sequence cattaggagtat, we produce three copies:

`cattaggagtat`

`cattaggagtat`

`cattaggagtat`

We then divide each into fragments

`catt — ag — gagtat`

`cat — tagg — ag — tat`

`ca — tta — gga — gtat`

Note how each cut is “covered” by an overlapping fragment telling us how to patch together the cut.

Definition 3.4 (Shotgun Method). The shotgun method works as follows.

1. Take a DNA sequence and make multiple copies.

2. Randomly cut the sequences using a “shotgun” (in reality, using radiation or chemicals) into short fragments.

3. Sequence each fragments (possibly in parallel).

4. Reconstruct the original genome from the fragments.

Remark. Steps 1–3 of the Shotgun Method are done in a wet lab, while step 4 is the algorithmically interesting component. Unfortunately it is not always possible to reconstruct the
exact original genome in step 4. **For example, we might get unlucky and cut all sequences
in the same location**. Even if we cut them in different locations, there can be many DNA
sequences that lead to the same collection of fragments. A particularly challenging problem is repetition: there is no easy way to know if repeated fragments are actual repetitions
in the sequence or if they are a product of the method itself.

### Genome Sequencing Problem
This section formulates the genome sequencing problem as an algorithmic problem.
Recall that the using the Shotgun Method, we can construct and sequence short fragments
made from copies of the original genome. Our goal is to use algorithms to reconstruct
the original genome sequence from the many fragments by somehow assembling back the
sequenced fragments. But there are many ways that such fragments can be assembled and
we have no idea how the original genome looked like. So what can we do? **In some sense
we want to come up with the “best solution” that we can, given the information that we
have (the fragment sequences).**

**Basic Terminology on Strings**. From an algorithmic perspective, we can treat a genome
sequence just as a sequence made up of the four different characters representing nucleotides. To study the problem algorithmically, let’s review some basic terminology on
strings.

**Definition 3.5 (Superstring).** A string r is a superstring of another string s if s occurs in r
as a contiguous block, i.e., s is a substring of r.

-  `gtat is a superstring of gta and tat but not of tac`

**Definition 3.6 (Substring, Prefix, Suffix).** A string s is a substring of another string r, if s
occurs in r as a contiguous block. A string s is a prefix of another string r, if s is a substring
starting at the beginning of r. A string s is a suffix of another string r, if s is a substring
ending at the end of r.

- `ag is a substring of ggag, and is also a suffix.`
- `gga is a substring of ggag, and is also a prefix`

**Definition 3.7 (Kleene Operators).** For any set Σ, its Kleene star Σ∗ is the set of all possible
strings consisting of characters Σ, including the empty string.

For any set Σ, its **Kleene plus** Σ+ is the set of all possible strings consisting of characters Σ,
excluding the empty string.

**Definition 3.8 (Shortest Superstring (SS) Problem).** Given an alphabet set Σ and a set of
finite-length strings A ⊆ Σ∗, return a shortest string r that contains every x 2 A as a
substring of r.

**Genome Sequencing as a String Problem. **

We can now try to understand the properties
of the genome-sequencing problem.

Exercise 3.1 (Properties of the Solution). What is a property that the result sequence needs
to have in relation to the fragments sequenced in the Shotgun Method?

Solution. Because the fragments all come from the original genome, the result should
contain all of them. In other words, the result is a superstring of the fragments.

Exercise 3.2. There can be multiple superstrings for any given set of fragments. Which
superstrings are more likely to be the actual genome?

**Solution. One possibly good solution is the shortest superstring. Because the Shotgun
Method starts by making copies of the original sequence, by insisting on the shortest superstring, we would make sure that the duplicates would be eliminated. More specifically,
if the original sequence had no duplicated fragments, then this approach would eliminate
all the duplicates created by the copies that we made in the beginning.**

### Algorithms for Genome Sequencing

**Brute Force**

Designing algorithms may appear to be an intimidating task, because it may seem as
though we would need brilliant ideas that come out of nowhere. In reality, we design algorithms by starting with simple ideas based on several well-known techniques and refine
them until the desired result is reached. Perhaps the simplest algorithm-design technique
(and usually the least effective) is brute-force—i.e., try all solutions and pick the best.

**Brute Force Algorithm 1. As applied to the genome-sequencing problem, a brute-force
technique involves trying all candidate superstrings of the fragments and selecting the
shortest one. Concretely, we can consider all strings x 2 Σ∗, and for each x check if every
fragment is a substring. Although we won’t describe how here, such a check can be performed efficiently. We then pick the shortest x that is indeed a superstring of all fragments.**

The problem with this brute-force algorithm is that there are an infinite number of strings
in Σ∗ so we cannot check them all. But, we don’t need to: we only need to consider strings
up to the total length of the fragments m, since we can easily construct a superstring by
concatenating all fragments. Since the length of such a string is m, the shortest superstring
has length at most m.

## Set and relations

A set is a collection of distinct objects. The objects that are contained in a set, are called members or the elements of the set. The elements of a set must be distinct: a set may not contain
the same element more than once. The set that contains no elements is called the empty set.

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/set1.JPG "Set1")

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/set2.JPG "Set2")

## Graph Theory

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/graph1.JPG "Graph1")

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/graph2.JPG "Graph2")

Remark. While directed graphs represent possibly asymmetric relationships, undirected
graphs represent symmetric relationships. Directed graphs are therefore more general than
undirected graphs because an undirected graph can be represented by a directed graph by
replacing an edge with two arcs, one in each direction.

Example of disconnected graph:

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/graph3.jpg "Graph3")

Definition 5.24 (Tree). An undirected graph is a **tree** if it does not have cycles and it is
connected. A **rooted tree** is a tree with a distinguished root node that can be used to access
all other nodes. An example of a rooted tree along with the associated terminology is given
in below.

Definition 5.25 (Rooted Trees). A **rooted tree** is a directed graph such that

1. One of the vertices is the **root** and it has no in edges.

2. All other vertices have one in-edge.

3. There is a path from the root to all other vertices.

Rooted trees are common structures in computing and have their own dedicated terminology.

- By convention we use the term **node** instead of vertex to refer to the vertices of a
rooted tree.

- A node is a **leaf** if it has no out edges, and an **internal nod**e otherwise.

- For each directed edge (u; v), u is the **parent** of v, and v is a **child** of u.

- For each path from u to v (including the empty path with u = v), u is an **ancestor** of
v, and v is a **descendant** of u.

- For a vertex v, its depth is the **length** of the path from the root to v and its **height*** is
the longest path from v to any leaf.

- The **height of a tree** is the height of its root.

- For any node v in a tree, the **subtree rooted** at v is the rooted tree defined by taking the
induced subgraph of all vertices reachable from v (i.e. the vertices and the directed
edges between them), and making v the root.

- As with graphs, an **ordered rooted tree** is a rooted tree in which the out edges (children) of each node are ordered.

![alt text](https://github.com/yeulam1thienthan/cs500/blob/master/src/common/images/graph4.JPG "Graph4")

## Sequences 

### Intro

If we were to identify the most fundamental ideas in computer science, we would probably
end up converging on a list that includes data structures. Data structures organize data in
a way that makes it possible for algorithms to work with the data quickly and efficiently.
If computation were to be the “hammer” of computer science, “data” would be the “nail”.
What is a hammer good for if there were no nails?

It is
possible to define sequences in several ways. One way is to use set theory. 

Mathematically, a sequence is an enumerated collection. As with a set, a sequence has elements. The length of the sequence is the number of elements in the sequence.

**Sequences allow for repetition: an element can appear at multiple positions**. The position
of an element is called its rank or its index. Traditionally, the first element of the sequence
is given rank 1, but, being computer scientists, we start at 0.

We define a sequence as a function whose domain is a contiguous set of natural numbers
starting at zero. This definition, stated more precisely below, allows us to specify the semantics of various operations on sequences succinctly.

An α sequence is a mapping (function) from N to α with
domain {0, ... , n − 1} for some n in N.

Example 12.1. 

Let A = {0; 1; 2; 3} and B = {’ a ’; ’ b ’; ’ c ’}. 

The function

R = {(0; ’ a ’); (1; ’ b ’); (3; ’ a ’)} from A to B has domain {0; 1; 3}. The function is not a sequence, because its domain has a
gap because it lacks 2 as index. In a relation, we note that sometimes indexes can be written expicitly like {a0; a1; ... ; an−1} or equivalent to {(0; a0); (1; a1); ... ; ((n − 1); an−1)}.

In contrast, The function
Z = {(1; ’ b ’); (3; ’ a ’); (2; ’ a ’); (0; ’ a ’)} is a sequence. The first element of the sequence is ’ a ’ and thus has rank 0. The
second element is ’ b ’ and has rank 1. The length of the sequence is 4.

For the sequence a = { 2; 3; 5; 7; 11; 13; 17; 19; 23; 29 }, we have

– a[0] = 2,

– a[2] = 5, and

– a[1 ... 4] = {3; 5; 7; 11}

A Z => Z function sequence:
<lambda x : x^2;

lambda y : y + 2;

lambda x : x − 4
>.

Here lambda is to define a function. For example, lambda x : x^2 is to define a function taking x as input and function definition is x^2

### Basic function

**Definition 13.3** (Length and indexing). Given a sequence a, length a, also written jaj, returns the length of a. The function nth returns the element of a sequence at a specified
index, e.g. nth a 2, written a[2], returns the element of a with rank 2. If the element demanded is out of range, the behavior is undefined and leads to an error.

**Definition 13.4** (Empty and singleton). The value empty is the empty sequence, h i. The
function singleton takes an element and returns a sequence containing that element, e.g.,
singleton 1 evaluates to h 1 i.

**Definition 13.5** (Functions isEmpty and isSingleton). To identify trivial sequences such as
empty sequences and singular sequences, which contain only one element, the interface
provides the functions isEmpty and isSingular. The function isEmpty returns true if the
sequence is empty and false otherwise. The function isSingleton returns true if the
sequence consists of a one element and false otherwise.

**Definition 13.6 (Tabulate)**. The function tabulate takes a function f and an natural number
n and produces a sequence of length n by applying f at each position. The function f can
be applied to each element in parallel. 

Example 13.1 (Fibonacci Numbers). Given the function fib i, which returns the i-th Fibonacci number, the expression:

a = <fib i : 0 ≤ i < 9>

is equivalent to
a = tabulate fib 9:
When evaluated, it returns the sequence

a = <0; 1; 1; 2; 3; 5; 8; 13; 21; 34>.

**Definition 13.8 (Map)**. The function map takes a function f and a sequence a and applies
the function f to each element of a returning a sequence of equal length with the results.
As with tabulate, in map, the function f can be applied to all the elements of the sequence
in parallel.

map (f : α ! β) <a1;...; an−1> : Sα) : Sβ = <f(a1);...; f(an−1)>

**Definition 13.10 (Filter)**. The function filter takes a Boolean function f and a sequence a
as arguments and applies f to each element of a. It then returns the sequence consisting
exactly of those elements of s 2 a for which f(s) returns true, while preserving the relative
order of the elements returned.

filter isPrime <10,13,17,21> = <13,17>

**Definition 13.12 (Subsequences)**. The subseq(a; i; j) function extracts a contiguous subsequence of a starting at location i and with length j. If the subsequence is out of bounds
of a, only the part within a is returned.

a[ei, ej] ≡ subseq (a; ei; ej − ei + 1):

**Splitting sequences**. As we shall see in the rest of this book, many algorithms operate
inductively on a sequence by splitting the sequence into parts, consisting for example, of
the first element and the rest, a.k.a., the head and the tail, or the first half or the second
half. We could define additional functions such as **splitHead, splitMid, take, and drop** for
these purposes. Since all of these are easily expressible in terms of subsequences, we omit
their discussion.

**Definition 13.13 (Append)**. The function append (a; b) appends the sequence b after the
sequence a. 

 <1; 2; 3> ++ <4; 5>
yields
<1; 2; 3; 4; 5> 

**Definition 13.14 (Flatten)**. To append more than two sequences the flatten a function takes
a sequence of sequences and flattens them. For the input is a sequence a = h a1; a2; : : : ; an i,
flatten returns a sequence whole elements consists of those of all the ai in order. We can
specify flatten more precisely as follows

flatten < < 1; 2; 3 > ; < 4 > ; h 5; 6>>

yields

<1; 2; 3; 4; 5; 6>

**Definition 13.15 (Update).** The function update (a; (i; x)), updates location i of sequence a
to contain the value x. If the location is out of range for the sequence, the function returns
the input sequence unchanged.

**Definition 13.16 (Inject)**. To update multiple positions at once, we can use inject. The
function inject (a; b) takes a sequence b of location-value pairs and updates each location
with its associated value. If a location is out of range, then the corresponding update is
ignored. If multiple locations are the same, one of the updates take effect.
In the case of duplicates in the update sequence b, i.e., multiple updates to the same position, we leave it unspecified which update takes effect. The function inject may thus treat
duplicate updates **non-deterministically (mean there are multiple possible results of inject)**.

**Definition 13.17 (Collect)**. Given a sequence of key-value pairs, the operation collect “collects” together all the values for a given key. This operation is quite common in data processing, and in relational database languages such as SQL it is referred to as “Group by”.

The following sequence consists of key-value pairs each of which
represents a student and the classes that they take.

kv = <(’ jack ’; ’ 15210 ’); (’ jack ’; ’ 15213 ’)
(’ mary ’; ’ 15210 ’); (’ mary ’; ’ 15213 ’); (’ mary ’; ’ 15251 ’);
(’ peter ’; ’ 15150 ’); (’ peter ’; ’ 15251 ’);
...
>.

We can determine the classes taken by each student by using collect cmp, where cmp is a
comparison function for strings

collect cmp kv = < (’ jack ’; < ’ 15210 ’; ’ 15213 ’; ...>)
(’ mary ’; < ’ 15210 ’; ’ 15213 ’; ’ 15251 ’; ...>);
(’ peter ’; < ’ 15150 ’; ’ 15251 ’; ...>);
...
>.

Note that the output sequence is ordered based on the first instance of their key in the
input sequences. Similarly, the order of the classes taken by each student are the same as
in the input sequence.
**Here, with a key, we collect all the value of the key and group them in form of a dictionary**

**Reduction**. The term reduction refers to a computation that repeatedly applies an associative binary operation to a collection of elements until the result is reduced to a single
value. Recall that associative operations are defined as operations that allow commuting
the order of operations.

Definition 13.19 (Assocative Function). A function f : α×α ! α is associative if f(f(x; y); z) =
f(x; f(y; z)) for all x; y and z of type α.

Example 13.9. Many functions are associative.
Addition and multiplication on natural numbers are associative, with 0 and 1 as their
identities, respectively.  Minimum and maximum are also associative with identities 1 and −1 respectively. The append function on sequences is associative, with identity being the empty sequence. The union operation on sets is associative, with the empty set as the identity. 

Note. Associativity implies that when applying f to some values, the order in which the
applications are performed does not matter. Associativity does not mean that you can
reorder the arguments to a function (that would be commutativity).

Important (Associativity of Floating Point Operations). Floating point operations are typically not associative, because performing them in different orders can lead to different
results because of loss of precision.
