# Studio 15

## Parameter Packs and Variadic Templates

In this studio, you will use parameter packs and their expansions to write variadic function templates, whose parameterized types (and argument lists) are not concretely specified until the templates are instantiated at compile time. You will see how recursive definitions of variadic function templates, and applying type patterns and functions when parameter packs are expanded, let you define powerful and nuanced functionality.

## Collaboration

You may complete this studio individually or in a small group.

## Exercises

Record your answers in `ANSWERS.md` as you work. Include the names of everyone who worked on the studio in your first answer, and number your responses so they are easy to match to the exercises.

1. List the names of the people who worked together on this studio.

2. SSH into `shell.cec.wustl.edu` using your WUSTL Key credentials, then use `qlogin` to connect to one of the Linux Lab machines and confirm that the version of `g++` there is correct, as you did in [Studio 0](https://github.com/cse4208-wustl/studio0).

   Clone your `studio15` repo using SSH:

   ```bash
   git clone git@github.com:cse4208-wustl/studio15.git
   cd studio15
   ```

   The repo already includes a `Makefile` that builds an executable named `studio15`. Copy the source and header files from your [Studio 14](https://github.com/cse4208-wustl/studio14) solution into this repo, and rename them as appropriate (for example, `studio14.h` to `studio15.h` and `studio14.cpp` to `studio15.cpp`). Update the `Makefile` so it lists all of your source and header files.

   Remove the contents of your program's `main` function, except for the statement that returns a descriptively named symbol with value 0 to indicate success. Above that statement, declare and initialize variables of the following types:

   1. a `std::string` initialized with a C-style string
   2. a `const char *` initialized with a different C-style string
   3. an object of the class type you declared and defined in the previous studio, initialized with a `std::string` that is initialized with yet another C-style string
   4. an `int` initialized with an integer value
   5. a `float` initialized with a numeric value that has a fractional part

   Pass each of these variables into a separate call to your function template that prints it to `std::cout`. Compile and run your program. In your answers, show the output that your program produced.

3. In your template header and source files, declare and define a variadic overload of your function template that:

   1. has template type parameters consisting of a single template type parameter followed by a template parameter pack
   2. has a `void` return type
   3. takes a reference to a `std::ostream`, a const reference to an argument of the first template parameter type, and a function parameter pack of const references to the types in the template parameter pack (that is, wrapping the template parameter pack with `const` and `&`, as in `const Args&... rest`)

   The function template should insert `__PRETTY_FUNCTION__` into the `std::ostream`, followed by a space, the first argument, and a newline, and then call itself recursively with the reference to the `std::ostream` followed by the function parameter pack. (`__PRETTY_FUNCTION__` is a `g++` and `clang` extension that holds the full signature of the function it appears in, including the deduced template arguments.)

   Then modify your `main` function so that, instead of calling the original non-variadic function multiple times, it passes all of the variables it declared into a single call to the variadic template. Compile and run your program. In your answers:

   1. show your declaration and definition of the variadic template, and the line in `main` that calls it
   2. show the output your program produced
   3. explain how the deduced types of `T` and `Args` shown by `__PRETTY_FUNCTION__` change from one recursive call to the next, and why the last argument was printed by a different function

4. In your template header and source files, declare and define another variadic template with a different name that:

   1. has only a template parameter pack in its template type parameter list
   2. has a `void` return type
   3. takes a reference to a `std::ostream` and a function parameter pack of const references to the types in the template parameter pack (again, as in `const Args&... rest`)

   This function template should use the `sizeof...` operator to print a line to the `std::ostream` saying how many elements are in the function parameter pack that was passed into it, and then pass the `std::ostream` and the function parameter pack into a call to the variadic template you defined in the previous exercise.

   In `main`, replace the call to the variadic template from the previous exercise with a call to the one from this exercise. Compile and run your program. In your answers, show the output your program produced.

5. In your template source file, declare a global static unsigned integer variable that is initialized to 0.

   In your template header and source files, declare and define another function template that has a single type parameter, takes a reference to a const variable of the parameterized type as its only argument, and returns a reference to a const variable of the parameterized type. This function template should use the plain `sizeof` operator to determine the size in bytes of the argument passed to it, add that amount to the global static unsigned integer variable, and then return the variable that was passed into it.

   Modify the definition of the variadic template from the previous exercise so that it applies this new function template to each element of the function parameter pack when it calls the variadic template from exercise 3. After that call, it should print a line stating how many bytes were contained in the function parameter pack that was passed into it (based on the value of the global static unsigned integer variable at that point).

   Compile and run your program. In your answers, show the output your program produced.

6. In this exercise you will use a variadic template for something other than printing: a wrapper that can call *any* function with *any* arguments. The standard library uses the same technique in `std::thread`, `std::invoke`, and `std::async`.

   In the source file for your `main` function, above `main`, declare and define three non-template functions with different signatures:

   1. one that takes no parameters and returns an `int`
   2. one that takes an `int` and a `float` and returns their product as a `float`
   3. one that takes a reference to a const `std::string` and an `unsigned int` *n*, and returns a `std::string` containing *n* copies of the string

   In your template source file, declare another global static unsigned integer variable that is initialized to 0, to count calls. In your template header and source files, declare and define a variadic function template named `call_counted` that:

   1. has a single template type parameter `F` for the function to call, followed by a template parameter pack
   2. has an `auto` return type
   3. takes a parameter `f` of type `F` and a function parameter pack of const references to the types in the template parameter pack (as in `const Args&... args`)
   4. increments the call counter and then returns the result of calling `f` with the expanded function parameter pack (`f(args...)`)

   In `main`, use `call_counted` to call each of your three functions with appropriate arguments, and print each result to `std::cout`. Finally, print the value of the call counter. Compile and run your program. In your answers, show your declaration and definition of `call_counted` and the output your program produced.

   Next, add a call that passes `call_counted` your `int` and `float` function, but with your `std::string` variable in place of the `float` argument. **Before compiling**, predict whether this will compile, and if not, where the error will be reported. Then compile. In your answers, show the error and explain when and where the type mistake was caught. Then remove that call.

   Finally, in 2-3 sentences, explain what you would need to write to get the same behavior as `call_counted` without parameter packs, and why a function that takes a non-const reference parameter (for example, `void reset(int& x)`) cannot be called through your `call_counted`.

## Deliverables

Commit and push all modified and added files, including `ANSWERS.md`, to the repo.
