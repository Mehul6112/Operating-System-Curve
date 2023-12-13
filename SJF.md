    #include <stdio.h>
    #include <stdlib.h>
    
    typedef struct Process {
      int pid; // process ID
      int arrival_time; // arrival time
      int burst_time; // burst time
      int waiting_time; // waiting time
      int turnaround_time; // turnaround time
    } Process;
    
    void swap(Process *a, Process *b) {
      Process temp = *a;
      *a = *b;
      *b = temp;
    }
    
    void sort_processes(Process *processes, int n) {
      // Sort processes by arrival time first
      for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
          if (processes[i].arrival_time > processes[j].arrival_time) {
            swap(&processes[i], &processes[j]);
          }
        }
      }
    
      // Then sort processes by burst time
      for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
          if (processes[i].arrival_time == processes[j].arrival_time && processes[i].burst_time > processes[j].burst_time) {
            swap(&processes[i], &processes[j]);
          }
        }
      }
    }
    
    int main() {
      int number_of_processes;
    
      // Get number of processes
      printf("Enter the number of processes: ");
      scanf("%d", &number_of_processes);
    
      // Allocate memory for processes
      Process *processes = malloc(sizeof(Process) * number_of_processes);
    
      // Get process details for each process
      for (int i = 0; i < number_of_processes; i++) {
        printf("Enter details for process %d:\n", i + 1);
        printf("Arrival time: ");
        scanf("%d", &processes[i].arrival_time);
        printf("Burst time: ");
        scanf("%d", &processes[i].burst_time);
        processes[i].pid = i + 1; // assign process ID
        processes[i].waiting_time = 0; // initialize waiting time
        processes[i].turnaround_time = 0; // initialize turnaround time
      }
    
      // Sort processes by arrival time and burst time
      sort_processes(processes, number_of_processes);
    
      // Calculate waiting and turnaround times
      int current_time = 0;
      for (int i = 0; i < number_of_processes; i++) {
        processes[i].waiting_time = current_time - processes[i].arrival_time;
        current_time += processes[i].burst_time;
        processes[i].turnaround_time = processes[i].waiting_time + processes[i].burst_time;
      }
    
      // Display process details with step-by-step execution information
      printf("\nProcess Details:\n");
      printf("PID | Arrival Time | Burst Time | Waiting Time | Turnaround Time | Execution Time\n");
      for (int i = 0; i < number_of_processes; i++) {
        printf("%d | %d | %d | %d | %d | %d - %d\n", processes[i].pid, processes[i].arrival_time,
               processes[i].burst_time, processes[i].waiting_time, processes[i].turnaround_time, current_time - processes[i].burst_time, current_time);
      }
    
      // Free allocated memory
      free(processes);
    
      return 0;
    }
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/0de546ae-2321-47b4-9d55-6ce3ea447cd2)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/52bf5c6b-b7d4-4e48-a38c-a0f375f749e7)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/977c2727-39cb-4967-b0af-d04d56731fe1)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/cb9c1473-d304-411f-bd8b-09b957517a4b)


