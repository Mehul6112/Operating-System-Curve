    #include <stdio.h>
    #include <stdlib.h>
    
    typedef struct process {
      int pid; // process ID
      int arrival_time; // arrival time
      int burst_time; // burst time
      int waiting_time; // waiting time
      int turnaround_time; // turnaround time
    } process;
    
    int main() {
      int number_of_processes;
    
      // Get number of processes
      printf("Enter the number of processes: ");
      scanf("%d", &number_of_processes);
    
      // Allocate memory for processes
      process *processes = malloc(sizeof(process) * number_of_processes);
    
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
    
      // Calculate waiting and turnaround times
      for (int i = 0; i < number_of_processes; i++) {
        int total_waiting_time = 0;
        for (int j = 0; j < i; j++) {
          total_waiting_time += processes[j].burst_time;
        }
        processes[i].waiting_time = total_waiting_time;
        processes[i].turnaround_time = processes[i].waiting_time + processes[i].burst_time;
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
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/7707570e-9f1d-4872-9388-b2f7aee57638)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/622901ca-7589-40da-bc9c-c0edcbd52c38)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/69ad55a0-767d-47bc-977f-8329072078d3)


