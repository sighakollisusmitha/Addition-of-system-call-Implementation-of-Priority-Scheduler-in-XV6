OPERATING SYSTEM USED: XV6

Xv6 is a simple Operating system developed by MIT for its own Operating Systems course. It is developed from the sixth edition of Unix and is coded in the C language. It is an Open Source Software and is freely available for us to develop upon.

EMULATOR USED: QEMU




INSTALLATION PROCEDURE:

UPDATING: Before installing anything, we have to make sure that the ubuntu operating system is up to date. The Updated operating system makes our work easier and keeps our PC secured.

COMMAND: sudo apt-get update. This command is used to update ubuntu operating system

SCREENSHOT:

![image](https://user-images.githubusercontent.com/73429559/137186077-f3491e2d-7e92-4732-ab36-2d4cddf5b2ff.png)

QEMU INSTALLATION:

COMMAND:

Sudo apt-get install qemu 
SCREENSHOT:

![image](https://user-images.githubusercontent.com/73429559/137186139-6ae25e5e-8550-427b-8e36-f5ac87830eef.png)

COMMAND:

sudo apt get-install qemu-kvm

![image](https://user-images.githubusercontent.com/73429559/137186998-46efdbca-15fe-49c5-9fe7-647e7e477a6d.png)

INSTALLING GIT REPOSITORY:

Git repository is installed to clone XV6 from github.

COMMAND: sudo apt install git

![image](https://user-images.githubusercontent.com/73429559/137187663-c2e162d5-50c4-4174-a03c-b8ebd57157a3.png)

CLONING XV6 FROM GITHUB:

Using git and cloning XV6 OS from git://github.com/mit-pdos/xv6-public.git

COMMAND:

git clone git://github.com/mit-pdos/xv6-public.git

![image](https://user-images.githubusercontent.com/73429559/137187877-5b3a6d51-2cda-4114-a874-7371506063a9.png)

INSTALLING MAKE REPOSITORY:

COMMAND: Sudo apt install make

![image](https://user-images.githubusercontent.com/73429559/137187935-55c33dd2-4d77-4cf6-b1ed-237ad5ea3449.png)

RUNNING XV6:

COMMAND:

cd xv6-public make qemu

![image](https://user-images.githubusercontent.com/73429559/137188053-54103a36-8cff-426d-a0f1-849eb6b47932.png)

Adding Simple User Program To Xv6:

First of all, I have created a C program as shown in below image. I saved it inside the source code directory of xv6 operating system with the name myprogram.c

CODE:

#include “types.h” #include “stat.h” #include “user.h”
Int main(int argc,char *argv[ ])

{

Printf(“a simple c program to experiment\n”); Exit();
}

![image](https://user-images.githubusercontent.com/73429559/137188119-8a6e2164-83a0-4343-8119-f4103e14097c.png)

Makefile.c:
The Makefile needs to be edited to make our program available for the xv6 source code for compilation. The following sections of the Makefile needs to be edited to add our program myprogram.c

![image](https://user-images.githubusercontent.com/73429559/137188171-aa6c81fe-0d20-41c1-a037-d1a2ddc3886f.png)

![image](https://user-images.githubusercontent.com/73429559/137188734-c5913ff2-7192-4f8e-a0e4-59065ec3df0b.png)

⮚	Now, start xv6 system on QEMU and when it booted up, run ls command to check whether our program is available for the user.

⮚	Here myprogram is availablein the list and by giving the name we can see the output of the Program In image below. 

![image](https://user-images.githubusercontent.com/73429559/137188302-7347d041-b0e4-4af4-8cdc-0f77c3f41491.png)



