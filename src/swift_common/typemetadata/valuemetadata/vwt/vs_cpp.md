
# Swift的VWT和C++对应关系

| Swift Value Witness Operation | C++ equivalent |
| ------------------------------ | ----- |
| `initializeWithCopy` | copy constructor |
| `assignWithCopy` | copy assignment operator |
| `initializeWithTake` | move constructor, followed by a call to destructor on the source |
| `assignWithTake` | move assignment operator, followed by a call to destructor on the source |
| `destroy` | destructor |
| `size` | `sizeof(T)` minus trailing padding |
| `stride` | `sizeof(T)` |
| `flags` | among other information, contains alignment, i.e., `alignof(T)` |
