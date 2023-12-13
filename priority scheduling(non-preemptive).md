    #include <stdio.h>
    #include <stdlib.h>
    
    typedef struct Process {
      int pid;
      int burstTime;
      int arrivalTime;
      int priority;
      int remainingTime;
      int waitTime;
      int turnaroundTime;
    } Process;
    
    int compareProcesses(const void* a, const void* b) {
      const Process* processA = (const Process*)a;
      const Process* processB = (const Process*)b;
      if (processA->arrivalTime != processB->arrivalTime) {
        return processA->arrivalTime - processB->arrivalTime; // First come, first served
      }
      return processB->priority - processA->priority; // High priority first
    }
    
    void nonPreemptivePriorityScheduling(Process processes[], int n) {
      int currentTime = 0;
      int completedProcesses = 0;
      int runningProcess = -1;
    
      while (completedProcesses < n) {
        // Find the highest priority process that has arrived
        int nextProcess = -1;
        int highestPriority = -1;
        for (int i = 0; i < n; ++i) {
          if (processes[i].arrivalTime <= currentTime && processes[i].remainingTime > 0) {
            if (processes[i].priority > highestPriority || (processes[i].priority == highestPriority && processes[i].arrivalTime < processes[nextProcess].arrivalTime)) {
              nextProcess = i;
              highestPriority = processes[i].priority;
            }
          }
        }
    
        // If no process is ready, advance time
        if (nextProcess == -1) {
          currentTime++;
          continue;
        }
    
        // Start execution of the next process
        if (runningProcess == -1) {
          runningProcess = nextProcess;
        }
    
        // Update process state and time
        processes[runningProcess].remainingTime--;
        currentTime++;
    
        // Check if process finishes execution
        if (processes[runningProcess].remainingTime == 0) {
          processes[runningProcess].turnaroundTime = currentTime - processes[runningProcess].arrivalTime;
          processes[runningProcess].waitTime = processes[runningProcess].turnaroundTime - processes[runningProcess].burstTime;
          runningProcess = -1;
          completedProcesses++;
        }
      }
    
      // Calculate average waiting and turnaround times
      float avgWaitTime = 0.0, avgTurnaroundTime = 0.0;
      for (int i = 0; i < n; ++i) {
        avgWaitTime += processes[i].waitTime;
        avgTurnaroundTime += processes[i].turnaroundTime;
      }
      avgWaitTime /= n;
      avgTurnaroundTime /= n;
    
      // Print results
      printf("Process ID\tArrival Time\tBurst Time\tPriority\tWait Time\tTurnaround Time\n");
      for (int i = 0; i < n; ++i) {
        printf("%d\t\t%d\t\t%d\t\t%d\t\t%d\t\t%d\n", processes[i].pid, processes[i].arrivalTime, processes[i].burstTime, processes[i].priority,
             processes[i].waitTime, processes[i].turnaroundTime);
      }
      printf("\nAverage waiting time: %.2f\nAverage turnaround time: %.2f\n", avgWaitTime, avgTurnaroundTime);
    }
    
    int main() {
      int n; // Number of processes
    
      printf("Enter number of processes: ");
      scanf("%d", &n);
    
      Process processes[n];
      for (int i = 0; i < n; ++i) {
        printf("Enter process details (pid, burst time, arrival time, priority): ");
        scanf("%d %d %d %d", &processes[i].pid, &processes[i].burstTime, &processes[i].arrivalTime, &processes[i].priority);
        processes[i].remainingTime = processes[i].burstTime;
      }
    
      // Sort processes by arrival time and priority
      qsort(processes, n, sizeof(Process), compareProcesses);
    
      // Execute non-preemptive priority scheduling
      nonPreemptivePriorityScheduling(processes, n);
    
      return 0;
    }

  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/264ad3b3-d2c6-4540-b3d4-714fb744a840)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/3d109f3c-d4a1-407f-83d7-73c3f346461e)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/305532dc-8bfb-47ef-a59c-5b22f7e09ed4)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/cd28bd8f-50df-4495-86b5-9e7061b41a73)




