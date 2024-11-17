# Washing Machine Simulation

This program simulates the usage of washing machines in a student dormitory using multi-threading, semaphores, and mutex locks to handle concurrent access and resource management.

## Algorithm

The solution implements a multi-threaded approach where each student is represented by a separate thread. Here's the high-level algorithm:

1. **Main Thread**:
   - Reads input (N students, M machines)
   - Creates N threads, one for each student
   - Waits for all threads to complete
   - Calculates and displays final statistics

2. **Student Thread (routine)**:
   - Sleeps until student's arrival time
   - Enters queue (implemented as a linked list)
   - Attempts to get a washing machine while:
     - Checking if it's first in queue
     - Hasn't exceeded patience time
   - Either:
     - Gets a machine, washes clothes, and leaves
     - Leaves without washing if patience expires

3. **Synchronization Mechanism**:
   - `queueLock`: Protects queue operations (enqueue/dequeue)
   - `machineLock`: Protects machine allocation/deallocation
   - `queueCond`: Condition variable for queue management
   - FCFS (First Come First Serve) implemented through queue ordering

### Key Components

1. **Queue Management**:
   - Implemented as a linked list
   - Protected by mutex to ensure thread-safe operations
   - Functions: `enqueue()`, `dequeue()`

2. **Machine Management**:
   - Boolean array to track machine status
   - Protected by mutex for atomic operations
   - Functions: `getMachine()`, `freeMachine()`

3. **Thread Synchronization**:
   - Uses condition variables to avoid busy waiting
   - Implements FCFS scheduling through queue ordering
   - Thread-safe output using mutex-protected display function

## Compilation Instructions

1. Compile the program using g++ with pthread support:
```bash
g++ washing_simulation.cpp
```

2. Run the program:
```bash
./a.out
```

3. Input format:
   - First line: N (number of students) M (number of machines)
   - Next N lines: T_i (arrival time) W_i (wash time) P_i (patience)

## Assumptions

1. **Time Units**:
   - All time inputs (arrival, wash, patience) are in seconds

2. **Queue Management**:
   - Students are processed in FCFS order
   - In case of simultaneous arrivals, lower ID has priority

3. **Machine Allocation**:
   - Machines are identical and interchangeable
   - Machine allocation is atomic (protected by mutex)
   - Students can use any available machine

4. **Thread Safety**:
   - All shared resources are protected by appropriate mutex locks
   - Output is synchronized to prevent interleaved printing

5. **Error Handling**:
   - Input is assumed to be valid and within bounds
   - Maximum number of students and machines is limited to 1000

## Output Format

The program follows the specified color coding for different events:
- White: Student arrival
- Green: Starting to wash
- Yellow: Leaving after washing
- Red: Leaving without washing

Final output includes:
1. Number of students who missed their turn
2. "Yes" if more machines needed (≥25% miss rate), "No" otherwise

## Limitations

1. The simulation uses sleep functions which might not perfectly represent real-time scenarios
2. The order of output might vary between runs due to thread scheduling
3. Memory management assumes successful allocation of resources
