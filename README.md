N-Queens Problem Solver
=======================
Solver for the N-Queens problem using OpenMP.

Building nqueens-openmp
------------------------------
- Install gcc 4.2 or higher
- Run the following command

    gcc -O -fopenmp -o nqueens-openmp nqueens-openmp.c
    
Running nqueens-openmp
-----------------------
    Usage: ./nqueens [n] [num_workers]

        num_workers    integer between 1 and 30. In case none is provided, or
                       a number out of this range is provided, then it's used 30
        n              positive integer indicating the size of the nxn board and number of queens
        
Speedup analysis
----------------
The execution time is measured only on the parallel block. This block uses only one shared variable, which is incremented in case a solution was found each ending of the for-loop. For that reason the block is highly parallelized and the speed-up should be very close to linear.

- Intel(R) Core(TM) i5 650 with 2 hyper-threaded cores @3.20GHz
The process was always run with n = 9 for different number of workers, being repeted 5 times for each number of workers configuration. The results can be seen on the table below.

| num_workers | median execution time | speedup |
| ----------- |:---------------------:| -------:|
| 1           | 23.3354 sec           | 1       |
| 2           | 12.1565 sec           | 1.9196  |
| 3           | 10.3652 sec           | 2.2513  |
| 4           | 8.9161 sec            | 2.5997  |

In this case it is not linear because this processor has only 2 cores. Consequently for more than 3 workers it starts sharing a core, cache, etc, which makes the performance not increase linearly anymore.

- AMD Opteron(tm) Processor 6172 with 12 hyper-threaded cores @2100MHz
The process was always run with n = 8 for different number of workers, being repeted 3 times for each number of workers configuration. The results can be seen on the table below.

| num_workers | median execution time | speedup |
| ----------- |:---------------------:| -------:|
| 1           | 5.24848 sec           | 1       |
| 2           | 2.68723 sec           | 1.9531  |
| 3           | 1.79051 sec           | 2.9313  |
| 4           | 1.34705 sec           | 3.8963  |
| 8           | 0.67400 sec           | 7.7870  |
| 12          | 0.45253 sec           | 11.598  |
| 24          | 0.245812 sec          | 21.352  |

In a similar way, the speedup increases linearly with num_workers between 1 to 12, since each task uses a separate core. Still we can see a very near to linear speedup for num_workers greater than 12, which indicates that this processor has a good multi-threaded architecture.

One interesting fact is that although the 6172 processor works in a much lower frequency than the i5 650, it solves the 9 queens problem faster when using more then 16 workers, presenting an execution time of 8.72762 sec with 17 workers and 6.22186 sec with 24 workers, while the Core i5 650 fastest time was 8.9161.
