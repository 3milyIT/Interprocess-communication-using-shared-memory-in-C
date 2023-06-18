# Interprocess-communication-using-shared-memory-in-C


---

# Interprocess Communication using Shared Memory in C

Interprocess communication (IPC) is an essential aspect of developing multi-process applications. It enables processes to exchange data and synchronize their activities. Shared memory is one of the IPC mechanisms that allows processes to communicate by accessing shared regions of memory. In this tutorial, we will explore how to implement interprocess communication using shared memory in C.

## Introduction to Shared Memory

Shared memory is a segment of memory that can be accessed by multiple processes. It provides a fast and efficient way for processes to communicate with each other by sharing data in a shared memory region. To establish shared memory communication, processes need to follow these steps:

1. Create a shared memory segment using `shmget()`.
2. Attach the shared memory segment to the process using `shmat()`.
3. Access and modify the shared memory region.
4. Detach the shared memory segment from the process using `shmdt()`.
5. Optionally, delete the shared memory segment using `shmctl()`.

## Program Overview

In this tutorial, we will develop two programs: one for sending messages and another for receiving messages. The sender program will continuously send the messages "CIEPLO" and "ZIMNO" alternately to the shared memory region. The receiver program will read the messages from the shared memory and display them. If an invalid message is received, an error message will be displayed.

Let's take a look at the code for both programs.

## Sender Program

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t key = ftok(".", 'S');
    int shmid = shmget(key, 1024, IPC_CREAT | 0666);
    char* shmessage = (char*) shmat(shmid, NULL, 0);

    while (1) {
        memcpy(shmessage, "CIEPLO", sizeof("CIEPLO"));
        sleep(1);

        memcpy(shmessage, "ZIMNO", sizeof("ZIMNO"));
        sleep(1);
    }

    shmdt(shmessage);
    shmctl(shmid, IPC_RMID, NULL);

    return 0;
}
```

The sender program begins by generating a unique key using `ftok()`. This key will be used to create and access the shared memory segment. The `shmget()` function is then called to create a shared memory segment with a size of 1024 bytes. If the segment already exists, it will be opened; otherwise, a new segment will be created.

Next, `shmat()` is used to attach the shared memory segment to the process. This function returns a pointer to the shared memory region, which is stored in the `shmessage` variable.

In the infinite loop, the program alternately writes the messages "CIEPLO" and "ZIMNO" to the shared memory using `memcpy()`. After each write, the program waits for 1 second using `sleep()`.

Finally, the shared memory segment is detached from the process using `shmdt()`, and `shmctl()` is called to delete the shared memory segment.

## Receiver Program

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t key = ftok(".", 'S');
    int shmid = shmget(key, 

1024, IPC_CREAT | 0666);
    char* shmessage = (char*) shmat(shmid, NULL, 0);

    while (1) {
        if (strcmp(shmessage, "CIEPLO") == 0) {
            printf("Received: CIEPLO\n");
            sleep(1);
        }
        else if (strcmp(shmessage, "ZIMNO") == 0) {
            printf("Received: ZIMNO\n");
            sleep(1);
        }
        else {
            printf("Error: Invalid message\n");
            sleep(1);
        }
    }

    shmdt(shmessage);

    return 0;
}
```

The receiver program follows a similar structure as the sender program. It begins by generating the same key using `ftok()`, and then it calls `shmget()` to create or access the shared memory segment.

Next, `shmat()` is used to attach the shared memory segment to the process, and the resulting pointer is stored in `shmessage`.

In the infinite loop, the program checks the contents of the shared memory using `strcmp()`. If the message is "CIEPLO" or "ZIMNO", it is displayed. Otherwise, an error message is printed.

Finally, the shared memory segment is detached from the process using `shmdt()`.

## Running the Programs

To compile and run the sender and receiver programs, follow these steps:

1. Compile the sender program:
```
gcc sender.c -o sender
```

2. Compile the receiver program:
```
gcc receiver.c -o receiver
```

3. Run the sender program in one terminal:
```
./sender
```

4. Run the receiver program in another terminal:
```
./receiver
```

You should see the sender program continuously sending messages, and the receiver program displaying the received messages. If an invalid message is encountered, an error message will be shown.

## Conclusion

In this tutorial, we explored how to implement interprocess communication using shared memory in C. We developed two programs: one for sending messages and another for receiving messages. Shared memory provides a fast and efficient way for processes to communicate by sharing data in a shared memory region. By following the steps outlined in this tutorial, you can create your own interprocess communication systems using shared memory in C.


I hope you found this tutorial helpful. Happy coding!

---
