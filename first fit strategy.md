    #include <stdio.h>
    #include <limits.h>
    
    // Memory Block structure
    struct MemoryBlock {
        int id;
        int size;
        int allocated;
    };
    
    // Memory Manager structure
    struct MemoryManager {
        struct MemoryBlock memory[100]; // Assuming a maximum of 100 memory blocks
        int blockCount;
    };
    
    // Initialize memory manager
    void initializeMemoryManager(struct MemoryManager* manager, int size) {
        manager->memory[0] = (struct MemoryBlock){1, size, 0};
        manager->blockCount = 1;
    }
    
    // First-Fit Allocation
    int firstFit(struct MemoryManager* manager, int processSize) {
        for (int i = 0; i < manager->blockCount; ++i) {
            if (!manager->memory[i].allocated && manager->memory[i].size >= processSize) {
                manager->memory[i].allocated = 1;
                if (manager->memory[i].size > processSize) {
                    manager->memory[manager->blockCount] = (struct MemoryBlock){manager->memory[i].id + 1,
                                                                                manager->memory[i].size - processSize, 0};
                    manager->memory[i].size = processSize;
                    manager->blockCount++;
                }
                return i;
            }
        }
        return -1; // Memory not available
    }
    
    // Display memory status
    void displayMemory(struct MemoryManager* manager) {
        printf("Memory Status:\n");
        for (int i = 0; i < manager->blockCount; ++i) {
            printf("Block ID: %d, Size: %d, Allocated: %s\n", manager->memory[i].id, manager->memory[i].size,
                   (manager->memory[i].allocated ? "Yes" : "No"));
        }
        printf("\n");
    }
    
    int main() {
        // Set the initial size of memory
        int initialMemorySize = 1000;
        struct MemoryManager memoryManager;
        initializeMemoryManager(&memoryManager, initialMemorySize);
    
        // Demonstrate first-fit memory allocation
        int processSize = 200;
    
        printf("First-Fit Allocation:\n");
        int index = firstFit(&memoryManager, processSize);
    
        if (index != -1) {
            printf("Memory allocated for process at index %d\n", index);
        } else {
            printf("Memory allocation failed.\n");
        }
    
        displayMemory(&memoryManager);
    
        return 0;
    }
  ![image](https://github.com/Mehul6112/Operating-System-Curve/assets/119481480/d3890281-d41d-4b40-aa3f-b3ede5104a1f)

