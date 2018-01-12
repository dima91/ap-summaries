# AP Summaries

## ***S_02***
**Software FW =>** Collecion of common code providing generic functionality that can be selectively ovirridden or specialzed by user code providing specific functionality  
**Application FW =>** A sw. framework used to implement the standard structure of an application for a specific development environment.  
A S.FW, like SW Libraries, provide *REUSABLE ABSTRACTIONS* of code wrapped in a well-defined API. Unlike libraries, FWs use the **Inversion of Control**: the overall program's flow control is dicted by the FW.

**OO S.FW =>** consists of a set of abstract classes. Instantiation of FW consists of composing and subclassing th existing classes.

**Design Patterns =>** general conceptual solutions to current design problems. More abstract than FWs, smaller architectural elements than FWs, less specialized than FWs.

## ***S_03***
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


## ***S_04***, ***S_05***
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


## ***S_06***
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



## ***S_07***
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

**Component forms**  
There are several way which a component could have: **-> CHECK ME!**
- ***Component Specification***: the specification of a unit of software that describes the behavior (defined as a set of interfaces) of a set of *Component Objects* and defines a unit of implementation. A Comp. Spec. is realized as a *Component Implementation*
- *Component Interface*: a definition of a set of behaviors that can be offeredby a *Component Object*
- *Component Implementation*: a sealization of a *Component Specification*, which is indipendently deployable. This means it can be installed and replaced indipendently of other components (not that is indipendent of other comp.)
- *Installed Component*: an installed/deployed copy of a *Component Implementation*
- *Component Object*: and instance of an *Installed Component*. It is a runtime concept, an object with its own data anda a unique identity. An *Installed Component* may have a multiple *Component Objects* or a single one


**Component Based Software Engeneering**
- The basis is the component
- Components can be assembled according to the rules specified by the component model
- Components are assembled through through their interfaces
- A **Component Composition** is the process of assembling components to form an assembly, a larger component or an application
- Component are performing in the context of a component framework
- All parts conform to the component modelled- A component technology is a concrete implementation of a component model


## ***S_08***

***TODO***


## ***S_09***

**The JavaBeans API**  
A Java Bean is a reusable software component model that can be manipulated visually in a builder tool (builders for web pages, visual applications, GUI layout, server applications, document editors, ...). Is a platform-neutral component architecture for reusable software component. Is a black box component to be used to build large component or application.  
Was defined a software component model for java, allowing vendors to create and ship java components that can be composed together into applications by end user.  

Characteristics:
- *granularity*: from small to medium
- *portability*
- *uniformity and semplicity*
- support for *properties*, both for customization and for programmatic use
- support for *events*, to connect several beans together
- support for *customization*, to customize the appearance and behavior of bean in an application builder
- support for *persistance*: after customization, a bean have its customized state saved away and can be reloaded later
- support for *introspection*: a builder tool can analyze how the bean works
- support for **Observer** design pattern, also known as **Publish-Subscribe**. In this case, an **event** is an object created by an *event source* and propagated to the registered *event listeners* (other beans)
- support for *event adaptors*, placed between the event source and listeners (it is at the same time a listener and a source)


A **Bound Property** generatesan event when the property is changed.  
A **Constrained Property** can only change value if none of the registered observer pose a *veto*. Once the property change, a *PropertyChangeEvent* event is generated and it is sent to objects that previously registered an interest in receiving such notifications.

## ***S_10***

**Reflection, Introspection, Intercession**  
*Reflection* is the ability of a program to manipulate as data something representing the state of the program during its own execution.  
*Introspection* is the ability of a program to read information about itself  
*Intercession* is the ability of a program to modify its own state through the description of itself.  
There are different levels which a system can support reflection: from simple information on types to reflecting entire structure of the program.  
Reflective capabilities need special support at the levels of language and compiler

**Cons**
+ *performance overhead*
+ *security restrictions*
+ *exposure of internals*

**In Java**  
+ java supports *inrospection* and *reflexive invocation*, but not code modification;
+ for every type, the JVM maintains an associated object of class *java.lang.Class*, which reflects the type it representd. It is the entry point of reflection;
+ three type of class memebers: *fields*, *methods*, *constructors* (thanks to **Member** interface, **Field**, **Method**, **Constructor** classes);
+ several methods to get infos about them;
+ for each memeber, the reflection API provides support to retreive declaration and type information, and operations unique to the member;
+ *Field* objects have type and value and can be setted or got; **FIXME**
+ *Method* objects have return values, parameters and may throw exceptions. They can be invoked on a given object;
+ *Constructor* objects are similar to methods but they haven't return value and an invocation of a constructor creates an object;


** In .NET**  
+ you can enumerate modules and types of an assembly;
+ obtain several infos for each type;
+ create istances of a types and invoke methods;



## ***S_11***

**Frameworks (cont.)**  
They support **inversion of control**: in order to use it, you need to insert your behavior into various places in the framework either by subclassing or by plugging in your own classes.  
FW, like libraries, provide reusable abstractions of code wrapped in a well-defined API.  
A FW is inteded to be extended to meet the needs of a particular application.
Two general topics: **Inversion of Control** and **Mastering dpendencies among components**.  
In a FW the application architecture is often fixed, even if customizabe, and determined by the FW.  
They provides **containers** for deploying components, which may provide at runtime functionalities needed by component to execute.


**Component Frameworks**  
Frameworks that supports:
+ *development*, *deployment*, *composition*, *execution* of components designed accordign to a given **Component model**
+ the development of individual component
+ the composition/connection of components
+ provide **prebuilt unctionalities** such as usefule components or automated assembly functions

**Advantages and techniques of loosely coupled systems**  
**TODO** -> *Factory and ServiceLocator, Reflection, auto-wiring*


## ***S_12***

The **Polymorphism** is the possibility of the same function or type to have many forms.  
It has two main forms: **Universal** or **Ad hoc**.  
With **universal polymorphism** the same function denotes different algorithms, determined by the actual types.  
With **ad hoc polymorphism** there is onl one algorithm: a single (universal) solution applies to different objects.

**Binding time**  
The BT of the unction name with the actual code to execute can be: at *compile time* (early/static binding), at *linking time*, at *execution time* (late/dynamic binding)

**Type of polymorphism**
+ **ad hoc**
  + **overloading**: present in all languages, at least for built-in arithmetic operators. The code to execute is determined by the type of the arguments: *early binding* in statically typed languages, *late binding* in dynamically typed languages.
  + **overriding**: redefinition of method *m* of class *A* in a sublcass *B* of *A*. Resolved at runtime by the lookup done by the invokevirtual operation of JVM.
+ **universal**
  + **coercion** : automatic (implicit) conversion of an object to a different type. opposed to *casting* which is explicit.
  + **parametric**
    + **implicit**
    + **expicit**
      + **bounded**
  + **inclusion**
    + **overriding**
    + **bounded**

**Inclusion polymorphism**  
aAlso known as *subtyping polymorphism*, or just **inheritance**.  
(from Liskov) *substitution principle*: an object of a subtype can be used in any context where an object of the supertype is expected.

**Parametric polymorphism** or **generic programming**  
+ **C++ templates**: each concrete instantiation produces a copy of the generic code, specialized for that type. Supports parametric polymorphism. Compiler choose template that i s the best match;
+ **Java generics**:


**STL**  
Represent algorithms in as general form as possible without compromising efficiency. Extensive use of templates adn overloading. Efficent example of generic programming. Only use static binding (and inlining) and iterators.
