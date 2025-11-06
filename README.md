# Linux-IPC-Message-Queues
Linux IPC-Message Queues
# Date 06-11-2025

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct mesg_buffer {
    long mesg_type;
    char mesg_text[100];
} message;

int main(int argc, char *argv[]) {
    key_t key;
    int msgid;

    if (argc != 2) {
        printf("Usage: %s writer|reader\n", argv[0]);
        return 1;
    }

    // Generate key
    key = ftok("progfile", 65);
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    // Create message queue and return id
    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    // Print msgid for grading script
    printf("Message Queue ID: %d\n", msgid);

    if (strcmp(argv[1], "writer") == 0) {
        message.mesg_type = 1;
        printf("Enter Message: ");
        fgets(message.mesg_text, sizeof(message.mesg_text), stdin);
        message.mesg_text[strcspn(message.mesg_text, "\n")] = 0; // remove newline

        if (msgsnd(msgid, &message, sizeof(message), 0) == -1) {
            perror("msgsnd");
            return 1;
        }

        printf("Message sent: %s\n", message.mesg_text);
    }
    else if (strcmp(argv[1], "reader") == 0) {
        if (msgrcv(msgid, &message, sizeof(message), 1, 0) == -1) {
            perror("msgrcv");
            return 1;
        }

        printf("Message received: %s\n", message.mesg_text);

        // Destroy the message queue
        msgctl(msgid, IPC_RMID, NULL);
    }
    else {
        printf("Invalid argument. Use writer or reader.\n");
        return 1;
    }

    return 0;
}

```



## OUTPUT


<img width="371" height="375" alt="Screenshot 2025-11-06 134834" src="https://github.com/user-attachments/assets/8a12bc29-6f95-4f12-a861-2a6b69992182" />

<img width="587" height="212" alt="Screenshot 2025-11-06 134942" src="https://github.com/user-attachments/assets/1069e609-217d-4ce2-a197-f94e0965dd90" />



# RESULT:
The programs are executed successfully.
