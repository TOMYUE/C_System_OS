

# lab01

- [x] gdb.txt
- [ ] `cgdb` debugging exercise
- [ ] `Valgrind`ing away
- [ ] Pointers and Structures in C



## Action Item

1. While you’re in a gdb session, how do you **set the arguments** that will be passed to the program when it’s run?

   `set args [Arguments]`

   `show args`

   ![image-20220419195437337](lab01.assets/image-20220419195437337.png)

2. How do you **create a breakpoint**

   - For `cgdb`
     - Change working section
       - press <esc> to enter the source code, and by pressing '`space`' to set a breakpoint at the current line.
       - press `i` on the keyboard to back to the `(gdb)` terminal
     - Set breakpoints
       1. in source section, press `space`on key board to set current line as a breakpoint.
       2. in gdb terminal, use command like this `b [n]` to set breakpoint at the  `n`th line. `b [file:] line`
       3. `b [file:] function` like `b main`, you set a breakpoint at the 'main' function entry.

3. How do you **execute the next line of C code** in the program after stopping at a breakpoint?

   By pressing `n` on our keyboard, execute next line including any funciton calls.

   One thing we should notice is the difference between the `n` and `c`

   - By pressing `c` on our keyboard.`continue [count]`or `c [count]`continue running; if *count* specifies, ignore this breakpoint next [count] times

4. If the next line of code is a function call, you’ll execute the whole function call at once if you use your answer to #3. (If not, consider a different command for #3!) How do you tell GDB that you **want to debug the code inside the function** (i.e. step into the function) instead? (If you changed your answer to #3, then that answer is most likely now applicable here.)

   `step`

   the difference between `stepi` and `step` mainly in that `stepi`= step by *machine instructions* rather than source line.

5. How do you **continue the program after stopping** at a breakpoint?

   `c`:ignore this breakpoint 

6. How can you **print the value of a variable** (or even an expression like 1+2) in gdb?

   `p [local_variable_name]`

7. How do you configure gdb so it **displays the value of a variable after every step**?

   

8. How do you **show a list of all variables and their values** in the current function?

   `info locals`: to list "local variables of current stack frame"(names and values), including variables in that funciton

   to list "All global and static variable names"(It might be a hug list)

   `info variables`:: to list "All global and static variable names"(It might be a hug list)

   - ```c
     void foo(a){
         int a;
     		if(a < 10){
           bar(a);
         }else{
           /* stop here */
           process(a); 
         }
     }
     
     int bar(int a){
       foo(a + 5);
     }
     ```

     ```bash
     (gdb) p a
     $1 = 10
     (gdb) p bar::a
     $2 = 5
     (gdb) up 2
     #2  0x080483d0 in foo (a=5) at foobar.c:12
     (gdb) p a
     $3 = 5
     (gdb) p bar::a
     $4 = 0
     ```

     The above shows how we could obtain the local variable separtely.

   `info args`: to list "Argumnets of the current stack frame"(name and values)

   `info register`:show all the current value in the registers.

9. How do you **quit** out of gdb?

   `(gdb) quit`



## 