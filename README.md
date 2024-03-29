# Cache-Simulator
Reference Trace Files

The traces subdirectory of the handout directory contains a collection of reference trace files that you can use to evaluate the correctness of the cache simulator you write. The trace files are generated by a Linux program called valgrind. For example, typing

linux> valgrind --log-fd=1 --tool=lackey -v --trace-mem=yes ls -l

on the command line runs the executable program ls -l, captures a trace of each of its memory accesses in the order they occur, and prints them on stdout. Valgrind memory traces have the following form:

I 0400d7d4,8

 M 0421c7f0,4

 L 04f6b868,8

 S 7ff0005c8,8

Each line denotes one or two memory accesses. The format of each line is

[space]operation address,size

The operation field denotes the type of memory access: "I" denotes an in- struction load, "L" a data load, "S" a data store, and "M" a data modify (i.e., a data load followed by a data store). There is never a space before each "I". There is always a space before each "M", "L", and "S". The address field specifies a 64-b hexadecimal memory address. The size field specifies the number of bytes accessed by the operation.

Writing a Cache Simulator

You will write a cache simulator that takes a valgrind memory trace as input, simulates the hit/miss behavior of a cache memory on this trace, and outputs the total number of hits, misses, and evictions. I have provided you with the binary executable of a reference cache simulator, called csim-ref, that simulates the behavior of a cache with arbitrary size and associativity on a valgrind trace file. It uses the FIFO (First In First Out) replacement policy when choosing which cache line to evict. The reference simulator takes the following command-line arguments:

Usage: ./csim-ref [-hv] -s <s> -E <E> -b <b> -t <tracefile>

-h: Optional help flag that prints usage info

-v: Optional verbose flag that displays trace info

-s <s>: Number of set index bits (S = 2 s is the number of sets)

-E <E>: Associativity (number of lines per set)

-b <b>: Number of block bits (B = 2 b is the block size)

-t <tracefile>: Name of the valgrind trace to replay

The command-line arguments are based on the notation (s, E, and b) from Chapter 6 of your textbook. For example:

linux> ./csim-ref -s 4 -E 1 -b 4 -t traces/yi.trace

hits:4 misses:5 evictions:3

The same example in verbose mode:

linux> ./csim-ref -v -s 4 -E 1 -b 4 -t traces/yi.trace

L 10,1 miss

M 20,1 miss hit

L 22,1 hit

S 18,1 hit

L 110,1 miss eviction

L 210,1 miss eviction

M 12,1 miss eviction hit

hits:4 misses:5 evictions:3

Your job is to write a cache simulator so that it takes the same command line arguments and produces the identical output as the reference simulator. Notice that this assignment starts from a "blank page." You'll need to write it from scratch.


For this assignment, we are interested only in data cache performance, so your simulator should ignore all instruction cache accesses (lines starting with "I"). Recall that valgrind always puts "I" in the first column (with no preceding space), and "M", "L", and "S" in the second column (with a preceding space). This may help you parse the trace.

You may NOT modify the trace files. I will be testing your simulator with traces of the same form as the reference traces I'm providing you, so your simulator MUST work with those traces as they are.

To receive credit you must clearly indicate the total number of hits, misses, and evictions at the end of each simulation.

For this assignment, you should assume that memory accesses are aligned properly, such that a single memory access never crosses block boundaries. By making this assumption, you can ignore the request sizes in the valgrind traces.
