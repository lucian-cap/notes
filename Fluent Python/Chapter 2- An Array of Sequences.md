## Overview of Built-in Sequences
- 2 types of sequences implemented in C:
	- container: holds items of different types
	- flat: hold items of one type
- container sequences hold references to the objects it contains, flat sequences store the value of its contents in its own memory space
- flat sequences are more compact but limited to holding primitive machine values
	- every Python object in memory has a header w/metadata, container sequences w/objects of the same type repeat this metadata per value whereas flat sequences store it once
- Mutability
	- mutable sequences: *list*, *bytearray*, *array.array*, & *collections.deque* 
	- immutable sequences: *tuple*, *str*, & *bytes*
- mutable sequences inherit all methods from immutable sequences & implement several additional methods
	- built-in concreate sequence types don't subclass the **Sequence** & **MutableSequence** ABCs but are virtual subclasses registered w/those ABCs
## List Comprehensions & Generator Expressions
- list comprehension is a explicit way of generating a new list
- variables have local scope inside the expression unless assigned w/the Walrus Operator ":="
	- the score of these variables is the enclosing function
- listcomps do every map & filter do but faster & w/out managing a lambda function
- ### Generator Expressions
	- a genexp saves memory b/c it yields items one by one using the iterator protocol
	- same syntax as listcomps but enclosed in parentheses 
## Tuples Are Not Just Immutable Lists
- useful not just as immutable lists but also as records w/no field names
- each item in the tuple holds data for one field w/the position giving the field its meaning
- ### Tuples as immutable lists:
	- clear b/c when we see a tuple we know it's length will never change
	- better performance b/c it uses less memory than a list & has some optimizations
	- tuple immutability applies only to the references it contains, no the objects referenced
		- the built-in **hash()** function can be used to explicitly determine if a tuple has a fixed value
	- tuples support all list methods not involving adding/removing items (except \_\_reversed\_\_() but just for optimization, **reversed()** still works)
## Unpacking Sequences & Iterables
- important b/c it avoids unnecessary & error-prone index based extraction
- works w/an iterable object as the data source, only requires 1 item per variable in the receiving end
- most visible form of unpacking is parallel assignment, i.e. assigning items from an iterable to a tuple of variables
- ### Using \* to grab excess items:
	- during parallel assignment the \* operator can appear before exactly one variable at any position to receive excess values
- ### Unpacking w/\* in function calls & sequence literals
	- when defining function parameters the \* operator can be used on a parameter to unpack incoming args
	- similarly the \* operator can be used when calling a function to unpack an iterable as separate args
	- it can also be used when defining *list*, *tuple*, or *set* literals
- ### Nested Unpacking
	- the target of an unpacking can use nesting & Python will correctly parse this if the value has the same nesting structure
## Pattern Matching w/ Sequences
- Python 3.10 introduced pattern matching w/*match* & *case* statement
	- the expression after the *match* keyword is the subject
	- *match*/*case* may look like switch/case but improve over switch via **destructuring**, a more advanced unpacking
- a sequence pattern matches the subject if:
	1. the subject is a sequence
	2. the subject & pattern have the same # of items 
	3. each corresponding item matches, including nested items
	- a case clause has 2 parts: a pattern & optional guard w/ the *if* keyword
- sequence patterns can be *tuples* or *lists*, but it makes no difference b/c parentheses & brackets mean the same
- can match instances of most actual or virtual subclasses of **collections.abc.Sequence** w/the exception of *str*, *bytes*, & *bytearray*
- type info can be added to patterns to make them more specific
- the \*_ collector matches any # of items w/out binding them to a variable
- the optional guard is evaluated only if the case is matched
## Slicing
- ### Why Slices & Ranges Exclude the Last Item
	- convienent features of the convention:
		- easy to see the length when only the stop position is given
		- easy to compute the length when start and stop are given via (stop-start)
		- easy to split on a index w/out overlapping
- ### Slice Objects
	- notation a:b:c is only valid when used w/in \[\] as the indexing operator & produces a slice object: slice(a, b, c)
- ### Multidimensional Slicing & Ellipses
	- the \[\] operator can take multiple indexes or slices separates by commas
	- the ellipsis, ..., is an alias to the Ellipsis object & can be passed as an argument to functions and slice specifications
- ### Assigning to Slices
	- mutable sequences can be modified in place using slice notation or as the target of the **del** statement
## Using + & \* w/Sequences
- typically expect + & \* to work w/sequences given both operands are of the same type, neither is modified, & a new sequence of that type is created
- ### Building Lists of Lists
	- beware of expressions like "a \* n" when *a* is a sequence w/mutable items as new objects won't be created but new references will be
- ### Augmented Assignment w/Sequences
	- the dunder method feeding += is \_\_iadd\_\_ (in-place addition)
		- if \_\_iadd\_\_ isn't implemented  \_\_add\_\_ is called
	- the identify of the object being assigned to may or may not change then depending on which is implemented/called
## list.sort vs sorted Built-In
- **list.sort** sorts a list in-place
-  functions/methods that change an object in place should return None to make it clear to the caller the receiver was changed & no new object was created
-  built-in sorted creates & returns a new list
	- it accepts any iterable object
## When a List Is Not The Answer
- if frequently checking if a item is present consider a **set**, especially for larger # of items
- ### Arrays
	- if a list stores only numbers an **array.array** is more efficient
	- supports all mutable sequence operations & additional methods for faster loading and saving
	- when creating an array a type code is used to determine the C type used for each item in the array
		- Python won't let you put a differently typed variable in the array
	- doesn't support in-place sorting, must use **sorted()** function & **bisect.insort** to keep sorted while appending
- ### Memory Views
	- shared memory sequence type that handles slices of arrays w/out copying bytes
	- the **memoryview.cast** method changes the way multiple bytes are read/written as units w/out moving bits around
		- returns another memory view object sharing the same memory
- ### NumPy
	- implements multidimensional, homogenous arrays & matrix types & provide efficient element-wise operations
	- SciPy is a library written atop NumPy offering scientific computing algorithms from linear algebra, numerical calculus, & stats
		- fast & reliable b/c it leverages the C & Fortran Netlib Repository
- ### Deques & Other Queues
	- the **collections.deque** class is a thread-safe double-ended queue designed for fast inserting & removing from both ends
	- can be created w/a fixed maximum length
		- if full when a new item is appended it discards an item from the opposite end
	- removing items from the middle is not as fast as a list