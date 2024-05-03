# Discussion and Reflection


The bulk of this project consists of a collection of five
questions. You are to answer these questions after spending some
amount of time looking over the code together to gather answers for
your questions. Try to seriously dig into the project together--it is
of course understood that you may not grasp every detail, but put
forth serious effort to spend several hours reading and discussing the
code, along with anything you have taken from it.

Questions will largely be graded on completion and maturity, but the
instructors do reserve the right to take off for technical
inaccuracies (i.e., if you say something factually wrong).

Each of these (six, five main followed by last) questions should take
roughly at least a paragraph or two. Try to aim for between 1-500
words per question. You may divide up the work, but each of you should
collectively read and agree to each other's answers.

[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

one.irv:
((mov-lit r0 1) (print r0))

add.irv:
((move-lit r0 3)
(move-lit r1 4)
(mov-lit r2 0)
(add r2 r1 r0)
(print r2))

sub.irv:
((mov-lit r0 1)
(move-lit r1 2)
(move-lit r2 0)
(sub r2 r1 r0)
(print r2))

IR-Virtual is a simplified form of code used by compilers to help make programs work on different types of computers without needing specific adjustments for each one. Unlike x86 assembly, which is closely tied to
specific types of processors, IR-Virtual is more general, which means it doesn't need to concern itself
with the physical constraints of any particular hardware. Also, it provides a cleaner structured
form of the program compared with x86. The pros of IR-Virtual is that it useful for optimizing and transforming code in a broad way without getting stuck by the details of the hardware. It's cons is that since it uses virtual registers and abstract representations, it may increased compiler complexity.


[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.

For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.

For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.

We added 23.ifa, arith3.ifa, and arith4.ifa in test-programs

23.ifa:
Input: 23
Output: 23

arith3.ifa:
Input: (* (+ 1 (* 4 5) 9))
Output: 30
First, it's simplified from its original, complex form into a clearer set of steps in IfArithTiny. Then, it's organized into straightforward instructions in Administrative Normal Form (ANF), turned into a sort of virtual computer code in IR-Virtual, detailed further into x86 assembly language, and finally compiled into a NASM file that the computer can execute.

arith4.ifa:
Input: (+ 4 9)
Output: 13

[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

Input: (+ 4 9) Stage1: '+' is recognized as 'bop?', '4' and '9' are literals, the output is the same, in this step it just checks the form. Stage2: the input is already a basic arithmetic operation, so the output still the same. Stage3: input will be transformed into ANF, the output will be (let ([v1 4] [v2 9]) (+ v1 v2)). Stage4: each value and operation should be assigned to a virtual register, the output will be 'mov-lit v1, 4'; 'mov-lit v2, 9'; 'add v3, v1, v2'. Stage5: it will convert virtual registers into x86 registers. Step6: it will convert x86 into NASM assembly, which leads to the final result 13. If the input is just a single literal or a basic arithmetic operation, then the passes in stage1 and stage2 could be redundant.

[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

In the function 'ifarith->ifarith-tiny', all the match forms for 'let*' are tail recursions. In function 'ifarith-tiny->anf', it uses lambda expressions, and it has some relation to lambda calculus. In function 'reg->stackpos', 'reachable-labels' and 'registers', it uses foldl and hash tables, 'translated-instrs' uses foldl.

[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

[ High Level Reflection ] 

In roughly 100-500 words, write a summary of your findings in working
on this project: what did you learn, what did you find interesting,
what did you find challenging? As you progress in your career, it will
be increasingly important to have technical conversations about the
nuts and bolts of code, try to use this experience as a way to think
about how you would approach doing group code critique. What would you
do differently next time, what did you learn?

From this project, we learned a new type of compiler. It’s different from the previous compiler we compiled. It has several layers, each of them is like a form transition, and we figured out their principles and the approaches to achieve. We also learned new concepts such as virtual registers and stack allocation. We implemented translators, how to represent input in Administrative Normal Form. One interesting part we found is the instructions in ‘ir-virtual?’. They are not like any types we learned before, and it’s not easy to remember them the first time we saw them. But after understanding what’s the purpose of it, it’s quite straightforward. The most challenging part our group found was the debugging part. It’s usually spending much more time on debugging than designing the program. And it’s more difficult to debug on a large program that contains a lot of new concepts. But luckily, with the help of the clear and detailed instructions that professor gave to us and all the efforts us group members put in, we successfully overcame the challenge. From this group project, we learned the importance of group working. It’s not just working together, but how each of you divide the work, how to communicate immediately to increase efficiency. It’s really a precious opportunity to do the project with others, and we all think it’s of great help for our future career.

