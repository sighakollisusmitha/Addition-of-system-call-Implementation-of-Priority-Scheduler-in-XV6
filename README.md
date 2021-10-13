# Addition of system call, implementation of priority scheduler in XV6 Operating system

### OPERATING SYSTEM USED: XV6

Xv6 is a simple Operating system developed by MIT for its own Operating Systems course. It is developed from the sixth edition of Unix and is coded in the C language. It is an Open Source Software and is freely available for us to develop upon.

### EMULATOR USED: QEMU




### INSTALLATION PROCEDURE:

### UPDATING: 

Before installing anything, we have to make sure that the ubuntu operating system is up to date. The Updated operating system makes our work easier and keeps our PC secured.

### COMMAND: 
sudo apt-get update. This command is used to update ubuntu operating system

### SCREENSHOT:

![image](https://user-images.githubusercontent.com/73429559/137186077-f3491e2d-7e92-4732-ab36-2d4cddf5b2ff.png)

### QEMU INSTALLATION:

### COMMAND:

Sudo apt-get install qemu 
### SCREENSHOT:

![image](https://user-images.githubusercontent.com/73429559/137186139-6ae25e5e-8550-427b-8e36-f5ac87830eef.png)

### COMMAND:

sudo apt get-install qemu-kvm

![image](https://user-images.githubusercontent.com/73429559/137186998-46efdbca-15fe-49c5-9fe7-647e7e477a6d.png)

### INSTALLING GIT REPOSITORY:

Git repository is installed to clone XV6 from github.

### COMMAND: 

sudo apt install git

![image](https://user-images.githubusercontent.com/73429559/137187663-c2e162d5-50c4-4174-a03c-b8ebd57157a3.png)

### CLONING XV6 FROM GITHUB:

Using git and cloning XV6 OS from git://github.com/mit-pdos/xv6-public.git

### COMMAND:

git clone git://github.com/mit-pdos/xv6-public.git

![image](https://user-images.githubusercontent.com/73429559/137187877-5b3a6d51-2cda-4114-a874-7371506063a9.png)

### INSTALLING MAKE REPOSITORY:

### COMMAND: 
Sudo apt install make

![image](https://user-images.githubusercontent.com/73429559/137187935-55c33dd2-4d77-4cf6-b1ed-237ad5ea3449.png)

### RUNNING XV6:

### COMMAND:

cd xv6-public make qemu

![image](https://user-images.githubusercontent.com/73429559/137188053-54103a36-8cff-426d-a0f1-849eb6b47932.png)

### Adding Simple User Program To Xv6:

First of all, I have created a C program as shown in below image. I saved it inside the source code directory of xv6 operating system with the name myprogram.c

### CODE:

#include “types.h” #include “stat.h” #include “user.h”
Int main(int argc,char *argv[ ])

{
    Printf(“a simple c program to experiment\n”); Exit();
}

![image](https://user-images.githubusercontent.com/73429559/137188119-8a6e2164-83a0-4343-8119-f4103e14097c.png)

### Makefile.c:
The Makefile needs to be edited to make our program available for the xv6 source code for compilation. The following sections of the Makefile needs to be edited to add our program myprogram.c

![image](https://user-images.githubusercontent.com/73429559/137190129-42493577-551f-48ef-8652-75a9a23fb692.png)

![image](https://user-images.githubusercontent.com/73429559/137190164-3d1839f9-01cf-4f23-ae56-62f468bb650e.png)

⮚	Now, start xv6 system on QEMU and when it booted up, run ls command to check whether our program is available for the user.

⮚	Here myprogram is availablein the list and by giving the name we can see the output of the Program In image below. 

![image](https://user-images.githubusercontent.com/73429559/137190294-8c2ebb1e-1a6e-419b-af1f-0044d74bbeb1.png)

### Adding New System Calls To xv6:


The following are the changes to be done to add our system call cps () to xv6:


### 1)	Add name to syscall.h :

This defines the position of the system call vector that connects to the implementation.

### CODE:	

#define sys_cps 22

![image](https://user-images.githubusercontent.com/73429559/137190357-f318822a-188d-412a-93fa-04247c4446a6.png)

### 2)	Add function prototype to defs.h :

This adds a forward declaration for the new system call. We add this function in proc.c

### CODE: 

int	cps(void);

![image](https://user-images.githubusercontent.com/73429559/137190504-fab1676d-714a-470c-b8b8-5f6387b7aca2.png)

### 3)	Add function prototype to user.h :
It defines the function that can be called through the shell. We add this function prototype in syscalls.
### CODE: 
int cps(void);

![image](https://user-images.githubusercontent.com/73429559/137190538-694d2368-23da-4a5e-b195-57c3c460a937.png)

### 4) Add function call to sysproc.c :

We add the real implementation of our method here. We add a function sys_cps in the file sysproc.c which calls the function cps().

### CODE:

Int sys_cps(void)

{

Return cps();

}

![image](https://user-images.githubusercontent.com/73429559/137190613-0f15a472-4860-4026-a96e-924f8bdd9276.png)

###  5) Add call to usys.S :

It uses the macro to define connect the call of user to the system call function.

### CODE: 
SYSCALL(cps)

![image](https://user-images.githubusercontent.com/73429559/137190663-951c5e90-f6da-4025-959e-7eb36a2ac406.png)

### 6) Add call to syscall.c :

It defines the function that connects the kernel and the shell and by using the position defined in syscall.h it adds the function to the system call.

### CODE: 
extern int sys_cps(void);

![image](https://user-images.githubusercontent.com/73429559/137193214-8bf08fa1-cc4b-4c9e-bed1-edc7613bb070.png)

### CODE: 
[SYS_cps] sys_cps,

![image](https://user-images.githubusercontent.com/73429559/137193369-20baf63a-20c2-4a27-8d28-c3976aff5de1.png)

###  7) Add code to proc.c :

We add this code to proc.c as written below.

It interrupts on the processor. It acquires a lock. It runs through the process table and checks whether the process is SLEEPING or RUNNING or RUNNABLE and then prints the same pid and status of the process. It releases the lock. It returns the syscall number which is 22.

CODE:

Int cps()

{

   struct proc *p;	// Enable interrupts on this processor
   sti();
   // Loop over process table looking for process with pid. 
   acquire(&ptable.lock);	// acquiring lock before use of critical section
   cprintf("name \t pid \t state \n");
   for(p = ptable.proc; p < &ptable.proc[NPROC]; p++) // checking process table
   {
        if(p->state == SLEEPING)
        cprintf("%s \t %d \t SLEEPING \n", p->name, p->pid); // printing pid,pname,state 
        else if(p->state == RUNNING)
        cprintf("%s \t %d \t RUNNING \n", p->name, p->pid);
    }

   release(&ptable.lock);	// releasing acquired lock 
   return 22;
}

![image](https://user-images.githubusercontent.com/73429559/137191088-ec27cf46-e42d-4c58-be9d-b94f0051a7df.png)

### 8)	
Create testing file ps.c with code shown below: 

### CODE:

#include “types.h” 
#include “stat.h” 
#include “user.h” 
#include “fnctl.h”

Int main(int argc, char *argv[ ])
{
    Cps();	// calling cps function exit();
}

![image](https://user-images.githubusercontent.com/73429559/137191175-b930142f-0adf-460a-bf0d-cd6db9842f40.png)

### 9)	Modify Makefile :

I am Modifying Makefile which compiles all changes we made inside the xv6 directories and subdirectories.

![image](https://user-images.githubusercontent.com/73429559/137191233-932499b7-2165-4514-bb99-1c36af94bc78.png)

### Output:
Now we will compile the whole code and execute the OS after the above changes are made.Our new syscall is now visible in the list:

![image](https://user-images.githubusercontent.com/73429559/137191270-77e7c014-1e8f-4036-9ef0-04b7509caf8d.png)

![image](https://user-images.githubusercontent.com/73429559/137191293-b1c0acde-27a0-4864-90c6-6df0062f72bf.png)

### IMPLEMENTATION OF PRIORITY SCHEDULING:

Default scheduling algorithm in XV6 operating system is round robin scheduling algorithm. It is not effective. It has more waiting time but priority scheduling algorithm reduces average waiting time.


### 1.	Add priority to struct proc in proc.h:


Struct proc in proc.h is typically like PCB(process control block).It consists information about all the processes in the system. We add attribute priority in the struct proc which represents the priority of the process.

### CODE: 

int priority; // process priority

![image](https://user-images.githubusercontent.com/73429559/137191336-65f1c61a-1770-4d24-8982-8278fb5ed808.png)

### 2.	
Assign a default priority in proc.h:

allocproc is a function that allocates resources to new process. It scans the entire process table and if it finds an unused entry then it will assign pid and resources to process. Here we set default priority for all the process to 60 in allocproc function.

### CODE:
p->priority=60;	//default priority set to 60

![image](https://user-images.githubusercontent.com/73429559/137191390-9601ce17-5206-4289-afbc-05213169e435.png)

### 3.	Adding code to print priority of process in cps in proc.c:

We have added code in cps function in proc.c to print priority of process along with process id, current process state and process name. 

![image](https://user-images.githubusercontent.com/73429559/137191430-bbc0f45d-2600-45fc-8c3f-977f5865b7bd.png)

### 4.	Creation of dummy program foo.c:

We create a dummy program named as foo.c and this dummy program will create a child and do sum dummy computations or calculations to waste CPU time. The main in this program take 2 arguments. The first argument is number of child processes that has to be created. We have used fork system call in this program to create child process.

CODE:
#include "types.h" 
#include "stat.h" 
#include "user.h" 
#include "fcntl.h"

int main(int argc, char *argv[])
{
    int k, n, id; 
    double x=0, z;
    if(argc < 2)
        n = 1;    // default value 
    else
        n = atoi(argv[1]);	// from user input 
    if(n<0 || n>100)
        n = 2; 
     x = 0;
    id = 0;
    for(k=0; k<n; k++)
    {
         id = fork(); 
         if(id < 0)
             printf(1, "%d failed in fork!\n", getpid()); 
         else if(id > 0)
         {	// Parent
             printf(1, "Parent %d creating child %d\n", getpid(), id); 
             wait();
         }
         
         else
         {	// Child
            printf(1, "Child %d created\n", getpid()); 
            for(z=0;z<8000000.0;z+=0.001)
                x = x + 3.14*69.69;	// Useless calculations to consume CPU time 
            break;
}
}

![image](https://user-images.githubusercontent.com/73429559/137191999-6176efb2-5981-4cc1-a6d0-f772642c6e04.png)

### 5.	Addition of new function chpr( change priority) to proc.c:

We add a new function named as chpr in proc.c file. This function takes two arguments. First argument is process id, and second argument is the priority, This function changes the priority of given process id.

CODE:
// Change priority int
ChangePriority(int pid, int priority)
{
struct proc *p;
acquire(&ptable.lock);	// acquire lock to access critical section 
for(p=ptable.proc; p<&ptable.proc[NPROC]; p++)
{
if(p->pid == pid)	// checking process table
{
p->priority = priority;	//changing priority of process break;
}
}
release(&ptable.lock); return pid;	}

![image](https://user-images.githubusercontent.com/73429559/137192152-6332af0a-f4e6-485f-ad61-89e1c072ab77.png)

### 6.	Adding system call(chpr):

As we have added system call cps, we have to follow same steps to add system call chpr in xv6 operating system.

### •	Add sys_chpr to syspoc.c:

![image](https://user-images.githubusercontent.com/73429559/137192198-5831814f-27de-4d5f-aac0-748dc09020cb.png)

### •	Adding to syscall.h:

![image](https://user-images.githubusercontent.com/73429559/137192254-1918c215-fcb1-433a-9515-7b1d0be94b34.png)

### •	Modify defs.h:

![image](https://user-images.githubusercontent.com/73429559/137192315-359b803c-6593-46a8-9f60-508d5cf1bcc6.png)

### •	Modify user.h:

![image](https://user-images.githubusercontent.com/73429559/137192353-31ee361a-fe39-4efc-ba77-4cebc77ed643.png)

### •	Modify usys.s:

![image](https://user-images.githubusercontent.com/73429559/137192392-fb2fbf0f-2d99-47d9-b971-d3e8b4b13b27.png)

### Modify syscall.c:

![image](https://user-images.githubusercontent.com/73429559/137192425-806ee616-7fb0-4a14-938e-80dbca266a2d.png)

![image](https://user-images.githubusercontent.com/73429559/137192457-02c32877-0ee6-4970-82e3-b8e1608b57ed.png)

### 6.	Adding nice.c program:

This user program will call system call chpr(change priority) to change priority of the process.

### CODE:

#include "types.h" #include "stat.h" #include "user.h" #include "fcntl.h"

int
    main(int argc, char *argv[])
      {
         int priority, pid;
         if(argc < 3)
{
printf(2, "Usage: nice pid priority\n"); exit();
}
pid = atoi(argv[1]); 
priority = atoi(argv[2]); 
if(priority<0 || priority>100)
{
printf(2, "Invalid priority (0-100)!\n"); exit();
}
printf(1, "pid=%d, pr=%d\n", pid, priority); ChangePriority(pid, priority);

exit();
}

![image](https://user-images.githubusercontent.com/73429559/137193731-7e410986-eddf-42ac-b1ca-f3d7bf4a63a0.png)

### OUTPUT:

![image](https://user-images.githubusercontent.com/73429559/137193778-e90e4045-39b2-47f5-97e7-81017059a818.png)

![image](https://user-images.githubusercontent.com/73429559/137193833-99dc12ca-5c99-42f1-a79d-368cd59caa48.png)

