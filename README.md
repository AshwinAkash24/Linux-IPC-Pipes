# EX-03:Linux-IPC--Pipes
# Name: Ashwin Akash M
# Reference Number: 212223230024
# Department: Artificial Intelligence And Data Science
# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
> #include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/wait.h>

void server(int rfd, int wfd);
void client(int wfd, int rfd);

int main() {
    int p1[2], p2[2], pid;

    // Create two pipes
    if (pipe(p1) == -1 || pipe(p2) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }

    // Fork the process
    pid = fork();
    if (pid < 0) {
        perror("fork");
        exit(EXIT_FAILURE);
    }

    if (pid == 0) { // Child process (server)
        close(p1[1]); // Close write end of p1
        close(p2[0]); // Close read end of p2
        server(p1[0], p2[1]);
        return 0;
    }

    // Parent process (client)
    close(p1[0]); // Close read end of p1
    close(p2[1]); // Close write end of p2
    client(p1[1], p2[0]);

    wait(NULL); // Wait for child process to finish
    return 0;
}

void server(int rfd, int wfd) {
    char fname[2000];
    char buff[2000];
    ssize_t n;

    n = read(rfd, fname, sizeof(fname));
    if (n < 0) {
        perror("read");
        return;
    }
}   }   printf("No data received from server or an error occurred.\n"); output

```



## OUTPUT
![Screenshot from 2024-10-10 07-28-19](https://github.com/user-attachments/assets/f464d062-7972-44bf-82a3-64fbdcf77a54)


## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
int main(){
int res = mkfifo("/tmp/my_fifo", 0777);
if (res == 0) printf("FIFO created\n");
exit(EXIT_SUCCESS);
}

```



## OUTPUT
![image](https://github.com/user-attachments/assets/fb80d92e-b2c8-47a2-8d13-334669fcfeab)
![image](https://github.com/user-attachments/assets/fb301601-6da7-4d53-83d3-0465a3b4fcfa)


# RESULT:
The program is executed successfully.
