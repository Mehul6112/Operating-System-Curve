    #include <stdio.h>
    #include <sys/sysinfo.h>
    
    int main() {
      struct sysinfo info;
      
      if (sysinfo(&info) == 0) {
        printf("Configured Memory: %ld kB\n", info.totalram);
        printf("Free Memory: %ld kB\n", info.freeram);
        printf("Used Memory: %ld kB\n", info.totalram - info.freeram);
      } else {
        perror("sysinfo error");
        return 1;
      }
    
      return 0;
    }
