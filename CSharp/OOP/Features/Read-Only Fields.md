- Closely related to constant data is the notion of read-only field data (which should not be confused with a read-only property). Like a constant, a read-only field cannot be changed after the initial assignment or you will receive a compile-time error. However, unlike a constant, the value assigned to a read-only field can be determined at runtime and, therefore, can legally be assigned within the scope of a constructor but nowhere else.  
- This can be helpful when you do not know the value of a field until runtime, perhaps because you need to read an external file to obtain the value but want to ensure that the value will not change after that point.
- Again, any attempt to make assignments to a field marked readonly outside the scope of a constructor results in a compiler error.