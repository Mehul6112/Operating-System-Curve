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
        return processA->arrivalTime - processB->arrivalTime;
      }
      return processB->priority - processA->priority; // High priority first
    }
    
    void preemptivePriorityScheduling(Process processes[], int n) {
      int currentTime = 0;
      int completedProcesses = 0;
      int runningProcess = -1;
    
      while (completedProcesses < n) {
        int nextProcess = -1;
        int highestPriority = -1;
    
        // Find the highest priority process that has arrived
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
    
        // If a process is running and has higher priority, preempt it
        if (runningProcess != -1 && processes[runningProcess].priority < processes[nextProcess].priority) {
          processes[runningProcess].remainingTime += currentTime - processes[runningProcess].arrivalTime;
        }
    
        // Start or resume execution of the next process
        runningProcess = nextProcess;
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
    
      // Execute
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/d3fec725-67c4-41b8-a44c-f27ebb0d2177)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/b4241320-6ef8-46c9-8d09-4fbff0417b58)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/edfd1240-e520-4d5b-a4fc-b667e01cfbc9)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/01c6ec50-2fa9-4cf0-8bfd-30e063bf50dc)




