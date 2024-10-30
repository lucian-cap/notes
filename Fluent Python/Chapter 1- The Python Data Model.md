- the python data model can be thought of as a description of Python as a framework, it's the backbone of what we call Pythonic
	- formalizes the interfaces of the building blocks of the language itself
	- the Python Data Model (PDM) can be leveraged when building new classes as the interpreter will invoke special methods to perform basic object operations 
		- these special methods are always named w/a leading & trailing underscore, giving them the name "dunder methods"
- the *collections.namedtuple* class is useful for building "classes" of objects that are just bundles of attributes w/no special methods
- 2 advantages of using special methods to leverage the PDM:
	1. Users of your class don't have to memorize arbitrary method names for standard operations
	2. Easier to benefit from the Python standard library & avoid reinventing the wheel
## How Special Methods are Used
- special dunder methods are meant to be called by the Python interpreter, not by you 
	- such methods allow the interpreter to take a shortcut for certain built-in/extended classes written in C by examining a field cheaply rather than a expensive function
	- these calls are also often implicit, i.e. for i in x -> iter(x) -> x.\_\_iter\_\_() 
	- unless metaprogramming, the only dunder method commonly used directly is \_\_init\_\_()
### Emulating Numeric Types
- custom classes can be designed to work w/operators like +, -, abs(), etc. through dunder methods
	- such infix operators shouldn't modify their operands but merely read & return new objects
### String Representation
- the \_\_repr\_\_() dunder method is called by the repr() build-in to get the string representation of the object for introspection
	- the string returned by this function should be unambiguous & if possible match the source code needed to recreate it
- the \_\_str\_\_() is called by the str() built-in & should return a string suitable to display for end users
	- the \_\_repr\_\_() is called as a fallback
### Boolean Value of  a Custom Type
- by default, user defined objects are considered True unless either \_\_bool\_\_() or \_\_len\_\_ is implemented 
### Collection API
- the **Iterable**, **Sized**, & **Container** abstract base classes (ABC) each have a single dunder method
	- the **Collection** ABS (new in 3.6) unifies the 3 interfaces that every collection should implement:
		- **Iterable** to support *for*, unpacking, and other iteration
		- **Sized** to support \_\_len\_\_()
		- **Container** to support *in* operation
	- concrete classes don't have to inherit from these ABCs, any class implementing the dunder method satisfies the interface
	- 3 important specializations of **Collection**:
		1. Sequence: formalizing the interface of built-ins like *list* & *str*
		2. Mapping: *dict*, *collections.defaultdict*, etc. 
		3. Set: *set* & *frozenset*, which implement infix operators