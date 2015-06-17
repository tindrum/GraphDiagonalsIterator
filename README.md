# GraphDiagonalsIterator
From a two-dimensional "array" of objects (of template type &lt;T>), iterates through rows, columns, and diagonals (left and right), like a word-search player would look for words in a puzzle. Uses C++, classes and templates

The first technical challenge is that template stuff in C++ must be in one file only. Something about the compiler not being able to statically make classes when the implementation is in a separete .cpp file. Everything goes in the .h file.
That's not a big deal, just something to remember.

Second technical challenge is that an iterator is a separate object, so I need to figure out what methods are required to make at least a forward iterator. What instance vars, methods, &c need to be there for an iterator of an object to be legit in C++.

Third technical challenge is the biggest.
The class, as currently implemented, is conceptually a rectangular "grid" of the same kind of object (simple integers for now).
Internally to the class, it's stored as a vector of vector<template TypeT>'s, like this:
    grid[0] = [ 3, 4, 6, 3]
    grid[1] = [ 2, 5, 2, 1]
    grid[2] = [ 8, 7, 1, 0]
    grid[3] = [ 3, 7, 2, 7]
Iterating through the first four (the first "height" elements) is easy: just return the vector of objects at grid[count].
Iterating through the next four (the next "rows" elements) is kinda easy: build a vector from element (count - height) element of each vector in grid[].
Next, though, we need the diagonals, which are NOT the same length. 
The simpler approach, if a null object of type TypeT (or an integer zero, which will work ok for the first project to use this class), is to add N to 0 extra elements to the end of grid[0] (N is 'height' items - 1), and add 0 to (N - 1) elements to the front:
    gridDiagonalRight[0] = [ 3, 4, 6, 3, 0, 0, 0]
    gridDiagonlaRight[1] = [ 0, 2, 5, 2, 1, 0, 0]
    gridDiagonalRight[2] = [ 0, 0, 8, 7, 1, 0, 0]
    gridDiagonalRight[3] = [ 0, 0, 0, 3, 7, 2, 7]
From this new grid-like thing, we get the vertical rows, and it is essentially the diagonals.

Then we need to do a gridDiagonalLeft in a similar way, and take the verticals.

The first problem is that, if the original object changes, the new grids will be incorrect.
Solution: don't allow mutability.
Other solution: make the grid out of pointers to the items, so the items can change, but make sure items are not replaced, just updated. 
The second problem is that this duplicates a lot of the data, and I'm not sure where it should be duplicated.
Should the iterator copy all the Grid object's into itself, make the new vertical grid and diagonal grids, and generate all the required vectors of <TypeT> when it's first called? This would be simple, but would bloat the data, especially on large grids or grids of large objects.

This may be a good way to go, though, if an iterator doesn't need to point into its 'parent' objects data and can have its own data to work with.

From what I've read, iterators, point into the original object, but you can see how there is no 'original' diagonal rows and whatnot.
