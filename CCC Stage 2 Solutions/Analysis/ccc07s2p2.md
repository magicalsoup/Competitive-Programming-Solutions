Brute force it.

We simply check every rotation and flip of every snowflake against every other snowflake.

Use a STL set/map of vectors in order to keep a list of already checked snowflakes as criteria to check later snowflakes against.
The lengths of each "blade" in a snowflake can be kept as a vector in the set and easily compared to another vector.

To cut down on a few comparisons however, only snowflakes equal in the sum of their lengths should be checked, 
otherwise they cannot be identical snowflakes.

For example, if given two snowflakes:
```
1 2 3 4 5 6
6 5 4 3 2 1

so we would check

1 2 3 4 5 6
----------- against
3 4 5 6 1 2
4 5 6 1 2 3
5 6 1 2 3 4

etc ...
```
If at ANY point we find a match, then we have found twin snowflakes and we're done!

If we go through every possible rotation of a snowflake and no match has been found, then we add it to our set/map of snowflakes and 
move on to the next one.

If we reach the end of the program without outputting anything, then we've found 0 identical snowflakes.
