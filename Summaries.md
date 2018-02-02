# AP Summaries

## ***S_02***
**Software FW =>** Collection of common code providing generic functionality that can be selectively overridden or specialized by user code providing specific functionality.  
**Application FW =>** A SW framework used to implement the standard structure of an application for a specific development environment.  
A S.FW, like SW Libraries, provide *REUSABLE ABSTRACTIONS* of code wrapped in a well-defined API. Unlike libraries, FWs use the **Inversion of Control**: the overall program's flow control is dicted by the FW.

**OO S.FW =>** Consists of a set of abstract classes. Instantiation of FW consists of composing and subclassing th existing classes.

**Design Patterns =>** General conceptual solutions to current design problems. More abstract than FWs, smaller architectural elements than FWs, less specialized than FWs.

## ***S_03***
A **Program Language** is defined via *SYNTAX*, *SEMANTICS* and *PRAGMATICS*.  
The **Syntax** is concerned with the form of programs: how expressions, commands, declaration and other constructs must be arranged to make a well-formed program. Used by compiler for *scanning* and *parsing*.  
The **Semantics** is concerned with the meaning of (well-formed) programs: how a program may be expected to behave when executed on a computer. Usually described precisely, but informally, in natural language.  
The **Pragmatics** is concerned with the way in which PL is inteded to be used in practice. Includes coding conventions, guidelines for elegant structuring of code.

**Programming Paradigm =>** a PP is a style of programming, characterized by a particular selection of key concepts and abstractions (imperative, OO, concurrent, functional, logic).

**Implementation of a PL *L***

  1. Programs written in *L* must be executable
  2. Every language *L* implicitly defines an **Abstract Machine** *Ml* having **L** as machine language
  3. Implementing *Ml* on an existing host machine *Mo* (via compilation or interpretation or both) makes program written in **L** executable

**Abstract Machine**  
Given a programming language *L*, an **AM** *Ml* for *L* is a collection of data structures and algorithms which can perform the storage and execution of programs written in *L*. The *AM* is an abstraction of the concept of hardware machine.  
Viceversa, each *AM* **M** defines a language **Lm** including all programs which can be executed by the interpreter of *M*.  
Programs are particular data on which the interpreter can act.

Each AM can be implemented in hardware or in firmware.  
Abstract machine *M* can be implemented over a host machine *Mo*, which we assume already implemented.  
The components of *M* are realized using data structures and algorithms implemented in the machine language of *Mo*.  
Two main cases to implement an AM:
  1. the interpreter of *M* coincides with the interpreter of *Mo*: ***M* is an extension of *Mo***
  2. the interpreter of *M* differs from the interpreter of *Mo*: ***M* is interpreted over *Mo***

**Implementing a PL =>** given a language *L*, an AM *Ml*, an host machine *Mo* and the language *Lo*, *L* can be implemented via:
  * **Pure interpretation**: *Ml* is interpreted over *Mo* (not very efficent).
  * **Pure compilation**: programs written in *L* are translated into equivalent programs written in *Lo*. The translated programs can be executed directly on *Mo* (execution more efficent, but the produced code is larger). ** *Ml* is not realized at all. **

**Compilation vs interpretation**  
Compilers efficently fix decisions that can be taken at compile time to avoid to generate code that makes those decisions at run time.  
Compilation leads to better performance in general.  
Interpretation facilitates interactive debugging and testing.

**Compilation + interpretation**  
All implementations of programming languages use both: compilation from internal to external representation, and interpretation for I/O operations (runtime support).  
Can be modelled by indentifying a *Intermediate Abstract Machine* *Mi* with language *Li*: a program in *L* is compiled to a program in *Li* and the obtained program is executed by an interpreter for *Mi*.
The intermediate abstract machine is called **Virtual Machine**.  
Using VMs involves higher *portability* (compile source, distribute the intermediate program and execute on any platform equipped with VM) and *interoperability* (for a new language *L*, just provide a compiler to intermediate language).

**Other compilation schemes**

1. Pure compilation and static linking (Fortran systems)
2. Compilation, assembly and static linking
3. Compilation, assembly and dynamic linking


## ***S_04***, ***S_05***
Every programming language defines an **Execution Model**.  
A **Runtime System** implements (part of) such EM, providing support during the execution of corresponding programs.  
The **Runtime Support** is needed both by interpreted and by compiled programs, even if tipically less by the latter.  
RTSys can be made of code in the executing program or running in other threads/processes, language libraries, operating system functionalities, interpreter or virtual machine itself.  
RTSup is needed for memory managment (both stack and heap), I/O operations, interaction with RE, parallel execution, dynamic type checking and binding, dynamic loading and linking of modules, debugging, code generation (JIT), optimizations, verification and monitoring.

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
- each instruction has different "forms"

IS composed by: load & store, arithmetic, type conversion, object creation & manipulation, operand stack manipulation, control transfer, method invocation & return, monitory entry/exit

The **Runtime Memory** is composed by local variable array (frame), operand stack (frame), object fields (heap), static fields (method area).  
The **Operand stack** is used to pass arguments to methods, return a result from a method, store intermediate results while evaluating expressions, store local variables. On the top contains arguments of functions and (at the end of operation) contains the results.

JVM supports three *addressing modes*: **immediate a.m.** (constant is part of instruction), **indexed a.m.** (accessing variables from local variable array), **stack a.m.** (retreiving values from operand stack using *pop*).

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
- *invokespecial* for calling constructors (not dinamically dispatched), private methods or superclass methods
- *invokestatic* for calling static methods
- *invokedynamic* see **a first taste of invokedynamic**

**Special instructions**  

- *new* to create an object
- *newarray* for array of primitive types
- *anewarray*, *multianewarray* for array of reference types
- *monitorenter*, *monitorexit* for critical sections
- *nop* no operation
- *athrow* for exceptions



## ***S_07***
**Software Components**  
A software component is a *unit of composition* with *contractually specified interfaces* and *explicit context dependencies* only. A software component can be *deployed independently* and is subject to composition by third parties.  
A *contract* is a specification attached to an interface that mutually blinds clients and providers of the components. A contract specifies more than dependencies and interfaces: specifies how the component can be deployed, can be instantiated, the instances behave through the advertised interfaces. *Context dependencies* are specifications of the deployment environment and run-time environment.


**SC characteristics**  

- modular (compatible, reusable, extensible)
- reliable (correct, robust)
- efficient
- portable
- timely
- are provided as binary units, without source code

**Basic concepts of component model**  

- *Component interface*: describes the operations that a component implements and that other components may use
- *Composition mechanism*: the manner in which different components can be composed to work together to accomplish some tasks
- *Component platform*: a platform for the development and execution of components

**Modules vs. components**  
*Modules* are the main feature of programming languages for supporting development of large applications (teams of programmers can work on separate modules in a project). Modules are an abstraction mechanism: collections of data with operations defined on them.  
***Modules are part of a program, components are part of a system***. Components can be anything and can contain anything.  
*Experience has shown that the use of OO doesn't necessarily produce reusable software*.

**Component forms**  
There are several way which a component could have: **-> CHECK ME!**

- ***Component Specification***: the specification of a unit of software that describes the behaviour (defined as a set of interfaces) of a set of *Component Objects* and defines a unit of implementation. A Comp. Spec. is realized as a *Component Implementation*
- ***Component Interface***: a definition of a set of behaviours that can be offered by a *Component Object*
- ***Component Implementation***: a realization of a *Component Specification*, which is independently deployable. This means it can be installed and replaced independently from other components (not that is indipendent from other comp.).
- ***Installed Component***: an installed/deployed copy of a *Component Implementation*
- ***Component Object***: an instance of an *Installed Component*. It is a runtime concept, an object with its own data and a unique identity. An *Installed Component* may have a multiple *Component Objects* or a single one.


**Component Based Software Engeneering**

- The basis is the component
- Components can be assembled according to the rules specified by the component model
- Components are assembled through their interfaces
- A **Component Composition** is the process of assembling components to form an assembly, a larger component or an application
- Components are performing in the context of a component framework
- All parts conform to the component modelled
- A component technology is a concrete implementation of a component model


## ***S_08***

***TODO***


## ***S_09***

**The JavaBeans API**  
A Java Bean is a reusable software component model that can be manipulated visually in a builder tool (builders for web pages, visual applications, GUI layout, server applications, document editors, ...). Is a platform-neutral component architecture for reusable software component. Is a black box component to be used to build large components or applications.  
It was defined a software component model for java, allowing vendors to create and ship java components that can be composed together into applications by end user.  

Characteristics:

- *granularity*: from small to medium
- *portability*
- *uniformity and semplicity*
- support for *properties*, both for customization and for programmatic use
- support for *events*, to connect several beans together
- support for *customization*, to customize the appearance and behaviour of beans in an application builder
- support for *persistance*: after customization, a bean has its customized state saved away and can be reloaded later
- support for *introspection*: a builder tool can analyze how the bean works
- support for **Observer** design pattern, also known as **Publish-Subscribe**. In this case, an **event** is an object created by an *event source* and propagated to the registered *event listeners* (other beans)
- support for *event adaptors*, placed between the event source and listeners (it is at the same time a listener and a source)


A **Bound Property** generates an event when the property is changed.  
A **Constrained Property** can only change value if none of the registered observer pose a *veto*. Once the property changes, a *PropertyChangeEvent* event is generated and it is sent to objects that previously registered an interest in receiving such notifications.

## ***S_10***

**Reflection, Introspection, Intercession**  
*Reflection* is the ability of a program to manipulate as data something representing the state of the program during its own execution.  
*Introspection* is the ability of a program to read information about itself  
*Intercession* is the ability of a program to modify its own state through the description of itself.  
There are different levels in which a system can support reflection: from simple information on types to reflecting entire structure of the program.  
Reflective capabilities need special support at the levels of language and compiler

**Cons**

+ *performance overhead*
+ *security restrictions*
+ *exposure of internals*

**In Java**  

+ java supports *introspection* and *reflexive invocation*, but not code modification;
+ for every type, the JVM maintains an associated object of class *java.lang.Class*, which reflects the type it represents. It is the entry point of reflection;
+ three type of class memebers: *fields*, *methods*, *constructors* (thanks to **Member** interface, **Field**, **Method**, **Constructor** classes);
+ several methods to get infos about them;
+ for each memeber, the reflection API provides support to retreive declaration and type information, and operations unique to the member;
+ *Field* objects have type and value and can be setted or got; **FIXME**
+ *Method* objects have return values, parameters and may throw exceptions. They can be invoked on a given object;
+ *Constructor* objects are similar to methods but they haven't return value and an invocation of a constructor creates an object;


**In .NET**  

+ you can enumerate modules and types of an assembly;
+ obtain several infos for each type;
+ create istances of a type and invoke methods;



## ***S_11***

**Frameworks (cont.)**  
They support **inversion of control**: in order to use it, you need to insert your behaviour into various places in the framework either by subclassing or by plugging in your own classes.  
FW, like libraries, provide reusable abstractions of code wrapped in a well-defined API.  
A FW is inteded to be extended to meet the needs of a particular application.
Two general topics: **Inversion of Control** and **Mastering dependencies among components**.  
In a FW the application architecture is often fixed, even if customizable, and determined by the FW.  
They provide **containers** for deploying components, which may provide runtime functionalities needed by components to be executed.


**Component Frameworks**  
Frameworks that supports:

+ *development*, *deployment*, *composition*, *execution* of components designed according to a given **Component model**
+ the development of individual components
+ the composition/connection of components
+ provide **pre-built functionalities** such as useful components or automated assembly functions

**Advantages and techniques of loosely coupled systems**  
**TODO** -> *Factory and ServiceLocator, Reflection, auto-wiring*


## ***S_12***

The **Polymorphism** is the possibility of the same function or type to have many forms.  
It has two main forms: **Universal** or **Ad hoc**.  
With **universal polymorphism** the same function denotes different algorithms, determined by the actual types.  
With **ad hoc polymorphism** there is only one algorithm: a single (universal) solution applies to different objects.

**Binding time**  
The BT of the function name with the actual code to execute can be: at *compile time* (early/static binding), at *linking time*, at *execution time* (late/dynamic binding)

**Type of polymorphism**

+ **ad hoc**
  + **overloading**: present in all languages, at least for built-in arithmetic operators. The code to execute is determined by the type of the arguments: *early binding* in statically typed languages, *late binding* in dynamically typed languages.
  + **overriding**: redefinition of method *m* of class *A* in a subclass *B* of *A*. Resolved at runtime by the lookup done by the invokevirtual operation of JVM.
+ **universal**
  + **coercion**: automatic (implicit) conversion of an object to a different type. Opposite to *casting* which is explicit.
  + **parametric**: *C++ templates* and *Java generics*
     * **implicit**
     * **expicit**
         * **bounded**
  + **inclusion**
     + **overriding**
     + **bounded**

**Inclusion polymorphism**  
Also known as *subtyping polymorphism*, or just **inheritance**.  which
(from Liskov) *substitution principle*: an object of a subtype can be used in any context where an object of the supertype is expected.

**Parametric polymorphism** or **generic programming**  

+ **C++ templates**: each concrete instantiation produces a copy of the generic code, specialized for that type. Supports parametric polymorphism. Compiler choose the template that is the best match;
+ **Java generics**:


**STL (Standard Template Library)**  
Represents algorithms in as general form as possible without compromising efficiency. Extensive use of templates and overloading. Efficient example of generic programming. Only use static binding, inlining, and iterators (for decoupling algorithms from containers): not object oriented and no dynamic binding



## ***S_13***
**C++ templates**  
Support parametric polymorphism, type parameters can also be primitive types.  
*Compiler/linker automatically generates one version of each parameter type used by a program*.  
A (function or class) primary template can be specialized by defining another template with *same name* and more specific parameters (*partial specialization*) or no parameter (*full specialization*). This case is similar to overriding and compiler chooses most specific applicable template.  
**Macros** can be used for polymorphism in simple cases. They are executed by preprocessor (different from templates which are executed by the compiler).


**C++ template instantiation**  

1. compiler chooses the template that is the best match
2. template instance is created (similar to syntactic substitution of parameters)
3. overloading resolution after substitution (it fails if some operator is not defined for the type instance)

*The compiler needs both the declaration and the definition of the template function to instantiate it*: it cannot compile definition of template and code instantiating the template separately!

*Main entities*: containers, iterators, algorithms, adapter, function object, allocator


**Java generics**  
Case of *Explicit parametric polymorphism*. Classes, Interfaces and Methods can have type parameters. The type parameters can be used arbitrarily in the definition. They can be instantiated by providing arbitrary (reference) type arguments.  
They permit  *upper bounds* (extends), *lower bounds* (super), *multiple upper bounds*.  
Type bounds for methods guarantee that the type argument supports the operations used in the method body.  
Type checking ensures that overloading will succeded (different from C++)  
*Subtyping in Java is **invariant** for generic classes* (if T2 and T3 are different, then T1<T2\> is not a subtype of T1<T3\>).  
E.G.: Array<T2\> and Array<T3\> are not related by subtyping. But, instead, if T2 is subype of T3, then T2[] is subtype of T3[]. Thus *Java (raw) arrays are covariant*.

**Java rules**

- Given two concrete types *A* and *B*, *MyClass<A\>* has no relationship to *MyClass<B\>*, regardless of wether or not *A* and *B* are related. *Subtyping in Java is invariant for generic classes.*
- If *A* extends *B* and they are generic classes, for each type *C* we have that *A<C\>* extends *B<C\>*.

An operator is **Covariant** if maintains subtyping relationship.  
An operator is **Contravariant** if inverts subtyping relationship.  
For each reference variable, the dynamic type must be a subtype of the static one.

**Wildcards**
+ *? extends T* denotes an unknown subtype of T
+ *?* shorthand for *? extends Object*
+ *? super T* denotes an unknown supertype of T

**PECS (Producer Extends, Consumer Super) principle**

+ use *? extends T* when you want to get values: supports *covariance*
+ use *? super T* when you want to insert values: supports *contravariance*
+ never use *?* when you both obtain and produce values!

**Type erasure**: all type of parameters of generic types are transformed to Object or to their first bound after compilation

## ***S_15***

**Church's thesis**  
Any intuitively appealing model of computing would be equally powerful as well.

**Lambda calculus**  
Formal system which permits to analize functions and their computation.  
Based on the notion of parameterized expression (parameter introduces *λ*, hence the notation's name).  
Allows one to define mathematical functions in a constructive/effective way.  
Was the inspiration for *functional programming*.  
An occurence of *x* is **free** in a term *t* if it is not in the body of an abstraction *λx.t*, otherwise it is **bound**

**Functional programming**  
*Do everything by composing functions, no mutable state and no side effect*.  
Functional languages are an attempt to realize Church's lambda calculus in pratical form.  
Key concepts:

- high-order function (function that takes other functions as arguments or return them as a result. eg. map, reduce, filter)
- recursion (takes the place of iteration)
- powerful list facilities
- polymorphism (typically universal parametric implicit)
- garbage collection

**Haskell**  
Several features in common with ML family, but some differences:

- types and type checking (doesn't allow cast or similar things)
- ad hoc polymorphism (overloading)
- purely functional
- lazy evaluation
- imperative interpreter
- additional types: tuples, lists, records
- anonymous functions: *lambda abstraction* (f= \x -> x+1)
- **lazy language**: functions and data constructors don't evaluate their arguments until they need them


**Applicative order evaluation** (*parameter passing by value*): arguments are evaluated before applying the function  
**Normal order evaluation** (*parameter passing by name*): function evaluated first, arguments if and when needed

**Assignment** given *a:= b*  
The left-hand side *a* of the assignment is an *l-value* which is an expression that should denote a location  
A **reference to X** is the address of the base cell where X is stored  
A **pointer to X** is a location containing the address of X  
The right-hand side *b* of he assignment is an *r-value* which can be any  sintactically valid expression with a type that is compatible to the left-hand side
Languages that adopt **value model** of variables copy the value of b into the location of a  
Languages that adopt **reference model** of variables copy references, resulting in shared data values via multiple references

**Parameter passing**  
*Call by **Sharing***: parameter passing of data in the reference model. The value of the variables is passed as actual argument, which in fact is a reference to the (shared) data  
*Call by **Name***: parameter passing of data in Algol 60. The actual parameter is copied wherever the formal parameter appears on body, then the resulting code is executed. Since the actual parameter can contains names, it is passed in a **closure** with the environment at invocation time.  
*Call by **Need***: parameter passing of data in Haskell. An expression passed as argument is evaluated only if its value is needed. The argument is evaluated only the first time and further uses of argument do not need to re-evaluate it. Combined with *lazy data constructors*, this allows to construct pontentially infinite data structure.



## ***S_17***

**Recursion**  
When subroutines call themselves directly or indirectly (mutual recursion).  
Is the natural solution when the solution of a problem is defined in terms of simplier version of the same problem, as for *tree traversal*.  
Recursion is in general less efficent than iteration, but good compilers for functional languages can perform good code optimization.  
**Tail recursive functions** are functions in which no operations follow the recursive call(s) in the function, thus the function returns immediately after recursive call. A tail recursive call could reuse the subroutine's frame on the run-time stack. Moreover the compiler replaces tail-recursive calls by jumps to the beginning of the function.

**Java 8 Lambdas**  
They enable:

- functional programming in JavaBeans
- to pass behaviours as well as data to functions
- introduction of lazy evaluation with stream processing
- facilitation of parallel programming

*See slides for lambdas syntax*

**Implementation of lambdas**  
The java 8 compiler first converts a lambda expr. into a function, compiling its code. Then it generates code to call the compiled function where needed.  
Lambdas are instances of **functional interfaces**, thus an interface with exactly one abstract method with the annotation *@FunctionalInterface*.  
L can be interpreted as instances of anonymous inner classes implementing the functional interfaces.  
Arguments and result types of the lambda must match those of the unique abstract method of the functional interface.  

**Method references**  
MR can be used to pass an existing function in places where a lambda is expected.  
The signature of the referenced method needs to match the signature of the functional interface method.


## ***S_18***

The *java.util.stream* package provides utilities to support functional-style operations on streams of values.  
**Streams** differ from Collections in several ways: they haven't *storage* (a stream is not a data structure that stores elements) and are *functional in nature* (an operation on a stream produces a result, but doesn't modify its source).  
  
Properties:

- many stream operations can be implemented lazily, exposing opportunities for optimization. Stream operations are divided into **intermediate** operations and **terminal** operations.  
*Intermediate operations are always lazy*.
- they are possibly **unbounded**: collections have finite size, streams need not. Short-circuiting operations such as *limit(n)* or *findFirst()* can allow computations on infinite streams to complete in finite time.

+ the elements of a stream are only visited once during the life of a stream.

A pipeline of operations contains: a **source** (which produce the elements of the stream), zero or more **intermediate operations** (lazy operations, which produce streams) and a **terminal operation** (which produces side-effects or non-stream values).  
Intermediate operations are performed on the stream elements and they are not processed until the terminal method is called.  
A stream is considered consumed when a terminal operation is invoked.  
A **Collector** encapsulates the functions used as arguments to collect (Supplier, BiConsumer, BiConsumer), allowing for reuse of collection strategies and composition of collect operations.  
A Stream can be an **infinite stream** which can be generated with *iterate* and *generate* operations.  
Streams facilitate parallel execution thanks to runtime support which takes care of using multithreading for parallel execution, in a transparent way. If operations don't have side effects, thread-safety is guaranteed even if non-thread-safe collections are used.  
*Order of processing stream elements depends on serial/parallel execution and intermediate operations*


## ***S_19***

**Type inference** is the process of associating a type with a symbol of a program, if possible, ensuring type safety.  
A **data type** is an homogeneous collection of values, effectively presented, equipped with a set of operations which manipulate these values (various perspective depending on programming paradigm)  
A **Type system** consists of:

- the set of predefined types of the language
- the mechanism which permits the definition of new types
- the mechanism for the control (checking) of types, which includes: equivalence rules, compatibility rules and rules and techniques for type inference
- the specification as to whether (or which) constraints are statically or dynamically checked

A **type error** occurs when a value is used in a way that is inconsistent with its definition (hardware interrupt, OS exception, continue execution possibly with wrong values)  
A language is **type safe (strongly typed)** when no program can violate the distinctions between types defined in its type system. That is when no program, during its execution, can generate an unsignalled type error  
To prevent type errors, before any operation is performed, its operands must be **type-checked** to ensure that they comply with the compatibility rules of the type system. Two type of languages: *statically typed* and *dynamically typed*.  
In **statically typed PL** all available expressions have fixed types and most operands are type checked at compile time (even if some type checking is done at run-time).  
In a **dynamically typed PL** values have fixed types, but variables and expressions do not. Operands must be type-checked when they are computed at run-time.

*Pro type inference:*  
- reduces syntactic overhead of expressive types
- guaranteed to produce most general type
- increasinly used also in imperative/OO languages
- illustrative example of a flow-intensitive static analysis algorithm.

**Constraints:**  
*Function application*: [f x] ==> [t0 = t1 -> t2], where t1 is domain of f (type of x) and t2 is range of f (result of application)  
*Function declaration*: [f x = e] ==> [t0 = t1 -> t2], where t1 is domain of f (type of x) and t2 is range of f (type of e)  

**Type inference steps:**  
1. Parse program text to construct **parse tree**
2. Assign type variables to nodes (t0, t1, t2, ...) in tree
3. Generate constraints
4. Solve constraints using *unification*
5. Determine type of top-level declarations

*See example slides [19.23 ; 19.26]*

Functions may have multiple clauses, and may be recursive: in this case, to perform type checking, we have to infer separate type for each clause and add the constraint that all clauses must have the same type. For recursive calls: function has same type as its definition.  
*For examples, slides [19.32 ; 19.35]*


## ***S_20***
Haskell introduces the **Type Classes** to solve problems linked to overloading of operators (member, sort, arithmetic functions don't work for all types or can't be overloaded in specific cases).  

They :
- provide concise types to describe overloaded functions, so no exponential blow-up
- allow user to define functions using overloaded operations (e.g.: square, squares, member ...)
- allow user to declare new collections of overloaded functions: equality and arithmetic operators are not privileged buitl-ins
- generalize ML's eqtypes to arbitrary types
- fit within type inference framework

**Type classes design overview**

- Type class declaration: define a set of operations, give the set a name
- type class instance declarations: specify the implementation for a particular type
- qualified types (or type constraints): concisely express the operations required on otherwise polymorphic type

**Examples**

class Eq a where  
  (==)                  :: a -> a -> Bool

-- Here Eq is the name of the class being defined, and == is the single operation in the class. This declaration may be read "a type a is an instance of the class Eq if there is an (overloaded) operation ==, of the appropriate type, defined on it." (Note that == is only defined on pairs of objects of the same type.)

 -- Works for any type n that supports the Num operations  
 square :: Num n => n -> n  
 square x = x * x

 -- The class declaration says what the Num operations are  
 class Num a where  
   (+)    :: a -> a -> a  
   (*)    :: a -> a -> a  
   ....

-- An instance declaration for a type T says how the Num operations are implemented on T's  
instance Num Int where  
   a + b = intPlus a b  
   a * b = intTimes a b  
   negate a = intNeg a


 *A value of type (Num n) is  a dictionary of the Num operations for type n*  

 When you compile a definition, declaration and instantiation of type class:
 - the compiler translates each function that uses an overloaded symbol into a function with an extra parameter: **the dictionary**
 - references to overloaded symbols are rewritten by the compiler to lookup the symbol in the dictionary
 - the compiler converts each type class declaration into a dictionary type declaration and a set of selector functions
 - the compiler converts each instance declaration into a dictionary of the appropriate type
 - the compiler rewrites calls to overloaded functions to pass a dictionary. It uses the static, qualified type of the funcion to select the dictionary  
  
 In presence of overloading type, type inference infers a **qualified type Q => T**: T is a type inferred as usual and Q is a set of type class predicates, called constraints  
  
**Type constructor** is a generic type with one more type variable: `data Maybe a = Nothing | Just a`. In this case *Maybe a* is a possible undefined value of type a, and `f::a -> Maybe b` is a partial function from *a* to *b*.


## ***S_21***

*Problems of functional programming*: a useful program has to be able to interact with external environment (I/O, imperative update, error recovery ..) and affect it. Try to add imperative constructs "the usual way", but in what way? Using monads!

While Type Classes are predicates over types, **Type Constructor Classes** are predicates over *Type constructor*. They (TCC) allow to define overloaded functions common to several type constructors.

**Functor -> ?**

**Monads** are formally constructors classes introducing operations for putting a value into a "box" (**return**) and for compose a function that returns "boxed" values (**bind**)  
Example :
```
-- m is a type constructor and m a is the type of monadic values
class Monad m where
   return :: a -> m a
   (>>=)  :: m a -> (a -> m b) -> m -> b   -- "bind"
   (>>)   :: m a -> m b -> m b             -- "then"
   ...
   ...

-- Maybe monad
instance Monad Maybe where
   return :: a -> Maybe a
   return x = Just x
   (>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
   y >>= g= case y of
              Nothing -> Nothing
              Just x -> g x
```

+ **return**, **bind** and **then** define a basic way to compose computations
+ they are used in Haskell libraries to define more complex composition operators and control structures (sequence, for-each loops, ..)
+ if a type constructor defining a library of computations is **monadic**, one gets automatically benefit of such libraries



***WARNING!!!  
See Monads on the slides***

## ***S_22***
**Scripting languages**  

Common characteristics:  
+ Both batch and interactive use
+ economy of expression (concise syntax)
+ lack of declarations
+ simple default scoping rules
+ flexible dynamic typing: a variable can be interpreted differently depending on the context
+ easy to access to system facilities
+ sophisticated pattern matching and string manipulation
+ high level data types (storage is garbage collected)
+ quicker development cycle than industrial-quality languages

Principal use cases:  
+ shell languages
+ text processing and report generation
+ mathematics and statistics
+ "glue" languages and general purpose scripting
+ extension languages


*Shell languages*  
**sh**, **csh**, **tcsh**, **bash**..  
Provide many mechanism to manipulate file names, arguments and commands and to glue together other programs

*Text processing and report generation*  
**sed** (unix stream editor; no variables, no state; just a powerful filter) and **awk** (adds variables, state and richer control structures; also field and associative arrays).  
Used also **perl** language (meant primarly for text processing but now has grown into a large and complex language; supports separate compilation, modularization and dynamic mechanism appropriate for large scale projects)

*Mathematics and statistics*  
**maple**, **mathematica**, **mathlab**, **octave**..  
extensive support for numerical methods, symbolic mathematics, data visualization, mathematical modeling  
provide scripting languages oriented towards scientific and engineering applications

*"Glue" languages and general purpose scripting*  
**Reex**, **Perl**, **Tcl**, **Python**, **Ruby**

*Extension languages*  
An EL serves to increase the usefulness of an application by allowing the user to create new commands, generally using the existing commands as primitive.  
To admit extension, a tool must:  
- incorporate or communicate with an interpreter for a scripting language;
- provide hooks that allow scripts to call the tool's existing commands;
- allow the user to tie newely defined commands to user interface events.

**Innovative features**  
...*see slides*
