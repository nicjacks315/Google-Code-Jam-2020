# Parenting Partnering Returns (7pts, 12pts)

# Problem

Cameron and Jamie's kid is almost 3 years old! However, even though the child is more independent now, scheduling kid activities and domestic necessities is still a challenge for the couple.

Cameron and Jamie have a list of N activities to take care of during the day. Each activity happens during a specified interval during the day. They need to assign each activity to one of them, so that neither of them is responsible for two activities that overlap. An activity that ends at time t is not considered to overlap with another activity that starts at time t.

For example, suppose that Jamie and Cameron need to cover 3 activities: one running from 18:00 to 20:00, another from 19:00 to 21:00 and another from 22:00 to 23:00. One possibility would be for Jamie to cover the activity running from 19:00 to 21:00, with Cameron covering the other two. Another valid schedule would be for Cameron to cover the activity from 18:00 to 20:00 and Jamie to cover the other two. Notice that the first two activities overlap in the time between 19:00 and 20:00, so it is impossible to assign both of those activities to the same partner.

Given the starting and ending times of each activity, find any schedule that does not require the same person to cover overlapping activities, or say that it is impossible.
## Input

The first line of the input gives the number of test cases, `T`. `T` test cases follow. Each test case starts with a line containing a single integer `N`, the number of activities to assign. Then, `N` more lines follow. The i-th of these lines (counting starting from 1) contains two integers `Si` and `Ei`. The i-th activity starts exactly `Si` minutes after midnight and ends exactly `Ei` minutes after midnight.
## Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is `IMPOSSIBLE` if there is no valid schedule according to the above rules, or a string of exactly N characters otherwise. The i-th character in `y` must be C if the i-th activity is assigned to Cameron in your proposed schedule, and J if it is assigned to Jamie.

If there are multiple solutions, you may output any one of them. (See "What if a test case has multiple correct solutions?" in the Competing section of the FAQ. This information about multiple solutions will not be explicitly stated in the remainder of the 2020 contest.)
## Limits

- Time limit: 20 seconds per test set.
- Memory limit: 1GB.
- 1 ≤ T ≤ 100.
- 0 ≤ Si < Ei ≤ 24 × 60.
### Test set 1 (Visible Verdict)

 - 2 ≤ N ≤ 10.
### Test set 2 (Visible Verdict)

- 2 ≤ N ≤ 1000.
# Sample

## Input
 
```
4
3
360 480
420 540
600 660
3
0 1440
1 3
2 4
5
99 150
1 100
100 301
2 5
150 250
2
0 720
720 1440
```
  
## Output
	
```
Case #1: CJC
Case #2: IMPOSSIBLE
Case #3: JCCJJ
Case #4: CC
``` 

Sample Case #1 is the one described in the problem statement. As mentioned above, there are other valid solutions, like JCJ and JCC.

In Sample Case #2, all three activities overlap with each other. Assigning them all would mean someone would end up with at least two overlapping activities, so there is no valid schedule. 

# Analysis

## Test Set 1

We can solve this test set by naively trying every possible subset of activities to be covered by Jamie and assign the rest of the activities to be covered by Cameron. For each subset of activities, we can check whether a pair of activities overlap for each pair of activities. An activity with start time `s1` and end time `t1` overlaps with another activity with start time `s2` and end time `t2` if the time intersection is not empty (i.e., `max(s1, s2) < min(t1, t2)`).

The running time of this solution is O(2N × N<sup>2</sup>), which is fast enough to solve Test Set 1.
## Test Set 2

We can solve this test set by greedily assigning the activities in increasing order of start time. For each activity (in increasing order of start time), we can check whether Jamie or Cameron can be assigned to cover the activity and assign the activity to whomever can be assigned to (or arbitrarily if both partners can be assigned). The check can be done by iterating all activities that have been previously assigned to Jamie and Cameron.

The greedy assignment is correct because the only way that the assignment fails is when there is a time that is covered by three activities. In such a case, there is indeed no valid assignment. When deciding who to assign an activity with start time `s`, only activities with start times no later than s have been assigned. Therefore, if both Jamie and Cameron have some activity assigned with end time later than `s`, it means that there are three activities that use the time between `s` and `s + 1`, and therefore, there is no possible assignment. If an assignment is possible, there cannot be any set of three activities that pairwise overlap, so by the contrapositive of the the previous argument, we will be able to assign the activity to at least one of Jamie or Cameron at every step.

The running time of this solution is O(N<sup>2</sup>), which is fast enough to solve this test set. To optimize the solution to O(N log N) time, we can efficiently check whether an activity can be assigned to Jamie or Cameron by keeping track of the end time of the last activity assigned to each partner and comparing this to the start time of the new activity. In this case, only O(N) extra time is needed after sorting the activities by their start time.
## Graph approach

Another possible approach to solve this test set is to construct a graph with `N` nodes, each representing one activity. We add an edge connecting a pair of nodes if the pair of activities represented by the nodes overlap (see Test Set 1 section for details on how to check if two intervals overlap). This graph is commonly known as an interval graph.

Therefore, the problem is equivalent to finding a partition of nodes C and J such that every edge connects a node in C and a node in J, as we can assign all activities represented by nodes in C to Cameron and all activities represented by nodes in J to Jamie. The running time of the algorithm to find the partition (or report if one does not exist) is linear on the size of the graph. The graph has `N` nodes and O(N<sup>2</sup>) edges, which means the solution requires O(N<sup>2</sup>) time to build the graph and O(N<sup>2</sup>) time to run the partition algorithm, so also O(N<sup>2</sup>) time overall.
