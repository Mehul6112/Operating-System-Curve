    #include <iostream>
    #include <fstream>
    #include <sstream>
    #include <string>
    
    using namespace std;
    
    int main() {
      string kernel_version, cpu_type;
      int cores = 0;
    
      ifstream("/proc/version") >> kernel_version;
      for (string line; getline(ifstream("/proc/cpuinfo"), line); ) {
        if (line.starts_with("model name")) {
          stringstream ss(line.substr(12));
          ss >> cpu_type;
        } else if (line.starts_with("physical id")) {
          cores++;
        }
      }
    
      cout << "Kernel Version: " << kernel_version << endl;
      cout << "CPU Type: " << cpu_type << endl;
      cout << "CPU Cores: " << cores << endl;
    
      return 0;
    }
