- With the introduction of generics, the C# default keyword has been given a dual identity. In addition to its use within a switch construct, it can be used to set a type parameter to its default value. This is helpful because a generic type does not know the actual placeholders up front, which means it cannot safely assume what the default value will be. The defaults for a type parameter are as follows:  
	- Numeric values have a default value of 0.  
	- Reference types have a default value of null.  
	- Fields of a structure are set to 0 (for value types) or null (for reference types).
