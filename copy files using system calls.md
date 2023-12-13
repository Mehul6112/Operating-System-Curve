    #include <stdio.h>
    #include <unistd.h>
    
    int main(int argc, char *argv[]) {
      if (argc < 3) {
        printf("Usage: %s <source_file> <destination_file>\n", argv[0]);
        exit(1);
      }
    
      if (cp(argv[1], argv[2]) == -1) {
        perror("Error copying file");
        exit(1);
      }
    
      printf("File copied successfully.\n");
    
      return 0;
    }
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/58d15a7d-b6f2-4b5a-9f5c-1df0a79adf67)
![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/c7fd1270-d7a0-4576-9b54-f85b2880ae56)

