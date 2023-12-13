    #include <stdio.h>
    #include <stdlib.h>
    
    // Process structure
    typedef struct Process {
      int pid; // Process ID
      int burstTime; // Burst time
      int arrivalTime; // Arrival time
      int waitTime; // Waiting time
      int turnaroundTime; // Turnaround time
    } Process;
    
    // Function to compare arrival times
    int compareArrivalTime(const void* a, const void* b) {
      const Process* processA = (const Process*)a;
      const Process* processB = (const Process*)b;
      return processA->arrivalTime - processB->arrivalTime;
    }
    
    // Round Robin algorithm
    void roundRobin(Process processes[], int n, int quantum) {
      int time = 0; // Current time
      int completed = 0; // Number of completed processes
    
      // Create a queue to hold processes
      Process queue[n];
      int front = 0, rear = 0;
    
      // Sort processes by arrival time
      qsort(processes, n, sizeof(Process), compareArrivalTime);
    
      // Main loop
      while (completed != n) {
        // Add processes that have arrived to the queue
        for (int i = 0; i < n; ++i) {
          if (processes[i].arrivalTime <= time && !processes[i].turnaroundTime) {
            queue[rear] = processes[i];
            rear = (rear + 1) % n;
          }
        }
    
        // Process the process at the front of the queue
        if (queue[front].burstTime > quantum) {
          time += quantum;
          queue[front].burstTime -= quantum;
        } else {
          time += queue[front].burstTime;
          queue[front].turnaroundTime = time - queue[front].arrivalTime;
          queue[front].waitTime = queue[front].turnaroundTime - queue[front].burstTime;
          completed++;
          front = (front + 1) % n;
        }
      }
    
      // Calculate average waiting and turnaround times
      float avgWaitTime = 0.0, avgTurnaroundTime = 0.0;
      for (int i = 0; i < n; ++i) {
        avgWaitTime += queue[i].waitTime;
        avgTurnaroundTime += queue[i].turnaroundTime;
      }
      avgWaitTime /= n;
      avgTurnaroundTime /= n;
    
      // Display results
      printf("Process ID\tBurst Time\tArrival Time\tWait Time\tTurnaround Time\n");
      for (int i = 0; i < n; ++i) {
        printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\n", queue[i].pid, queue[i].burstTime, queue[i].arrivalTime,
             queue[i].waitTime, queue[i].turnaroundTime);
      }
      printf("\nAverage waiting time: %.2f\nAverage turnaround time: %.2f\n", avgWaitTime, avgTurnaroundTime);
    }
    
    int main() {
      // Process data
      int n; // Number of processes
      int quantum; // Time quantum
    
      printf("Enter number of processes: ");
      scanf("%d", &n);
    
      printf("Enter process details (pid, burst time, arrival time):\n");
      Process processes[n];
      for (int i = 0; i < n; ++i) {
        scanf("%d %d %d", &processes[i].pid, &processes[i].burstTime, &processes[i].arrivalTime);
      }
    
      printf("Enter time quantum: ");
      scanf("%d", &quantum);
    
      // Execute Round Robin algorithm
      roundRobin(processes, n, quantum);
    
      return 0;
    }
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/6310c6a4-014f-4aff-b7a8-acd6563217b5)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/24de2e80-d671-4d20-b903-3e5dc8e9ed95)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/3528d2a8-cb10-4aa1-b1d4-8a6ce0b175c0)



