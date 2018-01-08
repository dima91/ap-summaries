# AP Summaries

## S_02
**Software FW =>** Collecion of common code providing generic functionality that can be selectively ovirridden or specialzed by user code providing specific functionality  
**Application FW =>** A sw. framework used to implement the standard structure of an application for a specific development environment.  
A S.FW, like SW Libraries, provide *REUSABLE ABSTRACTIONS* of code wrapped in a well-defined API. Unlike libraries, FWs use the **Inversion of Control**: the overall program's flow control is dicted by the FW.

**OO S.FW =>** consists of a set of abstract classes. Instantiation of FW consists of composing and subclassing th existing classes.

**Design Patterns =>** general conceptual solutions to current design problems. More abstract than FWs, smaller architectural elements than FWs, less specialized than FWs.

## S_03
A **Program Language** is defined via *SYNTAX*, *SEMANTICS* and *PRAGMATICS*.  
The **Sytnax** is concerned with the form of programs: how expressions, commands, declaration and other constructs must be arranged to make a well-formed program. Used by compiler for *scanning* and *parsing*.  
The **Semantics** is concerned with the meaning of (well-formed) programs: how a program may be expected to behave when executed on a computer. Usually described precisally, but informally, in natural language.  
The **Pragmatics** is cocerned with the way in which PL is inteded to be used in practice. Includes coding conventions, guidelines for elegant structuring of code.

**Programming Paradigm =>** a PP is a style of programming, characterized by a particular selectionof a key concepts and abstractions (imperative, OO, concurrent, functional, logic).

**Implementation of a PL *L***
  1. programs written in *L* must be executable
  2. every language *L* implicitly defines an **Abstract Machine** *Ml* having **L** as machine language
  3. implementing *Ml* on an existing host machine *Mo* (via compilation or interpretation or both) makes program written in **L** executable

**Abstract Machine**  
Given a programming language *L*, an **AM** *Ml* for *L* is a collection of data structures and algorithms which can perform the storage and execution of programs written in *L*. The *AM* is an abstraction of te concept of hardware machine.  
Viceversa, each *AM* **M** defines a language **Lm** including all programs which can be executed by the interpreter of *M*.  
Programs are particular data on which the interpreter can act.

Each AM can be implemented in hardware or in firmware.  
Abstract machijne *M* can be implemented over a host machine *Mo*, which we assume already implemented.  
The components of *M* are realized using data structures and algorithms implemneted in the machine language of *Mo*.  
Two main cases to implement an AM:
  1. the interpreter of *M* coincides with interpreter of *Mo*: ***M* is an extension of *Mo***
  2. the interpreter of *M* differs from interpreter of *Mo*: ***M* is interpreted over *Mo***

**Implementing a PL =>** given a language *L*, an AM *Ml*, an host machine *Mo* and the language *Lo*, *L* can be implemented via:
  * **Pure interpretation**: *Ml* is interpreted over *Mo* (not very efficent).
  * **Pure compilation**: programs written in *L* are translated into equivalent programs written in *Lo*. The translated programs can be executed directly on *Mo* (execution more efficent, but the produced code is larger). ** *Ml* is not realized at all**.

**Compilation vs interpretation**  
Compilers efficently fix decisions that can be taken at compile time to avoid to generate code that makes this decision at run time.  
Compilation leads to better performance in general.  
Interpretation facilitates interactive debugging and testing.

**Compilation + interpretation**  
All implementations of programming languages use both: compilation from internal to external representation, and interpretation for I/O operation (runtime support).  
Can be modelled by indentifying a *Intermediate Abstract Machine* *Mi* with language *Li*: program in *L* is compiled to a program in *Li* and the obtained program is executed by an interpreter for *Mi*.
The intermediate abstract machine is called **Virtual Machine**.  
Using VMs involves higher *portability* (compile source, distribute the intermediate program and execute on any platform equipped with VM) and *portability* (for a new language *L*, just provide a compiler to intermediate language).

**Other compilation schemes**
1. Pure compilation and static linking (Fortran systems)
2. Compilation, assembly and static linking
3. Compilation, assembly and dynamic linking


## S_04, S_05
Every programming language defines an **Execution Model**.  
A **Runtime System** implemnts (part of) such EM, providing support during the execution of corresponding programs.  
The **Runtime Support** is needed both by interpreted and by compiled programs, even if tipically less by the latter.  
RSys can be made of code in the executing program or running in other threads/processes, language libraries, operating system functionalities, interpreter or virtual machine itself.  
RSup is needed for memory managment (both stack and heap), I/O operations, interaction with RE, parallel execution, dynamic type checking and binding, dynamic loading and linking of modules, debugging, code generation (JIT), optimizations, verification and monitoring.

**Java, JRE and JVM**  
The **Java Runtime Environment** includes all what is needed to run compiled Java programs, that is the **JVM** and the **JCL** (Java Class Library, java API).
The **Java Virtual Machine** is a multi-threaded stack based machine.  
Its specification doesn't give implementation details but defines constraints, which all JVM implementations must support, about **class file format** and strong syntactic and structural constraints on its code, internal data types.  
*For other details see **JVM Internals!** pdf.*


## S_06
**JVM Interpreter loop**  
```
do {
  calculate pc and fetch next opcode;
  if (operands) fetch operands;
  execute the action for the opcode;
} while (there is more to do);
```
- variable length IS
- simple to very complex
- symbolic references
- each instruction have different "forms"

IS composed by: load & store, arithmetic, type conversion, object creation & manipulation, operand stack manipulation, control transfer, method invocation & return, monitory entry/exit

The **Runtime Memory** is composed by local variable array (frame), operand stack (frame), object fields (heap), static fields (method area).  
The **Operand stack** is used to pass arguments to methods, return a result from a method, store intermediate results while evaluating expressions, store local variables. On the top contains arguments of functions and (at the end of operation) contains the results.

JVM support three *addressing modes*: **immediate a.m.** (constant is part of instruction), **indexed a.m.** (accessing variables from local variable array), **stack a.m.** (retreiving values from operand stack using *pop*).

**IS properties**
- has typed instructions (different opcodes for instruction for integers, floats, arrays, references types, ..);
- non-orthogonality of the IS;
- *byte*, *char* and *short* wrapped into *int* computational type;
- accessing locals and arguments: *load* & *store*;
- accessing fields in objects: *getfield* & *putfield*;
- accessing static fields (allocated in the method area): *getstatic* & *putstatic*;

**Method invocation and return**  
- *invokevirtual* for calling method on an object
- *invokeinterface* for calling methods declared in an interface
- *invokespecial* for calling constructors (not dinamically dispatched), private methods or supercalss methods
- *invokestatic* for calling static methods
- *invokedynamic* see **a first taste of invokedynamic**

**Special instructions**  
- *new* to create an object
- *newarray* for array of primitive types
- *anewarray*, *multianewarray* for array of reference types
- *monitorenter*, *monitorexit* for critcal sections
- *nop* no operation
- *athrow* for exceptions



## S_07
**Software Components**  
A software component is a *unit of composition* with *contractually specified interfaces* and *explicit context dependencies* only. A software component can be *deployed indipendently* and is subject to composition by third parties.  
A *contract* is a specification attached to an interface that mutually blinds clients and providers of the components. A contract specifies more than dependencies and interfaces: specifies how the component can be deployed, can be instantiated, the instances behave through the advertised interfaces. *Context dependencies* are specifications of the deployment environment and run-time environment


**SC characteristics**  
- modular (compatble, reusable, extensible)
- reliable (correct, robust)
- efficent
- portable
- timely
- are provided as binary units, without source code

**Basic concepts of component model**  
- *Component interface*: describes the operationsthat a component implements and that other components may use
- *Composition mechanism*: the manner which different components can be composed to work together to accomplish some tasks
- *Component platform*: a platform for the development and execution components

**Modules vs. components**  
*Modules* are the main feature of programming languages for supporting development of large applications (teams of programmers can work on separate modules in a project). M are an abstraction mechanism: collections of data with operations defined on them.  
***Modules are part of program, components are part of system***. Component can be anything and can contains anything.  
*Experience has shown that the use of OO doesn't necessarily produce reusable software*.

**CONTINUA DALLA SLIDE 7.21**
