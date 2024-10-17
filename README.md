# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
#include <unistd.h>

#include <sys/stat.h>

#include <fcntl.h>

#include <stdlib.h>

int main()

{

char block[1024];

int in, out;

int nread;

in = open("filecopy.c", O_RDONLY);

out = open("file.out", O_WRONLY|O_CREAT, S_IRUSR|S_IWUSR);

while((nread = read(in,block,sizeof(block))) > 0)

write(out,block,nread);

exit(0);}

## OUTPUT

$ gcc -o filecopy.o filecopy.c $ ls -l file.out1 -rw------- 1 gganesh gganesh 317 Aug 2 06:52 file.out1

$ diff filecopy1.c file.out1

## 2.To Write a C program that illustrates files locking

#include <fcntl.h>

#include <stdio.h>

#include <string.h>

#include <unistd.h>

#include <sys/file.h>

int main (int argc, char* argv[])

{ char* file = argv[1];

int fd;

struct flock lock;

printf ("opening %s\n", file);

/* Open a file descriptor to the file. */

fd = open (file, O_WRONLY);

// acquire shared lock

if (flock(fd, LOCK_SH) == -1) {

printf("error"); }else

{printf("Acquiring shared lock using flock");

}

getchar();

// non-atomically upgrade to exclusive lock

// do it in non-blocking mode, i.e. fail if can't upgrade immediately

if (flock(fd, LOCK_EX | LOCK_NB) == -1) {

printf("error"); }else

{printf("Acquiring exclusive lock using flock");}

getchar();

// release lock

// lock is also released automatically when close() is called or process exits

if (flock(fd, LOCK_UN) == -1) {

printf("error"); }else{

printf("unlocking");

}

getchar();

close (fd);

return 0;

}

## OUTPUT

$ gcc -o lock.o lock.c

$ ./lock.o tricky.txt

opening tricky.txt

Acquiring shared lock using flock

Acquiring exclusive lock using flock

Unlocking

$ lslocks COMMAND PID TYPE SIZE MODE M START END PATH

:

VBoxClient 1826 POSIX 5B WRITE 0 0 0 /home/gganesh/.vboxclient-draganddrop.pid

update-notifier 2405 FLOCK 0B WRITE 0 0 0 /run/user/1000/update-notifier.pid

lock2.o 3130 FLOCK 41B READ 0 0 0 /home/gganesh/class/2ndunit/tricky.txt

$ lslocks

COMMAND PID TYPE SIZE MODE M START END PATH

:

lock2.o 3130 FLOCK 41B WRITE 0 0 0 /home/gganesh/class/2ndunit/tricky.txt

# RESULT:
The programs are executed successfully.
