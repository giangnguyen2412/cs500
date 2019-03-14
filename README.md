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
