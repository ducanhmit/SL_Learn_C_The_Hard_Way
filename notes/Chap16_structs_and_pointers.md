
Describe this program:
- **struct Person**: creating a structure that has 4 elements to describe a person. The final result is a new compound type that lets us reference these elements all as one, or each piece by name. It's similar to a row of a database table or a class in OOP language.
- **function Person_create**: We need a way to create these structures so I've made a function to do that. Here's the important things this function is doing:
    1. We use *malloc* for "memory allocate" to ask the OS to give us a piece of raw memory
    2. We pass to *malloc* the *sizeof(struct Person)* which calculates the total size of the struct, given all the fields inside it.
    3. We use *assert* to make sure that I have a valid piece of memory back from malloc. 