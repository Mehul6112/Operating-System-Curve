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
    
    // Best-Fit Allocation
    int bestFit(struct MemoryManager* manager, int processSize) {
        int bestFitIndex = -1;
        int minFragmentation = INT_MAX;
    
        for (int i = 0; i < manager->blockCount; ++i) {
            if (!manager->memory[i].allocated && manager->memory[i].size >= processSize) {
                int fragmentation = manager->memory[i].size - processSize;
                if (fragmentation < minFragmentation) {
                    bestFitIndex = i;
                    minFragmentation = fragmentation;
                }
            }
        }
    
        if (bestFitIndex != -1) {
            manager->memory[bestFitIndex].allocated = 1;
            if (manager->memory[bestFitIndex].size > processSize) {
                manager->memory[manager->blockCount] = (struct MemoryBlock){manager->memory[bestFitIndex].id + 1,
                                                                            manager->memory[bestFitIndex].size - processSize, 0};
                manager->memory[bestFitIndex].size = processSize;
                manager->blockCount++;
            }
        }
    
        return bestFitIndex;
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
    
        // Demonstrate best-fit memory allocation
        int processSize = 200;
    
        printf("Initial Memory Status:\n");
        displayMemory(&memoryManager);
    
        printf("Attempting to allocate memory for a process of size %d using Best-Fit:\n", processSize);
        int index = bestFit(&memoryManager, processSize);
    
        if (index != -1) {
            printf("Memory allocated for process at index %d\n", index);
        } else {
            printf("Memory allocation failed.\n");
        }
    
        printf("Final Memory Status:\n");
        displayMemory(&memoryManager);
    
        return 0;
    }
