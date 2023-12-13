    #include <stdio.h>
    #include <stdlib.h>
    #include <stdbool.h>
    #include <limits.h>
    
    typedef struct Process {
      int pid; // process ID
      int arrival_time; // arrival time
      int burst_time; // burst time
      int remaining_time; // remaining burst time
      int waiting_time; // waiting time
      int turnaround_time; // turnaround time
      bool completed; // process completion flag
    } Process;
    
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
        processes[i].remaining_time = processes[i].burst_time; // initialize remaining time
        processes[i].completed = false; // initialize completion flag
      }
    
      // Schedule processes using SRTF
      int current_time = 0;
      int completed_processes = 0;
      while (completed_processes < number_of_processes) {
        int shortest_process_index = -1;
        int shortest_remaining_time = INT_MAX;
    
        // Find the process with the shortest remaining time
        for (int i = 0; i < number_of_processes; i++) {
          if (!processes[i].completed && processes[i].arrival_time <= current_time && processes[i].remaining_time < shortest_remaining_time) {
            shortest_process_index = i;
            shortest_remaining_time = processes[i].remaining_time;
          }
        }
    
        // If no process is ready, advance time to the next arrival time
        if (shortest_process_index == -1) {
          for (int i = 0; i < number_of_processes; i++) {
            if (!processes[i].completed && processes[i].arrival_time > current_time) {
              current_time = processes[i].arrival_time;
              break;
            }
          }
          continue;
        }
    
        // Process the process with the shortest remaining time
        processes[shortest_process_index].remaining_time--;
        current_time++;
    
        // Check if process is completed
        if (processes[shortest_process_index].remaining_time == 0) {
          processes[shortest_process_index].completed = true;
          completed_processes++;
          processes[shortest_process_index].turnaround_time = current_time - processes[shortest_process_index].arrival_time;
          processes[shortest_process_index].waiting_time = processes[shortest_process_index].turnaround_time - processes[shortest_process_index].burst_time;
        }
      }
    
      // Display process details
      printf("\nProcess Details:\n");
      printf("PID | Arrival Time | Burst Time | Waiting Time | Turnaround Time\n");
      for (int i = 0; i < number_of_processes; i++) {
        printf("%d | %d | %d | %d | %d\n", processes[i].pid, processes[i].arrival_time,
               processes[i].burst_time, processes[i].waiting_time, processes[i].turnaround_time);
      }
    
      // Free allocated memory
      free(processes);
    
      return 0;
    }

![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/694513e0-2f8a-4961-ab5c-a8d0c1ef9dc6)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/60bc5f18-1c79-4e66-8260-6869763a5fbb)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/b201ada8-68a4-4684-b12a-ccba0ed7f2d1)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/598425b4-3bc1-4229-99a1-f5795a67480f)



