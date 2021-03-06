This at first looks like an easy problem. The algorithm for insertion is given in the problem description, and one is lead to believe that 

following that procedure will result in an accepted program. However, this is not so.

The first attempt is always the brute force solution. Making a binary search tree and inserting as given will result in a solution that is 

too slow - but enough for 50% of the test cases. The complexity is O(N<sup>2</sup>).

The main insight is that when one inserts a node, we are really querying what depth the node is being inserted into. Therefore, we need a 

fast way to query node depth of the tree.

After some thought, one realizes to represent the tree as a series of ranges. Keep a set with a node that contains the value of the node 

(1...N), as well as the depth of any node that is less than this value, and greater than this value. Since the set is sorted, two adjacent

nodes in the set represents an upper and lower bound, since all values are guaranteed to be unique.

When inserting, find the containing range to insert into (log N). The depth of this node is given by the containing nodes, and insertion

of this new node results in children that are one deeper than this node (obviously). This runs in time, for searching is log N, and you do

this N times for a total runtime of O(N log N).
