- ## Why Julia
	- Terse sintax
	- Easy to write, similar to mathematics
	- Syntactic sugar
	- Less boilerplate
- ## Installing Julia
	- Installation instruction can be found at [https://julialang.org/downloads/] (windows and linux)
		- This will install the [Juliaup](https://github.com/JuliaLang/juliaup) installation manager, which will automatically install Julia and help keep it up to date. The command `juliaup` is also installed. To install different julia versions see `juliaup --help`.
	- on Ubuntu, I installed Julia through snap
		- `snap install julia`, this also keeps my installation up to date; when on a system with snap, I prefer to use the snap installer, but it is a matter of taste...
		  The biggest difference is that `juliaup` provides the `julia` executable only to one user, while `snap install julia` installs julia systemwide
	- Please remark that the fact that `julia` automatically updates sometimes can be somewhat problematic, because it leads sometimes to the fact that breaking versions of a package may be installed
- ## The Julia REPL and initial setup
	- The basic way of interfacing with Julia is the REPL, the **Read-Eval-Print Loop**
	- At a first glance, it looks like a fancy calculator
	- Before going further, I would like to install and setup the system in a way that I found comfortable, and may work for you too
	- write `julia` on the command line, I will teach you two ways of installing packages
		- ### The package prompt
			- the first one opens the shell package manager, press `]`
			- the prompt color changes to blue, we are in **Package mode**, it is used to install packages into an environment; to get back from the pkg prompt to the REPL, just press `backspace`
			- type `add OhMyREPL`, which is a package that colors the output of the REPL so it is easier to read it; remark that the REPL and the shells in Julia support `TAB`-autocomplete
		- ### Explicitly importing Pkg
			- another way to install packages is by using the `Pkg` package, that allows us to install packages, change environments and so on from inside Julia
			- in the REPL, the one with the `julia >` prompt, write `import Pkg`
			- this means that we are bringing the `Pkg` name in scope inside the REPL (but not its subnames)
			- Now, we can call `Pkg.add("BenchmarkTools")`; this installs a package to do Benchmarks
			- Since we only imported the `Pkg` name in scope, we need to specify the function using the dot notation
			- I use this form when I want to avoid polluting my namespaces (it is not really important in Julia, since it is strongly typed, but it is a good practice)
			- Instead of `import` we could have used `using Pkg` that imports the name `Pkg` and all the names exported by `Pkg`
			- We install one last package `Pkg.add("Revise")`, this package allows Julia to recompile functions on the fly when we change them in a loaded package
	- Now, for me it is a good practice to start these packages as Julia starts, therefore, we make a file
		- `nano ~/.julia/config/startup.jl` on linux
		- on windows we should do the same in `C:\Users\USERNAME\.julia\config\startup.jl` with text editor
		- add `using Revise, OhMyREPL, BenchmarkTools` and save the file `CTRL-O + CTRL-X`
		- Documentation on the startup file https://docs.julialang.org/en/v1/manual/command-line-interface/#Startup-file
- ## Installing VSCode
	- Probably VSCode is the best thing that Microsoft has done in many years (maybe forever)
		- https://code.visualstudio.com/
	- On Ubuntu
		- `snap install code`
		- On windows, we just go to the page above and download it
	- Install the Julia extension, that is going to take care of the interface between VSCode and Julia
		- If you have any problem, probably is the fact that the extension is not able to find the julia executable
		- On Ubuntu, if you installed through snap, the path is `/snap/julia/current/bin/julia`
- ## First steps in VSCode/Julia
	- We make a directory, I called it `~/Coding/HSI2025`
	- Good practice: when we work on a new project, whether its coding a package or experiments, it is good to make a new environment, so you avoid loading unnecessary packages for your actual work
	- Open VSCode, open folder, go to the folder you created
	- Press `F1` and start the Julia REPL
	- To create a new environment in the directory
	- `import Pkg; Pkg.activate("./")`
	- in the REPL we install the `Plots` package, by using `Pkg.add("Plots")`
	- Now we also change the environment for VSCode, by clicking on Julia env in the bottom state menu and choosing our working directory; this is to make VSCode aware of the packages we installed in the environment
	- Remark that using environments comes with zero cost: julia only downloads and compile one copy of the package that is then made available to the environments
- ## Compiler vs Interpreter
	- ### Compiled Languages (C, C++, Fortran, …)
		- A **compiler** translates your entire program (written in a high-level language) into **machine code** once, before execution.
		- After compilation, you can run the program many times without recompiling.
		- Advantages:
			- Runs very fast (since the machine code is already prepared).
		- Disadvantages:
			- Slower development cycle: you must compile before running, and errors are often reported only after the compilation step.
			- Code tends to be more rigid: you must carefully specify types and follow strict rules.
	- ### Interpreted Languages (Python, Perl, …)
		- An **interpreter** reads your program line by line and executes it directly, without producing a separate machine-code file.
		- Every time you run the program, the interpreter does this translation again.
		- Advantages:
			- Faster to test ideas: just write and run.
			- Error messages are often easier to understand.
			- More flexibility in coding style.
		- Disadvantages:
			- Slower execution, because every run involves the overhead of interpretation.
	- ### Julia: Just-In-Time (JIT) Compilation
		- Julia takes a **hybrid approach**.
		- The **first time** you run a function, Julia compiles it into optimized machine code using LLVM (a compiler backend).
		- That compiled version is then stored in memory (and sometimes on disk), so **subsequent runs are as fast as C or Fortran**.
		- Advantages:
			- Interactive experience like Python (you can write and test quickly).
			- Performance close to compiled languages once functions are compiled.
		- Trade-off:
			- The first run of a function may feel slower because of the compilation step (“time-to-first-plot problem” is a famous example).
- ## Types
	- An important concept, when dealing with programming is the concept of **type**
	- A **type** is a label that tells Julia (and you) what kind of data something is, like “an integer number,” “a decimal number,” or “a complex number.”
	- In practice a **type** describes both the *kind of data* (like integers, floating-point numbers, strings, arrays, functions) and how Julia should store it in memory and choose the right method to run when you call a function.
	- In Julia, everything has a type. Types tell Julia how to represent data in memory and which version of a function to run (this is called **multiple dispatch**).
- ## First use of Julia
	- In the REPL now we can use Julia to make some simple computations
	- We can use Julia as a simple calculator
		- ```julia
		  julia> 1.0 + 1.0
		  2.0
		  ```
	- It is worth starting to think about types
		- ```julia
		  julia> typeof(1.0)
		  Float64
		  ```
		- ```julia
		  julia> typeof(1)
		  Int64
		  ```
		- ```julia
		  julia>  typeof(1.0+0.3*im)
		  ComplexF64 (alias for Complex{Float64})
		  ```
	- As you can see Julia implicitly identifies the types of the objects: Julia is a typed language and under the hood it uses types to identify and dispatch the right function
	- What is the difference between Julia and Python?
		- **Just in time** compilation
		- We define a function
			- ```julia
			  T(x) = 4*x*(1-x)
			  T (generic function with 1 method)
			  ```
		- What is going to happen when we run this function?
			- ```julia
			  julia> T(0)
			  0
			  julia> @code_warntype T(0)
			  MethodInstance for T(::Int64)
			    from T(x) @ Main REPL[8]:1
			  Arguments
			    #self#::Core.Const(T)
			    x::Int64
			  Body::Int64
			  1 ─ %1 = (1 - x)::Int64
			  │   %2 = (4 * x * %1)::Int64
			  └──      return %2
			  ```
		- When we ran this function Julia identified that the input was an integer, checked if it had available all the functions needed to compile this function with an integer input and compiled a version of this function and stored it in memory (recently, it also stores them on disc, to avoid recompilation); this means that the **the first time** we ran a function Julia is going to incur in overhead, but the **second time** it is going to have access to a compiled and often optimized version of the function which is going to be really fast
		- Functions that start with an `@` are called **macros** and are functions that act on julia source code before compilations. The macro `@code_warntype`is used to analyze the julia code and identify eventual type instabilities
		- The macro `@time` computes the runtime of a function
		- In the REPL, we can access the help prompt by pressing `?`
		- We ran the example in the help
- ## Declaring more complex functions in julia
	- We open a file and add the following code
		- ```julia
		  function orbit(T, x0::Float64, N)
		     orb = zeros(Float64, N)
		     x = x0
		     for i in 1:N
		        orb[i] = x
		        x = T(x)
		     end
		     return orb
		  end
		  ```
	- We would like to be able not to worry about the input type, but types are something we can work with in Julia, to do so we use the `typeof` function
		- ```julia
		  function orbit(T, x0, N)
		     orb = zeros(typeof(x0), N)
		     x = x0
		     for i in 1:N
		        orb[i] = x
		        x = T(x)
		     end
		     return orb
		  end
		  ```
	- This is similar to what is achieved in C++ by using templates, but it is simpler to use
- ## Plotting the histogram of an orbit
	- We would like to plot the histogram of an orbit, to visualize the frequency of visits of the dynamical system  $T:[0,1]\to [0,1]$ , $T(x)=4x(1-x)$ , in different parts of the interval
	- Since we already have implemented the orbit function this is quite easy
	- We need to install the Plots package (it takes some time)
		- ```julia
		  Pkg.add("Plots")
		  using Plots
		  ```
	- Now, we define our dynamical system and compute an orbit that starts at the point $x0=0.1$
	- It is worth remarking that this is a numerical experiment
		- ```julia
		  T(x) = 4*x*(1-x)
		  v = orbit(T, 0.1, 10000)
		  plt = histogram(v, normalize = :pdf, label = "Empirical", bins = 100)
		  plt
		  ```
	- In this case the density distribution of orbits is known
	  $$g(x)=\frac{1}{\pi \sqrt{x(1-x)}}$$
	- ```julia
	  g(x) = 1/(pi*(sqrt(x*(1-x))))
	  plot!(plt, g, 0, 1, color=:orange, label = "Density")
	  ```
	- The command plot! allows us to plot on top of an already made plot
- ## Summary of the lecture
	- We installed Julia and VScode
	- We installed some basic packages
	- We used the REPL and discussed on types
	- We implemented a somewhat more complicated function
	- Made a small discussion on generic code
	- Plotted an histogram
	-