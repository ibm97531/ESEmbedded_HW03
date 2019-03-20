HW03
===
This is the hw03 sample. Please follow the steps below.

# Build the Sample Program

1. Fork this repo to your own github account.

2. Clone the repo that you just forked.

3. Under the hw03 dir, use:

	* `make` to build.

	* `make clean` to clean the ouput files.

4. Extract `gnu-mcu-eclipse-qemu.zip` into hw03 dir. Under the path of hw03, start emulation with `make qemu`.

	See [Lecture 02 ─ Emulation with QEMU] for more details.

5. The sample is a minimal program for ARM Cortex-M4 devices, which enters `while(1);` after reset. Use gdb to get more details.

	See [ESEmbedded_HW02_Example] for knowing how to do the observation and how to use markdown for taking notes.

# Build Your Own Program

1. Edit main.c.

2. Make and run like the steps above.

3. Please avoid using hardware dependent C Standard library functions like `printf`, `malloc`, etc.

# HW03 Requirements

1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)

[Lecture 02 ─ Emulation with QEMU]: http://www.nc.es.ncku.edu.tw/course/embedded/02/#Emulation-with-QEMU
[ESEmbedded_HW02_Example]: https://github.com/vwxyzjimmy/ESEmbedded_HW02_Example

--------------------

- [x] **If you volunteer to give the presentation next week, check this.**

--------------------

**★★★ Please take your note here ★★★**
--------------------
#HW03
===

### project
1. How do C functions pass and return parameters? Please describe the related standard used by the Application Binary Interface (ABI) for the ARM architecture.

2. Modify main.c to observe what you found.

3. You have to state how you designed the observation (code), and how you performed it.

	Just like how you did in HW02.

3. If there are any official data that define the rules, you can also use them as references.

4. Push your repo to your github. (Use .gitignore to exclude the output files like object files or executable files and the qemu bin folder)
### process
1. my code is
```
void reset_handler(void)
{
int y=1;
y=2;
y=3;
int z;
z=y+7;
z=z+y;
char g=8;
int r=5;
int u=8;
int d=4;
while (1)
                ;
}
```
2. After "make" , I can get below
```
00000008 <reset_handler>:
   8:	b480      	push	{r7}
   a:	b087      	sub	sp, #28
   c:	af00      	add	r7, sp, #0
   e:	2301      	movs	r3, #1
  10:	617b      	str	r3, [r7, #20]
  12:	2302      	movs	r3, #2
  14:	617b      	str	r3, [r7, #20]
  16:	2303      	movs	r3, #3
  18:	617b      	str	r3, [r7, #20]
  1a:	697b      	ldr	r3, [r7, #20]
  1c:	3307      	adds	r3, #7
  1e:	613b      	str	r3, [r7, #16]
  20:	693a      	ldr	r2, [r7, #16]
  22:	697b      	ldr	r3, [r7, #20]
  24:	4413      	add	r3, r2
  26:	613b      	str	r3, [r7, #16]
  28:	2308      	movs	r3, #8
  2a:	73fb      	strb	r3, [r7, #15]
  2c:	2305      	movs	r3, #5
  2e:	60bb      	str	r3, [r7, #8]
  30:	2308      	movs	r3, #8
  32:	607b      	str	r3, [r7, #4]
  34:	2304      	movs	r3, #4
  36:	603b      	str	r3, [r7, #0]
  38:	e7fe      	b.n	38 <reset_handler+0x30>
  3a:	bf00      	nop
```
### My observation

1. reset_handler is arranged to the location 8.
2. System will use 20 bits in stack automatically,so I declare and define more variables.
  It use 28 bits in stack now.
3. int is about 4 bits.
4. char is about 1 bit.
5. r3 is used to be the workspace.
6. The value of variables will be stored in stack after any processing.
7. Any variable will get a address.EX: y->#20 , z->#16 ,g->#15 , r->#8 , u->#4 , d->#0
8. The order when using stack is from higher number to lower number.EX:20->16->15->8->4->0
9. If the variable is called,system will load the address of the variable into workspace.
10. The second used workspace is r2,when r3 is using.(Maybe the next one is r1 and then r0?)

