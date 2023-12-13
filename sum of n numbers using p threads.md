    #include <pthread.h>
    #include <stdio.h>
    #include <stdlib.h>
    
    // Structure to store thread arguments
    typedef struct thread_args {
      int *arr; // Pointer to the array of numbers
      int start; // Starting index of the sublist
      int end; // Ending index of the sublist
      int sum; // Sum of the sublist
    } thread_args;
    
    // Function executed by each thread
    void *sum_sublist(void *arg) {
      thread_args *args = (thread_args *)arg;
      int local_sum = 0;
    
      // Calculate the sum of the sublist
      for (int i = args->start; i <= args->end; ++i) {
        local_sum += args->arr[i];
      }
    
      // Store the sum in the thread arguments
      args->sum = local_sum;
    
      // Exit the thread
      pthread_exit(NULL);
    }
    
    int main() {
      int n; // Number of numbers to sum
      int *arr; // Array of numbers
    
      // Get the number of numbers from the user
      printf("Enter the number of numbers: ");
      scanf("%d", &n);
    
      // Allocate memory for the array
      arr = malloc(n * sizeof(int));
    
      // Get the numbers from the user
      printf("Enter the numbers: ");
      for (int i = 0; i < n; ++i) {
        scanf("%d", &arr[i]);
      }
    
      // Create two threads
      pthread_t thread1, thread2;
      thread_args args1, args2;
    
      // Divide the array into two sublists
      args1.start = 0;
      args1.end = n / 2 - 1;
      args1.arr = arr;
    
      args2.start = n / 2;
      args2.end = n - 1;
      args2.arr = arr;
    
      // Create the threads
      pthread_create(&thread1, NULL, sum_sublist, &args1);
      pthread_create(&thread2, NULL, sum_sublist, &args2);
    
      // Wait for the threads to finish
      pthread_join(thread1, NULL);
      pthread_join(thread2, NULL);
    
      // Sum the results of the sublists
      int total_sum = args1.sum + args2.sum;
    
      // Print the total sum
      printf("The sum of the numbers is: %d\n", total_sum);
    
      // Free allocated memory
      free(arr);
    
      return 0;
    }
  
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/9b09f37a-6c42-40ab-9731-831fee615fec)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/2d69a033-a061-4866-9b85-c2a0f72338fd)
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/c1880932-12a0-4a3d-b769-5813e3e9701c)



