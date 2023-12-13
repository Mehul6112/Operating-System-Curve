    #include <iostream>
    #include <unistd.h>
    #include <sys/wait.h>
    
    using namespace std;
    
    // Function to demonstrate different scenarios
    void execute(int scenario) {
      pid_t pid = fork();
    
      if (pid == 0) { // Child process
        switch (scenario) {
          case 1: // Same program, same code
            cout << "Child process executing same code as parent." << endl;
            break;
          case 2: // Same program, different code
            cout << "Child process executing different code." << endl;
            sleep(2);
            cout << "Child process complete." << endl;
            break;
          default:
            cout << "Invalid scenario." << endl;
            break;
        }
      } else if (pid > 0) { // Parent process
        wait(NULL);
        cout << "Parent process waiting for child to complete." << endl;
        cout << "Child process completed." << endl;
      } else {
        cout << "Error creating child process." << endl;
      }
    }
    
    int main() {
      // Choose scenario
      int scenario;
      cout << "Choose scenario (1-3): ";
      cin >> scenario;
    
      // Execute chosen scenario
      execute(scenario);
    
      return 0;
    }
    
