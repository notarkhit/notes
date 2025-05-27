STL is a collection of templates

-  containers
-  algorithms
-  iterators 
-  function object

## Containers

### vector
- dynamic array
- it can grow or shrink in size at runtime
- it is a part of <vector> header in STL.



| builtin function      | description     |
| ---                   | ---             |
| vec[i]                | return element at index i  |
| vec.clear()           | remove all elements  |
| vec.erase()           | remove element at pos   |
| vec.begin()           | returns iterator to first element  |
| vec.end()             | returns iterator past the last element     |


## set 
- set is an ordered container.
- stores unique elements
- automatically sorts the elements in ascending order
- is implemented as balanced binary search tree [bbst]

## function object
