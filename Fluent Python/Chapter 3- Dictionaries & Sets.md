- Pythons *dicts* are highly optimized, hash tables are the engines behind them
## Modern *dict* Syntax
- ### *dict* comprehensions
	- a "dictcomp" builds a dict instance by taking key: value pairs from an iterable
- ### Unpacking Mappings
	- since Python 3.5 the \*\* operator can be applied to more than one argument in a function call (if keys are all strings & unique)
	- the \*\* operator can be used multiple times in a *dict* literal
- ### Merging Mappings w/ | 
	- Python 3.9 supports using | & |= to merge mappings
## Pattern Matching w/Mappings
- the **match/case** statement supports subjects that are mapping objects
	- mapping patterns succeed on partial matches, unlike sequence patterns
## Standard API of Mapping Types
- *collections.abc* provides the **Mapping** & **MutableMapping** ABCs describing the interfaces of **dict** & similar types
- all concrete mapping classes the the standard library share the limitation that the keys must be hashable
- ### What Is Hashable
	- an object is hashable if it has a hash code which never changes during its lifetime & can be compared to other objects, hashable object evaluated as equal must have the same hash code
	- the hash code is retrieved via its *\_\_hash\_\_()* method
	- the hash code of an object may change depending on the architecture of the machine, the version of Python used, and some security reasons thus the hash code is only guaranteed to be constant within one Python process
- ### Overview of Common Mapping Methods
	- the way **dict.update(m)** handles it's 1st arg is a prime example of *duck typing*: it 1st checks whether the arg has a **keys** method if if so assumes it's a mapping
		- otherwise it falls back to iterating over the argument assuming its items are (*key*, *value*) pairs
	- ### Inserting or Updating Mutable Values
		- **dict** access with d\[k\] raises a KeyError when *k* is not a existing key, the **get(*k*, default)** method is a easy alternative to handling the error
		- the **setdefault()** method is similarly useful for retrieving a mutable value when the end goal is to update it
## Automatic Handling of Missing Keys
- there are two main approaches to have a mapping that returns a made-up value when a missing key is searched:
	1. use a **defaultdict**
	2. subclass **dict** and add a \_\_missing\_\_ method
- ### defaultdict: Another Take on Missing Keys
	- creates items with a default value on demand whenever a missing key is search using **d\[k\]** syntax
	- it works by providing a callable when instantiating the object to provide a default value when **\_\_getitem\_\_()** is called
		- if no default is provide the usual KeyError is raised
- ### The \_\_missing\_\_ Method
		- not default in the base **dict** class but its \_\_getitem\_\_() method will call it whenever a key is not found instead of raising KeyError
		- looking up keys in a dictionary is faster using "k in d" than "k in d.keys()" b/c it avoid the attribute lookup to find the **.keys()** method
## Variations of dict
- ### collections.OrderedDict
	- the built-in **dict** keeps the keys ordered since Python 3.6
	- designed to be good at reordering operations, space efficiency, iteration speed, & performance of an update were secondary 
- ### collections.ChainMap
	- holds a list of mappings that can be searched as one
	- the lookup is performed on each input mapping in the order it appears in the constructor call and succeeds as soon as the key is found in one of those mappings
	- doesn't copy the input mappings
- ### collections.Counter
	- a mapping that holds an integer count for each key, updating an existing key adds to its count
- ### shelve.Shelf
	- provides persistent storage for a mapping of string keys to Python object serialized in the *pickle* binary format
	- provide a few additional I/O methods on top of **abc.MutableMapping**
	- keys & values are saved whenever a new value is assigned to a key
	- keys must be strings
	- values must be objects that pickle can serialize
- ### Subclassing UserDict instead of dict
	- better to create a new mapping type by extending **collections.UserDict** rather than **dict**
		- this is b/c the **dict** built-in has some implementation shortcuts that we have to override whereas using **UserDict** allows us to just inherit 
## Immutable Mappings
- the **types** module provides a wrapper class called *MappingProxyType* that given a class returns a *mappingproxy* object that is read-only but a dynamic proxy for the original mapping (reflects changes made to the original, but can't be used to make the changes)
## Dictionary Views
- **dict** instance methods like *.keys()*, *.values()*, & *.items()* return dictionary views that are read-only projections of the internal data structures used in the **dict** implementation
	- a view object is a dynamic proxy
## Practical Consequences of How dict works
- keys must be hashable i.e. implement proper \_\_hash\_\_() & \_\_eq\_\_() methods
- key ordering is preserved as of 3.7
- significant memory overhead, need at least 1/3 of the hash table rows to be empty to remain efficient
- avoid creating instance attributes outside of the \_\_init\_\_() method
	- this forces Python to create a new hash table just for that one instance instead of using the common class hash table w/pointers for its own attributes
	- this optimization can reduce memory use by 10-20% for OOP
## Set Theory
- a set is a collection of unique objects
- set elements must be hashable, though the **set** type is not hashable itself (**frozenset** is however)
- implements many set operations as infix operators, such as union ( **|** ), intersection (**&**), difference (**-**), & symmetric difference (**^**)
- ### Set Literals
	- set literals use curly brace notation, however there's no literal notation for the empty set so we must use **set()**
	- literal set syntax is faster & more readable than calling the constructor b/c Python runs  specialized bytecode for construction
## Practical Consequences of How Sets Work
- **set** & **frozenset** are implemented with a hash table
- elements must be hashable
- membership testing is very efficient 
- significant memory overhead
- element ordering depends on insertion order, but not in a useful or reliable way
- adding elements to a set may change the order of existing elements