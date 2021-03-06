.. currentmodule:: Base

************
 Essentials
************

Introduction
------------

The Julia standard library contains a range of functions and macros appropriate for performing scientific and numerical computing, but is also as broad as those of many general purpose programming languages.  Additional functionality is available from a growing collection of available packages. Functions are grouped by topic below.

Some general notes:

* Except for functions in built-in modules (:mod:`~Base.Pkg`, :mod:`~Base.Collections`,
  :mod:`~Base.Test` and :mod:`~Base.Profile`), all functions documented here are directly available for use in programs.
* To use module functions, use ``import Module`` to import the module, and ``Module.fn(x)`` to use the functions.
* Alternatively, ``using Module`` will import all exported ``Module`` functions into the current namespace.
* By convention, function names ending with an exclamation point (``!``) modify their arguments.  Some functions have both modifying (e.g., ``sort!``) and non-modifying (``sort``) versions.

Getting Around
--------------

.. function:: exit([code])

   .. Docstring generated from Julia source

   Quit (or control-D at the prompt). The default exit code is zero, indicating that the processes completed successfully.

.. function:: quit()

   .. Docstring generated from Julia source

   Quit the program indicating that the processes completed successfully. This function calls ``exit(0)`` (see :func:`exit`).

.. function:: atexit(f)

   .. Docstring generated from Julia source

   Register a zero-argument function ``f()`` to be called at process exit. ``atexit()`` hooks are called in last in first out (LIFO) order and run before object finalizers.

.. function:: atreplinit(f)

   .. Docstring generated from Julia source

   Register a one-argument function to be called before the REPL interface is initialized in interactive sessions; this is useful to customize the interface. The argument of ``f`` is the REPL object. This function should be called from within the ``.juliarc.jl`` initialization file.

.. function:: isinteractive() -> Bool

   .. Docstring generated from Julia source

   Determine whether Julia is running an interactive session.

.. function:: whos([io,] [Module,] [pattern::Regex])

   .. Docstring generated from Julia source

   Print information about exported global variables in a module, optionally restricted to those matching ``pattern``\ .

   The memory consumption estimate is an approximate lower bound on the size of the internal structure of the object.

.. function:: Base.summarysize(obj; exclude=Union{Module,Function,DataType,TypeName}) -> Int

   .. Docstring generated from Julia source

   Compute the amount of memory used by all unique objects reachable from the argument. Keyword argument ``exclude`` specifies a type of objects to exclude from the traversal.

.. function:: edit(file::AbstractString, [line])

   .. Docstring generated from Julia source

   Edit a file optionally providing a line number to edit at. Returns to the julia prompt when you quit the editor.

.. function:: edit(function, [types])

   .. Docstring generated from Julia source

   Edit the definition of a function, optionally specifying a tuple of types to indicate which method to edit.

.. function:: @edit

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls the ``edit`` function on the resulting expression.

.. function:: less(file::AbstractString, [line])

   .. Docstring generated from Julia source

   Show a file using the default pager, optionally providing a starting line number. Returns to the julia prompt when you quit the pager.

.. function:: less(function, [types])

   .. Docstring generated from Julia source

   Show the definition of a function using the default pager, optionally specifying a tuple of types to indicate which method to see.

.. function:: @less

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls the ``less`` function on the resulting expression.

.. function:: clipboard(x)

   .. Docstring generated from Julia source

   Send a printed form of ``x`` to the operating system clipboard ("copy").

.. function:: clipboard() -> AbstractString

   .. Docstring generated from Julia source

   Return a string with the contents of the operating system clipboard ("paste").

.. function:: reload(name::AbstractString)

   .. Docstring generated from Julia source

   Force reloading of a package, even if it has been loaded before. This is intended for use during package development as code is modified.

.. function:: require(module::Symbol)

   .. Docstring generated from Julia source

   This function is part of the implementation of ``using`` / ``import``\ , if a module is not already defined in ``Main``\ . It can also be called directly to force reloading a module, regardless of whether it has been loaded before (for example, when interactively developing libraries).

   Loads a source files, in the context of the ``Main`` module, on every active node, searching standard locations for files. ``require`` is considered a top-level operation, so it sets the current ``include`` path but does not use it to search for files (see help for ``include``\ ). This function is typically used to load library code, and is implicitly called by ``using`` to load packages.

   When searching for files, ``require`` first looks in the current working directory, then looks for package code under ``Pkg.dir()``\ , then tries paths in the global array ``LOAD_PATH``\ .

.. function:: Base.compilecache(module::ByteString)

   .. Docstring generated from Julia source

   Creates a precompiled cache file for module (see help for ``require``) and all of its dependencies. This can be used to reduce package load times. Cache files are stored in ``LOAD_CACHE_PATH[1]``, which defaults to ``~/.julia/lib/VERSION``. See :ref:`Module initialization and precompilation <man-modules-initialization-precompilation>` for important notes.

.. function:: __precompile__(isprecompilable::Bool=true)

   .. Docstring generated from Julia source

   Specify whether the file calling this function is precompilable. If ``isprecompilable`` is ``true``\ , then ``__precompile__`` throws an exception when the file is loaded by ``using``\ /``import``\ /``require`` *unless* the file is being precompiled, and in a module file it causes the module to be automatically precompiled when it is imported. Typically, ``__precompile__()`` should occur before the ``module`` declaration in the file, or better yet ``VERSION >= v"0.4" && __precompile__()`` in order to be backward-compatible with Julia 0.3.

   If a module or file is *not* safely precompilable, it should call ``__precompile__(false)`` in order to throw an error if Julia attempts to precompile it.

.. function:: include(path::AbstractString)

   .. Docstring generated from Julia source

   Evaluate the contents of a source file in the current context. During including, a task-local include path is set to the directory containing the file. Nested calls to ``include`` will search relative to that path. All paths refer to files on node 1 when running in parallel, and files will be fetched from node 1. This function is typically used to load source interactively, or to combine files in packages that are broken into multiple source files.

.. function:: include_string(code::AbstractString, [filename])

   .. Docstring generated from Julia source

   Like ``include``\ , except reads code from the given string rather than from a file. Since there is no file path involved, no path processing or fetching from node 1 is done.

.. function:: include_dependency(path::AbstractString)

   .. Docstring generated from Julia source

   In a module, declare that the file specified by ``path`` (relative or absolute) is a dependency for precompilation; that is, the module will need to be recompiled if this file changes.

   This is only needed if your module depends on a file that is not used via ``include``\ . It has no effect outside of compilation.

.. function:: apropos(string)

   .. Docstring generated from Julia source

   Search through all documention for a string, ignoring case.

.. function:: which(f, types)

   .. Docstring generated from Julia source

   Returns the method of ``f`` (a ``Method`` object) that would be called for arguments of the given ``types``\ .

   If ``types`` is an abstract type, then the method that would be called by ``invoke`` is returned.

.. function:: which(symbol)

   .. Docstring generated from Julia source

   Return the module in which the binding for the variable referenced by ``symbol`` was created.

.. function:: @which

   .. Docstring generated from Julia source

   Applied to a function call, it evaluates the arguments to the specified function call, and returns the ``Method`` object for the method that would be called for those arguments. Applied to a variable, it returns the module in which the variable was bound. It calls out to the ``which`` function.

.. function:: methods(f, [types])

   .. Docstring generated from Julia source

   Returns the method table for ``f``\ .

   If ``types`` is specified, returns an array of methods whose types match.

.. function:: methodswith(typ[, module or function][, showparents])

   .. Docstring generated from Julia source

   Return an array of methods with an argument of type ``typ``\ . If optional ``showparents`` is ``true``\ , also return arguments with a parent type of ``typ``\ , excluding type ``Any``\ .

   The optional second argument restricts the search to a particular module or function.

.. function:: @show

   .. Docstring generated from Julia source

   Show an expression and result, returning the result.

.. function:: versioninfo([verbose::Bool])

   .. Docstring generated from Julia source

   Print information about the version of Julia in use. If the ``verbose`` argument is ``true``\ , detailed system information is shown as well.

.. function:: workspace()

   .. Docstring generated from Julia source

   Replace the top-level module (``Main``\ ) with a new one, providing a clean workspace. The previous ``Main`` module is made available as ``LastMain``\ . A previously-loaded package can be accessed using a statement such as ``using LastMain.Package``\ .

   This function should only be used interactively.

.. data:: ans

   A variable referring to the last computed value, automatically set at the
   interactive prompt.

All Objects
-----------

.. function:: is(x, y) -> Bool
              ===(x,y) -> Bool
              ≡(x,y) -> Bool

   .. Docstring generated from Julia source

   Determine whether ``x`` and ``y`` are identical, in the sense that no program could distinguish them. Compares mutable objects by address in memory, and compares immutable objects (such as numbers) by contents at the bit level. This function is sometimes called ``egal``\ .

.. function:: isa(x, type) -> Bool

   .. Docstring generated from Julia source

   Determine whether ``x`` is of the given ``type``\ .

.. function:: isequal(x, y)

   .. Docstring generated from Julia source

   Similar to ``==``\ , except treats all floating-point ``NaN`` values as equal to each other, and treats ``-0.0`` as unequal to ``0.0``\ . The default implementation of ``isequal`` calls ``==``\ , so if you have a type that doesn't have these floating-point subtleties then you probably only need to define ``==``\ .

   ``isequal`` is the comparison function used by hash tables (``Dict``\ ). ``isequal(x,y)`` must imply that ``hash(x) == hash(y)``\ .

   This typically means that if you define your own ``==`` function then you must define a corresponding ``hash`` (and vice versa). Collections typically implement ``isequal`` by calling ``isequal`` recursively on all contents.

   Scalar types generally do not need to implement ``isequal`` separate from ``==``\ , unless they represent floating-point numbers amenable to a more efficient implementation than that provided as a generic fallback (based on ``isnan``\ , ``signbit``\ , and ``==``\ ).

.. function:: isless(x, y)

   .. Docstring generated from Julia source

   Test whether ``x`` is less than ``y``\ , according to a canonical total order. Values that are normally unordered, such as ``NaN``\ , are ordered in an arbitrary but consistent fashion. This is the default comparison used by ``sort``\ . Non-numeric types with a canonical total order should implement this function. Numeric types only need to implement it if they have special values such as ``NaN``\ .

.. function:: ifelse(condition::Bool, x, y)

   .. Docstring generated from Julia source

   Return ``x`` if ``condition`` is ``true``\ , otherwise return ``y``\ . This differs from ``?`` or ``if`` in that it is an ordinary function, so all the arguments are evaluated first. In some cases, using ``ifelse`` instead of an ``if`` statement can eliminate the branch in generated code and provide higher performance in tight loops.

.. function:: lexcmp(x, y)

   .. Docstring generated from Julia source

   Compare ``x`` and ``y`` lexicographically and return -1, 0, or 1 depending on whether ``x`` is less than, equal to, or greater than ``y``\ , respectively. This function should be defined for lexicographically comparable types, and ``lexless`` will call ``lexcmp`` by default.

.. function:: lexless(x, y)

   .. Docstring generated from Julia source

   Determine whether ``x`` is lexicographically less than ``y``\ .

.. function:: typeof(x)

   .. Docstring generated from Julia source

   Get the concrete type of ``x``\ .

.. function:: tuple(xs...)

   .. Docstring generated from Julia source

   Construct a tuple of the given objects.

.. function:: ntuple(f::Function, n)

   .. Docstring generated from Julia source

   Create a tuple of length ``n``\ , computing each element as ``f(i)``\ , where ``i`` is the index of the element.

.. function:: object_id(x)

   .. Docstring generated from Julia source

   Get a unique integer id for ``x``\ . ``object_id(x)==object_id(y)`` if and only if ``is(x,y)``\ .

.. function:: hash(x[, h])

   .. Docstring generated from Julia source

   Compute an integer hash code such that ``isequal(x,y)`` implies ``hash(x)==hash(y)``\ . The optional second argument ``h`` is a hash code to be mixed with the result.

   New types should implement the 2-argument form, typically by calling the 2-argument ``hash`` method recursively in order to mix hashes of the contents with each other (and with ``h``\ ). Typically, any type that implements ``hash`` should also implement its own ``==`` (hence ``isequal``\ ) to guarantee the property mentioned above.

.. function:: finalizer(x, function)

   .. Docstring generated from Julia source

   Register a function ``f(x)`` to be called when there are no program-accessible references to ``x``\ . The behavior of this function is unpredictable if ``x`` is of a bits type.

.. function:: finalize(x)

   .. Docstring generated from Julia source

   Immediately run finalizers registered for object ``x``\ .

.. function:: copy(x)

   .. Docstring generated from Julia source

   Create a shallow copy of ``x``\ : the outer structure is copied, but not all internal values. For example, copying an array produces a new array with identically-same elements as the original.

.. function:: deepcopy(x)

   .. Docstring generated from Julia source

   Create a deep copy of ``x``\ : everything is copied recursively, resulting in a fully independent object. For example, deep-copying an array produces a new array whose elements are deep copies of the original elements. Calling ``deepcopy`` on an object should generally have the same effect as serializing and then deserializing it.

   As a special case, functions can only be actually deep-copied if they are anonymous, otherwise they are just copied. The difference is only relevant in the case of closures, i.e. functions which may contain hidden internal references.

   While it isn't normally necessary, user-defined types can override the default ``deepcopy`` behavior by defining a specialized version of the function ``deepcopy_internal(x::T, dict::ObjectIdDict)`` (which shouldn't otherwise be used), where ``T`` is the type to be specialized for, and ``dict`` keeps track of objects copied so far within the recursion. Within the definition, ``deepcopy_internal`` should be used in place of ``deepcopy``\ , and the ``dict`` variable should be updated as appropriate before returning.

.. function:: isdefined([object,] index | symbol)

   .. Docstring generated from Julia source

   Tests whether an assignable location is defined. The arguments can be an array and index, a composite object and field name (as a symbol), or a module and a symbol. With a single symbol argument, tests whether a global variable with that name is defined in ``current_module()``\ .

.. function:: convert(T, x)

   .. Docstring generated from Julia source

   Convert ``x`` to a value of type ``T``.

   If ``T`` is an ``Integer`` type, an :exc:`InexactError` will be raised if
   ``x`` is not representable by ``T``, for example if ``x`` is not
   integer-valued, or is outside the range supported by ``T``.

   .. doctest::

      julia> convert(Int, 3.0)
      3

      julia> convert(Int, 3.5)
      ERROR: InexactError()
       in convert at int.jl:209

   If ``T`` is a :obj:`AbstractFloat` or :obj:`Rational` type, then it will return
   the closest value to ``x`` representable by ``T``.

   .. doctest::

      julia> x = 1/3
      0.3333333333333333

      julia> convert(Float32, x)
      0.33333334f0

      julia> convert(Rational{Int32}, x)
      1//3

      julia> convert(Rational{Int64}, x)
      6004799503160661//18014398509481984

.. function:: promote(xs...)

   .. Docstring generated from Julia source

   Convert all arguments to their common promotion type (if any), and return them all (as a tuple).

.. function:: oftype(x, y)

   .. Docstring generated from Julia source

   Convert ``y`` to the type of ``x`` (``convert(typeof(x), y)``\ ).

.. function:: widen(type | x)

   .. Docstring generated from Julia source

   If the argument is a type, return a "larger" type (for numeric types, this will be a type with at least as much range and precision as the argument, and usually more). Otherwise the argument ``x`` is converted to ``widen(typeof(x))``\ .

   .. doctest::

       julia> widen(Int32)
       Int64

       julia> widen(1.5f0)
       1.5

.. function:: identity(x)

   .. Docstring generated from Julia source

   The identity function. Returns its argument.

Types
-----

.. function:: super(T::DataType)

   .. Docstring generated from Julia source

   Return the supertype of DataType ``T``\ .

.. function:: issubtype(type1, type2)

   .. Docstring generated from Julia source

   Return ``true`` if and only if all values of ``type1`` are also of ``type2``\ . Can also be written using the ``<:`` infix operator as ``type1 <: type2``\ .

.. function:: <:(T1, T2)

   .. Docstring generated from Julia source

   Subtype operator, equivalent to ``issubtype(T1,T2)``\ .

.. function:: subtypes(T::DataType)

   .. Docstring generated from Julia source

   Return a list of immediate subtypes of DataType ``T``\ . Note that all currently loaded subtypes are included, including those not visible in the current module.

.. function:: typemin(T)

   .. Docstring generated from Julia source

   The lowest value representable by the given (real) numeric DataType ``T``\ .

.. function:: typemax(T)

   .. Docstring generated from Julia source

   The highest value representable by the given (real) numeric ``DataType``\ .

.. function:: realmin(T)

   .. Docstring generated from Julia source

   The smallest in absolute value non-subnormal value representable by the given floating-point DataType ``T``\ .

.. function:: realmax(T)

   .. Docstring generated from Julia source

   The highest finite value representable by the given floating-point DataType ``T``\ .

.. function:: maxintfloat(T)

   .. Docstring generated from Julia source

   The largest integer losslessly representable by the given floating-point DataType ``T``\ .

.. function:: sizeof(T)

   .. Docstring generated from Julia source

   Size, in bytes, of the canonical binary representation of the given DataType ``T``\ , if any.

.. function:: eps(T)

   .. Docstring generated from Julia source

   The distance between 1.0 and the next larger representable floating-point value of ``DataType`` ``T``\ . Only floating-point types are sensible arguments.

.. function:: eps()

   .. Docstring generated from Julia source

   The distance between 1.0 and the next larger representable floating-point value of ``Float64``\ .

.. function:: eps(x)

   .. Docstring generated from Julia source

   The distance between ``x`` and the next larger representable floating-point value of the same ``DataType`` as ``x``\ .

.. function:: promote_type(type1, type2)

   .. Docstring generated from Julia source

   Determine a type big enough to hold values of each argument type without loss, whenever possible. In some cases, where no type exists to which both types can be promoted losslessly, some loss is tolerated; for example, ``promote_type(Int64,Float64)`` returns ``Float64`` even though strictly, not all ``Int64`` values can be represented exactly as ``Float64`` values.

.. function:: promote_rule(type1, type2)

   .. Docstring generated from Julia source

   Specifies what type should be used by ``promote`` when given values of types ``type1`` and ``type2``\ . This function should not be called directly, but should have definitions added to it for new types as appropriate.

.. function:: getfield(value, name::Symbol)

   .. Docstring generated from Julia source

   Extract a named field from a ``value`` of composite type. The syntax ``a.b`` calls ``getfield(a, :b)``\ , and the syntax ``a.(b)`` calls ``getfield(a, b)``\ .

.. function:: setfield!(value, name::Symbol, x)

   .. Docstring generated from Julia source

   Assign ``x`` to a named field in ``value`` of composite type. The syntax ``a.b = c`` calls ``setfield!(a, :b, c)``\ , and the syntax ``a.(b) = c`` calls ``setfield!(a, b, c)``\ .

.. function:: fieldoffsets(type)

   .. Docstring generated from Julia source

   The byte offset of each field of a type relative to the data start. For example, we could use it in the following manner to summarize information about a struct type:

   .. doctest::

       julia> structinfo(T) = [zip(fieldoffsets(T),fieldnames(T),T.types)...];

       julia> structinfo(StatStruct)
       12-element Array{Tuple{Int64,Symbol,DataType},1}:
        (0,:device,UInt64)
        (8,:inode,UInt64)
        (16,:mode,UInt64)
        (24,:nlink,Int64)
        (32,:uid,UInt64)
        (40,:gid,UInt64)
        (48,:rdev,UInt64)
        (56,:size,Int64)
        (64,:blksize,Int64)
        (72,:blocks,Int64)
        (80,:mtime,Float64)
        (88,:ctime,Float64)

.. function:: fieldtype(T, name::Symbol | index::Int)

   .. Docstring generated from Julia source

   Determine the declared type of a field (specified by name or index) in a composite DataType ``T``\ .

.. function:: isimmutable(v)

   .. Docstring generated from Julia source

   Return ``true`` iff value ``v`` is immutable.  See :ref:`man-immutable-composite-types` for a discussion of immutability.
   Note that this function works on values, so if you give it a type, it will tell you that a value of ``DataType`` is mutable.

.. function:: isbits(T)

   .. Docstring generated from Julia source

   Return ``true`` if ``T`` is a "plain data" type, meaning it is immutable and contains no references to other values. Typical examples are numeric types such as ``UInt8``\ , ``Float64``\ , and ``Complex{Float64}``\ .

   .. doctest::

       julia> isbits(Complex{Float64})
       true

       julia> isbits(Complex)
       false

.. function:: isleaftype(T)

   .. Docstring generated from Julia source

   Determine whether ``T`` is a concrete type that can have instances, meaning its only subtypes are itself and ``None`` (but ``T`` itself is not ``None``\ ).

.. function:: typejoin(T, S)

   .. Docstring generated from Julia source

   Compute a type that contains both ``T`` and ``S``\ .

.. function:: typeintersect(T, S)

   .. Docstring generated from Julia source

   Compute a type that contains the intersection of ``T`` and ``S``\ . Usually this will be the smallest such type or one close to it.

.. function:: Val{c}

   .. Docstring generated from Julia source

   Create a "value type" out of ``c``\ , which must be an ``isbits`` value. The intent of this construct is to be able to dispatch on constants, e.g., ``f(Val{false})`` allows you to dispatch directly (at compile-time) to an implementation ``f(::Type{Val{false}})``\ , without having to test the boolean value at runtime.

.. function:: @enum EnumName EnumValue1[=x] EnumValue2[=y]

   .. Docstring generated from Julia source

   Create an :obj:`Enum` type with name ``EnumName`` and enum member values of ``EnumValue1`` and ``EnumValue2`` with optional assigned values of ``x`` and ``y``, respectively. ``EnumName`` can be used just like other types and enum member values as regular values, such as

   .. doctest::

      julia> @enum FRUIT apple=1 orange=2 kiwi=3

      julia> f(x::FRUIT) = "I'm a FRUIT with value: $(Int(x))"
      f (generic function with 1 method)

      julia> f(apple)
      "I'm a FRUIT with value: 1"

.. function:: instances(T::Type)

   .. Docstring generated from Julia source

   Return a collection of all instances of the given type, if applicable. Mostly used for enumerated types (see ``@enum``\ ).

Generic Functions
-----------------

.. function:: method_exists(f, Tuple type) -> Bool

   .. Docstring generated from Julia source

   Determine whether the given generic function has a method matching the given :obj:`Tuple` of argument types.

   .. doctest::

      julia> method_exists(length, Tuple{Array})
      true

.. function:: applicable(f, args...) -> Bool

   .. Docstring generated from Julia source

   Determine whether the given generic function has a method applicable to the given arguments.

   .. doctest::

       julia> function f(x, y)
                  x + y
              end;

       julia> applicable(f, 1)
       false

       julia> applicable(f, 1, 2)
       true

.. function:: invoke(f, (types...), args...)

   .. Docstring generated from Julia source

   Invoke a method for the given generic function matching the specified types (as a tuple), on the specified arguments. The arguments must be compatible with the specified types. This allows invoking a method other than the most specific matching method, which is useful when the behavior of a more general definition is explicitly needed (often as part of the implementation of a more specific method of the same function).

.. function:: |>(x, f)

   .. Docstring generated from Julia source

   Applies a function to the preceding argument. This allows for easy function chaining.

   .. doctest::

       julia> [1:5;] |> x->x.^2 |> sum |> inv
       0.01818181818181818

.. function:: call(x, args...)

   .. Docstring generated from Julia source

   If ``x`` is not a ``Function``\ , then ``x(args...)`` is equivalent to ``call(x, args...)``\ . This means that function-like behavior can be added to any type by defining new ``call`` methods.

Syntax
------

.. function:: eval([m::Module], expr::Expr)

   .. Docstring generated from Julia source

   Evaluate an expression in the given module and return the result. Every ``Module`` (except those defined with ``baremodule``\ ) has its own 1-argument definition of ``eval``\ , which evaluates expressions in that module.

.. function:: @eval

   .. Docstring generated from Julia source

   Evaluate an expression and return the value.

.. function:: evalfile(path::AbstractString)

   .. Docstring generated from Julia source

   Load the file using ``include``\ , evaluate all expressions, and return the value of the last one.

.. function:: esc(e::ANY)

   .. Docstring generated from Julia source

   Only valid in the context of an ``Expr`` returned from a macro. Prevents the macro hygiene pass from turning embedded variables into gensym variables. See the :ref:`man-macros`
   section of the Metaprogramming chapter of the manual for more details and examples.

.. function:: gensym([tag])

   .. Docstring generated from Julia source

   Generates a symbol which will not conflict with other variable names.

.. function:: @gensym

   .. Docstring generated from Julia source

   Generates a gensym symbol for a variable. For example, ``@gensym x y`` is transformed into ``x = gensym("x"); y = gensym("y")``\ .

.. function:: parse(str, start; greedy=true, raise=true)

   .. Docstring generated from Julia source

   Parse the expression string and return an expression (which could later be passed to eval for execution). ``start`` is the index of the first character to start parsing. If ``greedy`` is ``true`` (default), ``parse`` will try to consume as much input as it can; otherwise, it will stop as soon as it has parsed a valid expression. Incomplete but otherwise syntactically valid expressions will return ``Expr(:incomplete, "(error message)")``\ . If ``raise`` is ``true`` (default), syntax errors other than incomplete expressions will raise an error. If ``raise`` is ``false``\ , ``parse`` will return an expression that will raise an error upon evaluation.

.. function:: parse(str; raise=true)

   .. Docstring generated from Julia source

   Parse the expression string greedily, returning a single expression. An error is thrown if there are additional characters after the first expression. If ``raise`` is ``true`` (default), syntax errors will raise an error; otherwise, ``parse`` will return an expression that will raise an error upon evaluation.

Nullables
---------

.. function:: Nullable(x)

   .. Docstring generated from Julia source

   Wrap value ``x`` in an object of type ``Nullable``\ , which indicates whether a value is present. ``Nullable(x)`` yields a non-empty wrapper, and ``Nullable{T}()`` yields an empty instance of a wrapper that might contain a value of type ``T``\ .

.. function:: get(x)

   .. Docstring generated from Julia source

   Attempt to access the value of the ``Nullable`` object, ``x``. Returns the
   value if it is present; otherwise, throws a ``NullException``.

.. function:: get(x, y)

   .. Docstring generated from Julia source

   Attempt to access the value of the ``Nullable{T}`` object, ``x``. Returns
   the value if it is present; otherwise, returns ``convert(T, y)``.

.. function:: isnull(x)

   .. Docstring generated from Julia source

   Is the ``Nullable`` object ``x`` null, i.e. missing a value?

System
------

.. function:: run(command)

   .. Docstring generated from Julia source

   Run a command object, constructed with backticks. Throws an error if anything goes wrong, including the process exiting with a non-zero status.

.. function:: spawn(command)

   .. Docstring generated from Julia source

   Run a command object asynchronously, returning the resulting ``Process`` object.

.. data:: DevNull

   Used in a stream redirect to discard all data written to it. Essentially equivalent to /dev/null on Unix or NUL on Windows.
   Usage: ``run(`cat test.txt` |> DevNull)``

.. function:: success(command)

   .. Docstring generated from Julia source

   Run a command object, constructed with backticks, and tell whether it was successful (exited with a code of 0). An exception is raised if the process cannot be started.

.. function:: process_running(p::Process)

   .. Docstring generated from Julia source

   Determine whether a process is currently running.

.. function:: process_exited(p::Process)

   .. Docstring generated from Julia source

   Determine whether a process has exited.

.. function:: kill(p::Process, signum=SIGTERM)

   .. Docstring generated from Julia source

   Send a signal to a process. The default is to terminate the process.

.. function:: open(command, mode::AbstractString="r", stdio=DevNull)

   .. Docstring generated from Julia source

   Start running ``command`` asynchronously, and return a tuple
   ``(stream,process)``.  If ``mode`` is ``"r"``, then ``stream``
   reads from the process's standard output and ``stdio`` optionally
   specifies the process's standard input stream.  If ``mode`` is
   ``"w"``, then ``stream`` writes to the process's standard input
   and ``stdio`` optionally specifies the process's standard output
   stream.

.. function:: open(f::Function, command, mode::AbstractString="r", stdio=DevNull)

   .. Docstring generated from Julia source

   Similar to ``open(command, mode, stdio)``, but calls ``f(stream)``
   on the resulting read or write stream, then closes the stream
   and waits for the process to complete.  Returns the value returned
   by ``f``.

.. function:: Sys.set_process_title(title::AbstractString)

   .. Docstring generated from Julia source

   Set the process title. No-op on some operating systems. (not exported)

.. function:: Sys.get_process_title()

   .. Docstring generated from Julia source

   Get the process title. On some systems, will always return empty string. (not exported)

.. function:: readandwrite(command)

   .. Docstring generated from Julia source

   Starts running a command asynchronously, and returns a tuple (stdout,stdin,process) of the output stream and input stream of the process, and the process object itself.

.. function:: ignorestatus(command)

   .. Docstring generated from Julia source

   Mark a command object so that running it will not throw an error if the result code is non-zero.

.. function:: detach(command)

   .. Docstring generated from Julia source

   Mark a command object so that it will be run in a new process group, allowing it to outlive the julia process, and not have Ctrl-C interrupts passed to it.

.. function:: setenv(command, env; dir=working_dir)

   .. Docstring generated from Julia source

   Set environment variables to use when running the given ``command``\ . ``env`` is either a dictionary mapping strings to strings, an array of strings of the form ``"var=val"``\ , or zero or more ``"var"=>val`` pair arguments. In order to modify (rather than replace) the existing environment, create ``env`` by ``copy(ENV)`` and then setting ``env["var"]=val`` as desired, or use ``withenv``\ .

   The ``dir`` keyword argument can be used to specify a working directory for the command.

.. function:: withenv(f::Function, kv::Pair...)

   .. Docstring generated from Julia source

   Execute ``f()`` in an environment that is temporarily modified (not replaced as in ``setenv``\ ) by zero or more ``"var"=>val`` arguments ``kv``\ . ``withenv`` is generally used via the ``withenv(kv...) do ... end`` syntax. A value of ``nothing`` can be used to temporarily unset an environment variable (if it is set). When ``withenv`` returns, the original environment has been restored.

.. function:: pipeline(from, to, ...)

   .. Docstring generated from Julia source

   Create a pipeline from a data source to a destination. The source and destination can be commands, I/O streams, strings, or results of other ``pipeline`` calls. At least one argument must be a command. Strings refer to filenames. When called with more than two arguments, they are chained together from left to right. For example ``pipeline(a,b,c)`` is equivalent to ``pipeline(pipeline(a,b),c)``\ . This provides a more concise way to specify multi-stage pipelines.

   **Examples**:

     * ``run(pipeline(`ls`, `grep xyz`))``
     * ``run(pipeline(`ls`, "out.txt"))``
     * ``run(pipeline("out.txt", `grep xyz`))``

.. function:: pipeline(command; stdin, stdout, stderr, append=false)

   .. Docstring generated from Julia source

   Redirect I/O to or from the given ``command``\ . Keyword arguments specify which of the command's streams should be redirected. ``append`` controls whether file output appends to the file. This is a more general version of the 2-argument ``pipeline`` function. ``pipeline(from, to)`` is equivalent to ``pipeline(from, stdout=to)`` when ``from`` is a command, and to ``pipe(to, stdin=from)`` when ``from`` is another kind of data source.

   **Examples**:

     * ``run(pipeline(`dothings`, stdout="out.txt", stderr="errs.txt"))``
     * ``run(pipeline(`update`, stdout="log.txt", append=true))``

.. function:: gethostname() -> AbstractString

   .. Docstring generated from Julia source

   Get the local machine's host name.

.. function:: getipaddr() -> AbstractString

   .. Docstring generated from Julia source

   Get the IP address of the local machine, as a string of the form "x.x.x.x".

.. function:: getpid() -> Int32

   .. Docstring generated from Julia source

   Get julia's process ID.

.. function:: time()

   .. Docstring generated from Julia source

   Get the system time in seconds since the epoch, with fairly high (typically, microsecond) resolution.

.. function:: time_ns()

   .. Docstring generated from Julia source

   Get the time in nanoseconds. The time corresponding to 0 is undefined, and wraps every 5.8 years.

.. function:: tic()

   .. Docstring generated from Julia source

   Set a timer to be read by the next call to :func:`toc` or :func:`toq`. The macro call ``@time expr`` can also be used to time evaluation.

.. function:: toc()

   .. Docstring generated from Julia source

   Print and return the time elapsed since the last :func:`tic`.

.. function:: toq()

   .. Docstring generated from Julia source

   Return, but do not print, the time elapsed since the last :func:`tic`.

.. function:: @time

   .. Docstring generated from Julia source

   A macro to execute an expression, printing the time it took to execute, the number of allocations, and the total number of bytes its execution caused to be allocated, before returning the value of the expression.

.. function:: @timev

   .. Docstring generated from Julia source

   This is a verbose version of the ``@time`` macro. It first prints the same information as ``@time``\ , then any non-zero memory allocation counters, and then returns the value of the expression.

.. function:: @timed

   .. Docstring generated from Julia source

   A macro to execute an expression, and return the value of the expression, elapsed time, total bytes allocated, garbage collection time, and an object with various memory allocation counters.

.. function:: @elapsed

   .. Docstring generated from Julia source

   A macro to evaluate an expression, discarding the resulting value, instead returning the number of seconds it took to execute as a floating-point number.

.. function:: @allocated

   .. Docstring generated from Julia source

   A macro to evaluate an expression, discarding the resulting value, instead returning the total number of bytes allocated during evaluation of the expression. Note: the expression is evaluated inside a local function, instead of the current context, in order to eliminate the effects of compilation, however, there still may be some allocations due to JIT compilation. This also makes the results inconsistent with the ``@time`` macros, which do not try to adjust for the effects of compilation.

.. function:: EnvHash() -> EnvHash

   .. Docstring generated from Julia source

   A singleton of this type provides a hash table interface to environment variables.

.. data:: ENV

   Reference to the singleton ``EnvHash``, providing a dictionary interface to system environment variables.

.. function:: @unix

   .. Docstring generated from Julia source

   Given ``@unix? a : b``\ , do ``a`` on Unix systems (including Linux and OS X) and ``b`` elsewhere. See documentation for Handling Platform Variations in the Calling C and Fortran Code section of the manual.

.. function:: @osx

   .. Docstring generated from Julia source

   Given ``@osx? a : b``\ , do ``a`` on OS X and ``b`` elsewhere. See documentation for Handling Platform Variations in the Calling C and Fortran Code section of the manual.

.. function:: @linux

   .. Docstring generated from Julia source

   Given ``@linux? a : b``\ , do ``a`` on Linux and ``b`` elsewhere. See documentation for Handling Platform Variations in the Calling C and Fortran Code section of the manual.

.. function:: @windows

   .. Docstring generated from Julia source

   Given ``@windows? a : b``\ , do ``a`` on Windows and ``b`` elsewhere. See documentation for Handling Platform Variations in the Calling C and Fortran Code section of the manual.

Errors
------

.. function:: error(message::AbstractString)

   .. Docstring generated from Julia source

   Raise an ``ErrorException`` with the given message

.. function:: throw(e)

   .. Docstring generated from Julia source

   Throw an object as an exception

.. function:: rethrow([e])

   .. Docstring generated from Julia source

   Throw an object without changing the current exception backtrace. The default argument is the current exception (if called within a ``catch`` block).

.. function:: backtrace()

   .. Docstring generated from Julia source

   Get a backtrace object for the current program point.

.. function:: catch_backtrace()

   .. Docstring generated from Julia source

   Get the backtrace of the current exception, for use within ``catch`` blocks.

.. function:: assert(cond)

   .. Docstring generated from Julia source

   Throw an ``AssertionError`` if ``cond`` is ``false``\ . Also available as the macro ``@assert expr``\ .

.. function:: @assert cond [text]

   .. Docstring generated from Julia source

   Throw an ``AssertionError`` if ``cond`` is ``false``\ . Preferred syntax for writing assertions. Message ``text`` is optionally displayed upon assertion failure.

.. function:: ArgumentError(msg)

   .. Docstring generated from Julia source

   The parameters to a function call do not match a valid signature. Argument ``msg`` is a descriptive error string.

.. function:: AssertionError([msg])

   .. Docstring generated from Julia source

   The asserted condition did not evalutate to ``true``\ . Optional argument ``msg`` is a descriptive error string.

.. function:: BoundsError([a],[i])

   .. Docstring generated from Julia source

   An indexing operation into an array, ``a``\ , tried to access an out-of-bounds element, ``i``\ .

.. function:: DimensionMismatch([msg])

   .. Docstring generated from Julia source

   The objects called do not have matching dimensionality. Optional argument ``msg`` is a descriptive error string.

.. function:: DivideError()

   .. Docstring generated from Julia source

   Integer division was attempted with a denominator value of 0.

.. function:: DomainError()

   .. Docstring generated from Julia source

   The arguments to a function or constructor are outside the valid domain.

.. function:: EOFError()

   .. Docstring generated from Julia source

   No more data was available to read from a file or stream.

.. function:: ErrorException(msg)

   .. Docstring generated from Julia source

   Generic error type. The error message, in the ``.msg`` field, may provide more specific details.

.. function:: InexactError()

   .. Docstring generated from Julia source

   Type conversion cannot be done exactly.

.. function:: InterruptException()

   .. Docstring generated from Julia source

   The process was stopped by a terminal interrupt (CTRL+C).

.. function:: KeyError(key)

   .. Docstring generated from Julia source

   An indexing operation into an ``Associative`` (``Dict``\ ) or ``Set`` like object tried to access or delete a non-existent element.

.. function:: LoadError(file::AbstractString, line::Int, error)

   .. Docstring generated from Julia source

   An error occurred while ``include``\ ing, ``require``\ ing, or ``using`` a file. The error specifics should be available in the ``.error`` field.

.. function:: MethodError(f, args)

   .. Docstring generated from Julia source

   A method with the required type signature does not exist in the given generic function.

.. function:: NullException()

   .. Docstring generated from Julia source

   An attempted access to a ``Nullable`` with no defined value.

.. function:: OutOfMemoryError()

   .. Docstring generated from Julia source

   An operation allocated too much memory for either the system or the garbage collector to handle properly.

.. function:: ReadOnlyMemoryError()

   .. Docstring generated from Julia source

   An operation tried to write to memory that is read-only.

.. function:: OverflowError()

   .. Docstring generated from Julia source

   The result of an expression is too large for the specified type and will cause a wraparound.

.. function:: ParseError(msg)

   .. Docstring generated from Julia source

   The expression passed to the ``parse`` function could not be interpreted as a valid Julia expression.

.. function:: ProcessExitedException()

   .. Docstring generated from Julia source

   After a client Julia process has exited, further attempts to reference the dead child will throw this exception.

.. function:: StackOverflowError()

   .. Docstring generated from Julia source

   The function call grew beyond the size of the call stack. This usually happens when a call recurses infinitely.

.. function:: SystemError(prefix::AbstractString, [errno::Int32])

   .. Docstring generated from Julia source

   A system call failed with an error code (in the ``errno`` global variable).

.. function:: TypeError(func::Symbol, context::AbstractString, expected::Type, got)

   .. Docstring generated from Julia source

   A type assertion failure, or calling an intrinsic function with an incorrect argument type.

.. function:: UndefRefError()

   .. Docstring generated from Julia source

   The item or field is not defined for the given object.

.. function:: UndefVarError(var::Symbol)

   .. Docstring generated from Julia source

   A symbol in the current scope is not defined.

.. function:: InitError(mod::Symbol, error)

   .. Docstring generated from Julia source

   An error occurred when running a module's ``__init__`` function. The actual error thrown is available in the ``.error`` field.

Events
------

.. function:: Timer(callback::Function, delay, repeat=0)

   .. Docstring generated from Julia source

   Create a timer to call the given ``callback`` function. The ``callback`` is passed one argument, the timer object itself. The callback will be invoked after the specified initial ``delay``\ , and then repeating with the given ``repeat`` interval. If ``repeat`` is ``0``\ , the timer is only triggered once. Times are in seconds. A timer is stopped and has its resources freed by calling ``close`` on it.

.. function:: Timer(delay, repeat=0)

   .. Docstring generated from Julia source

   Create a timer that wakes up tasks waiting for it (by calling ``wait`` on the timer object) at a specified interval.  Times are in seconds.  Waiting tasks are woken with an error when the timer is closed (by ``close``\ ). Use ``isopen`` to check whether a timer is still active.

Reflection
----------

.. function:: module_name(m::Module) -> Symbol

   .. Docstring generated from Julia source

   Get the name of a ``Module`` as a ``Symbol``\ .

.. function:: module_parent(m::Module) -> Module

   .. Docstring generated from Julia source

   Get a module's enclosing ``Module``\ . ``Main`` is its own parent, as is ``LastMain`` after ``workspace()``\ .

.. function:: current_module() -> Module

   .. Docstring generated from Julia source

   Get the *dynamically* current ``Module``\ , which is the ``Module`` code is currently being read from. In general, this is not the same as the module containing the call to this function.

.. function:: fullname(m::Module)

   .. Docstring generated from Julia source

   Get the fully-qualified name of a module as a tuple of symbols. For example, ``fullname(Base.Pkg)`` gives ``(:Base,:Pkg)``\ , and ``fullname(Main)`` gives ``()``\ .

.. function:: names(x::Module[, all=false[, imported=false]])

   .. Docstring generated from Julia source

   Get an array of the names exported by a ``Module``\ , with optionally more ``Module`` globals according to the additional parameters.

.. function:: nfields(x::DataType) -> Int

   .. Docstring generated from Julia source

   Get the number of fields of a ``DataType``\ .

.. function:: fieldnames(x::DataType)

   .. Docstring generated from Julia source

   Get an array of the fields of a ``DataType``\ .

.. function:: isconst([m::Module], s::Symbol) -> Bool

   .. Docstring generated from Julia source

   Determine whether a global is declared ``const`` in a given ``Module``\ . The default ``Module`` argument is ``current_module()``\ .

.. function:: isgeneric(f::Function) -> Bool

   .. Docstring generated from Julia source

   Determine whether a ``Function`` is generic.

.. function:: function_name(f::Function) -> Symbol

   .. Docstring generated from Julia source

   Get the name of a generic ``Function`` as a symbol, or ``:anonymous``\ .

.. function:: function_module(f::Function, types) -> Module

   .. Docstring generated from Julia source

   Determine the module containing a given definition of a generic function.

.. function:: functionloc(f::Function, types)

   .. Docstring generated from Julia source

   Returns a tuple ``(filename,line)`` giving the location of a generic ``Function`` definition.

.. function:: functionloc(m::Method)

   .. Docstring generated from Julia source

   Returns a tuple ``(filename,line)`` giving the location of a ``Method`` definition.

Internals
---------

.. function:: gc()

   .. Docstring generated from Julia source

   Perform garbage collection. This should not generally be used.

.. function:: gc_enable(on::Bool)

   .. Docstring generated from Julia source

   Control whether garbage collection is enabled using a boolean argument (``true`` for enabled, ``false`` for disabled). Returns previous GC state. Disabling garbage collection should be used only with extreme caution, as it can cause memory use to grow without bound.

.. function:: macroexpand(x)

   .. Docstring generated from Julia source

   Takes the expression ``x`` and returns an equivalent expression with all macros removed (expanded).

.. function:: expand(x)

   .. Docstring generated from Julia source

   Takes the expression ``x`` and returns an equivalent expression in lowered form.

.. function:: code_lowered(f, types)

   .. Docstring generated from Julia source

   Returns an array of lowered ASTs for the methods matching the given generic function and type signature.

.. function:: @code_lowered

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls :func:`code_lowered` on the resulting expression.

.. function:: code_typed(f, types; optimize=true)

   .. Docstring generated from Julia source

   Returns an array of lowered and type-inferred ASTs for the methods matching the given generic function and type signature. The keyword argument ``optimize`` controls whether additional optimizations, such as inlining, are also applied.

.. function:: @code_typed

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls :func:`code_typed` on the resulting expression.

.. function:: code_warntype(f, types)

   .. Docstring generated from Julia source

   Displays lowered and type-inferred ASTs for the methods matching the given generic function and type signature. The ASTs are annotated in such a way as to cause "non-leaf" types to be emphasized (if color is available, displayed in red). This serves as a warning of potential type instability. Not all non-leaf types are particularly problematic for performance, so the results need to be used judiciously. See :ref:`man-code-warntype` for more information.

.. function:: @code_warntype

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls :func:`code_warntype` on the resulting expression.

.. function:: code_llvm(f, types)

   .. Docstring generated from Julia source

   Prints the LLVM bitcodes generated for running the method matching the given generic function and type signature to :const:`STDOUT`.

   All metadata and dbg.* calls are removed from the printed bitcode. Use code_llvm_raw for the full IR.

.. function:: @code_llvm

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls :func:`code_llvm` on the resulting expression.

.. function:: code_native(f, types)

   .. Docstring generated from Julia source

   Prints the native assembly instructions generated for running the method matching the given generic function and type signature to STDOUT.

.. function:: @code_native

   .. Docstring generated from Julia source

   Evaluates the arguments to the function call, determines their types, and calls :func:`code_native` on the resulting expression.

.. function:: precompile(f,args::Tuple{Vararg{Any}})

   .. Docstring generated from Julia source

   Compile the given function ``f`` for the argument tuple (of types) ``args``\ , but do not execute it.

