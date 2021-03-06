solution of exercise of Mining of Massive Datasets
====================================================

## chapter 2( Large-Scale File Systems and Map-Reduce)
### section 2.3
**Exercise 2.3.1**: Design map-reduce algorithms to take a very large ?le of integers and produce as output:<br>
**(a) The largest integer.**<br>
solution:let **s** is set of integer that is input<br>the map function:for each integer in **s** such t,produce key-value pair (1,t)<br>
the reduce function:input of this task is pair that this key is 1,and associated value is list[t1,t2,...ti,...,tn] that each ti is value of pair of output of map task.output of reduce task is pair(1,max{t1,t2,....,tn})<br>
notice that max is a associative and commutative function,we can produce only one pair (1,max{t1,...tm}) in map task,that m is number of integer that input to map task.<br>
**(b) The average of all the integers.**<br>
solution:<br>
the map function:for each integer in s such t,produce key-value pair (1,t)<br>
the reduce function:input of this task is pair that this key is 1,and associated value is list[t1,t2,...ti,...,tn] that each ti is value of pair of output of map task.output of reduce task is pair(1,averge{t1,t2,....,tn})
<br>**(c) The same set of integers, but with each integer appearing only once.**
<br>solution:<br>
the map function:for each integer in s such t,produce key-value pair (t,t)<br>
reduce function:For each key t produced by any of the Map tasks, there will be one or more key-value pairs (t,t). The Reduce function turns (t,[t,t,...,t]) into (t',t'), so it produces exactly one pair (t,t) for this key t.<br>
**(d) The count of the number of distinct integers in the input.**<br>
from previous question we have:<br>
solution:<br>
map1:for each integer in s such t,produce key-value pair (t,t)<br>
reduce1:For each key t produced by any of the Map tasks, there will be one or more key-value pairs (t,t). The Reduce function turns (t,[t,t,...,t]) into (t',t'), so it produces exactly one pair (t,t) for this key t.<br>
map2:for each (t,t) output (s,1)
reduce 2:after grouping we have (s,[1...1]),output (s,sum[1,.....,1])<br>
**Exercise 2.3.2**:Our formulation of matrix-vector multiplication assumed that the matrix M was square. Generalize the algorithm to the case where M is an r-by-c matrix for some number of rows r and columns c.<br>
solution:<br>
we can this,similar to matrix-vector multiplication that study in lesson.<br>
we divide the matrix into vertical **stripes**.if we can't store this stripes in main memory,we must divide the matrix into squre matrix.and then the map_reduce is same map-reduce we study matrix-vector multiplication.<br>
**Exercise 2.3.3**: In the form of relational algebra implemented in SQL, relations are not sets, but bags; that is, tuples are allowed to appear more than once. There are extended deﬁnitions of union, intersection, and diﬀerence for bags, which we shall deﬁne below. Write map-reduce algorithms for computing the following operations on bags R and S:<br>
**(a) Bag Union, deﬁned to be the bag of tuples in which tuple t appears the sum of the numbers of times it appears in R and S.**<br>
solution:<br>
map:for each tuple t in R and S output (t,t)<br>
reduce:identity<br>
**(b) Bag Intersection, deﬁned to be the bag of tuples in which tuple t appears the minimum of the numbers of times it appears in R and S.**<br>
solution:<br>
map1:for each tuple t in R output (t,R)&for each tuple t in S output (t,S)<br>
reduce1:the inputs of this function are (t,[R,...,R,S,...,S]):<br>
for i in range list[t]:<br>
  if i==R<br>
      r=r+1<br>
   else<br>
      s=s+1<br>
output (t,r,s)<br>
map2:for each (t,r,s) output (t,min{r,s})<br>
reduce 2:for input (t,n) (that n=min{s,r}) produce n tuple(t,t)<br>
**(c) Bag Diﬀerence, deﬁned to be the bag of tuples in which the number of times a tuple t appears is equal to the number of times it appears in R minus the number of times it appears in S. A tuple that appears more times in S than in R does not appear in the diﬀerence.**<br>
map1:for each tuple t in R output (t,R)&for each tuple t in S output (t,S)<br>
reduce1:the inputs of this function are (t,[R,...,R,S,...,S]):<br>
for i in range list[t]:<br>
  if i==R<br>
      r=r+1<br>
   else<br>
      s=s+1<br>
output (t,r,s)<br>
map2:for each (t,r,s) output (t,r-s)<br>
reduce 2:for input (t,n) (that n=r-s) produce n tuple(t,t).(if n=<0,prodce nothing)<br>
**Exercise 2.3.4: Selection can also be performed on bags. Give a map-reduce implementation that produces the proper number of copies of each tuple t that passes the selection condition. That is, produce key-value pairs from which the correct result of the selection can be obtained easily from the values.**<br>
map:for each t satisfy C produce (t,1)<br>
reduce:after sorting ang grouping we have (t,[1,....,1]),we produce (t,sum[1,...,1])<br>
**Exercise 2.3.5: The relational-algebra operation R(A,B) ⊲⊳ B<C S(C,D) produces all tuples (a,b,c,d) such that tuple (a,b) is in relation R, tuple (c,d) is in S, and b < c. Give a map-reduce implementation of this operation, assuming R and S are sets.**<br>

