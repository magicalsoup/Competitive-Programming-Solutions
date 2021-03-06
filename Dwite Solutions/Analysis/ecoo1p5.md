This problem is an exercise in discrete event simulation. There are a number of ways to approach it 

(and various people may see various approaches as being the most intuitive or obvious), so there is not necessarily any solution that 

deserves to be called the "model solution" and given special attention in analysis. Instead, much of the code in a solution addresses 

implementational trivialities, such as the choice of data structure for representing jobs.

One basic solution paradigm is as follows: at each step, we try to figure out which job will be the next to print. When the stack is empty

(as it is initially), we find the next job by reading a request from the input file. Otherwise, the next job to run will be the one at the 

top of the stack. In either case, there may be additional jobs that were submitted while we were waiting for that job to finish printing.

We add them to the stack, in order of submission, and then repeat this until all the jobs have been printed. 

The advantage of this approach is that the time at which the fifth job finishes will be determined at the fifth iteration, and the time at

which the last job finishes will be determined at the last iteration. 

This problem can also be solved with the classic discrete event simulation technique using a priority queue of events keyed by time; 

each event is either the submission of a new request or the completion of a job. In this case we will run a loop with twice as many 

iterations as jobs, and the completion of the fifth job will not occur at a fixed iteration number. We start with an empty stack, 

read all the requests from input, and place their submission events on the priority queue. Then we start the simulation. 

Every time we pop off a submission event, we check whether the printer is "busy" (it starts out not busy). 

If it is not busy, we push the completion event onto the queue (since we can now directly compute its time), and mark the printer busy. 

If it is busy, then we just push the event onto the stack. Every time we pop off a completion event, we check whether it's the fifth or 

the last; we pop the top submission off the stack and add its completion event to the queue, 

if the stack is nonempty; or mark the printer "not busy", if it is empty. There are some additional details to take care of here, such as

how to handle events that take place at the same time; these are left as exercises for the keener.
