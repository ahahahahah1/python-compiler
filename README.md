# HDS COMPILER

The following repository holds the code for the end-to-end implementation of a compiler designed to compile Python scripts into assembly language. This project was undertaken as a part of the coursework for [CS335: Compiler Design](https://www.cse.iitk.ac.in/users/swarnendu/courses/spring2024-cs335/) at IIT Kanpur, under the guidance of [Prof. Swarnendu Biswas](https://www.cse.iitk.ac.in/users/swarnendu/). The project was completed by a team of three, which included me, [Harsh Bihany](https://github.com/bihany-harsh) and [Danish Mehmood](https://github.com/danx069) (hence the name).

### Features overview

The compiler works on a statically-typed subset of python (type hints are to be provided using [PEP484 conventions](https://peps.python.org/pep-0484/)) and follows [python3.8 grammar](https://docs.python.org/3.8/reference/grammar.html#full-grammar-specification). Lexical scoping is followed for variable use (similar to C/C++). The compiler throws relevant error messages (unless the user provides something extraordinarily unexepected). Some example testcases can be viewed in `./tests/`.

* Supports for integer (`int`), boolean (`bool`), floating point (`float`) and string (`str`) data types with coercion support between int, float and bool. Note that the support for floats is only up to the generation of the intermediate representation (Three Address Code) and x86 instructions are not generated for floats as per the scope of the project (although it is not hard to extend this implementaion for the generation of x86 code for floats).
* 1-D lists over these base data types as well as over objects of classes with relevant type-checking. Dictionaries and tuples not supported.
* All basic arithmetic, relational, logical, bitwise and augmented assigment operators supported.
* Basic language features like the `if-elif-else`, `while` and `for` loops supported. Note that the for loop supported is only of the type:

```python
for i in range(a):
	# code
```

```python
for i in range(a, b):
	# code
```

* Recursive methods supported.
* Support the library function `print()` for only printing the primitive Python types, one at a time.
* Object-oriented features include support of classes and objects with a `__init__` method, including multilevel inheritance and constructors, with their method calls supported and also access to members . Multiple inheritances (more than one parent class) are not supported.
* Static polymorphism via method overloading (an implementaion synonymous to that of C++).
* Multiple function calls can be executed within a single line.
* Can return lists and class objects from functions and can also take them as arguments to function calls.
* Support for the `len(array)` within well-defined scopes.
* The compiler requires the following block:

  ```python-repl
  if __name__ == "__main__":
  	# your function calls.
  ```
  as that is treated as the entry point for the program (similar to how `main()` is treated in C). The compiler also expects a newline at the end of the input file.
* Do note that format specifiers within print statements are not supported and neither are sequence of object receivers. `import` statements are not supported, no string operations except string comparison.
* Lambda functions, nested functions/classes are not supported.

---



### Requirements

Ensure that `flex`, `bison` are installed along with the `gcc` compiler.

### Working

```bash
git clone git@github.com:ahahahahah1/python-compiler.git
cd src
make
```

This generates the executable `pycompiler.o`. The python script can then be compiled using

```bash
./pycompiler.o -i ../tests/test1.py -o graph.dot -a asm1.s -s sym_tab.csv -t tac.txt -v
```

The above command uses the complete set of flags (you can run `./pycompiler -h` for information on the available flags). You can omit any subset of these flags and their output will be redirected to a file with a default filename. The generated asm can be run using

```bash
./pyrun.sh asm1.s
```

You can optionally provide the .dot file to generate the AST (abstract symbol table) of the python script, in a .pdf file, for example:

```bash
./pyrun.sh asm1.s graph.dot 
```

### Background
The project was completed in three milestones. The implementation goals of each milestone were as follows:
* **Milestone 1**: To develop the scanner and the parser for the given features of python. The deliverable for this milestone was to implement the actions to generate an Abstract Syntax Tree (AST) for the given input testcase.
* **Milestone 2**: To implement the necessary actions to generate the symbol and intermediate representation (three address code was chosen by us) for the given input testcase.
* **Milestone 3**: To incorporate the scripts to generate the x86 code for the given testcase.

These tasks were completed over the course of roughly two and a half months, with each checkpoint being formally checked by a TA (teaching assistant, a PhD student) via a demo and private testcases. This was quite a rigorous project, considering the fact that none of us had previously completed any course related to compiler design we learned the necessary theory in the classroom for features while we implemented them, alongside other demanding coursework.

We obtained 100/100 in milestones 1 & 2 and 95/100 in milestone 3. A minor testcase was missed- the action for adding `return` to the 3AC for a very specific case was missing (this error has been corrected now).

---
Trivia: Due to the rigorous nature of this course and the fact that it resulted in the degree extension for many students, this course was discontinued as a compulsory course for the computer science program and we were a part of the last batch to take this up as a compulsory course.
