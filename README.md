# Brief
cs500 design and analysis of algorithm

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


