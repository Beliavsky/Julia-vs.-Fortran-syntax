# Julia vs. Fortran syntax

Here are some notes on equivalent syntax in Julia and Fortran. Below, the Julia syntax appears first and the Fortran syntax appears after "vs.". Additions and corrections are welcomed -- please raise an issue.

`x = 1.0` vs. `x = 1.0d0` (Julia uses 64-bit floats)

`x = zeros(5)` # array of Float64 intialized to zero 

vs.

    `real(kind=kind(1.0d0)), allocatable :: x(:)`
    `allocate (x(5),source=0.0d0)`

`[]` vs. `()` to index array elements

`true` and `false` vs. `.true.` and `.false.`

`&` vs `.and.`

`|` vs. `.or.`

`!` vs. `.not.`

`^` vs. `**` for exponentiation.

`x = rand(n)` vs. `call random_number(x)`
`x = rand(n1,n2)` fills a matrix with dimensions [n1,n2]

`sum(x)` like Fortran
`sum(x,dims=1)` vs. `sum(x,dim=1)`
`sum(x,dims=[1,2])` = `sum(x)` for 2-D array
In general, Julia has a `dims` optional argument for array functions vs. `dim` in Fortran,

`minimum(x)` and `maximum(x)` vs. `minval(x)` and `maxval(x)`

`size(x)` returns tuple, `size(x,dim)` returns scalar. Fortran `size(x)` returns a scalar. 
`size(x)` vs. `shape(x)`
`length(x)` vs. `size(x)`

`x = [2,3]` creates a 1-D array of integers 
`x = [2,3.1]` creates a 1-D array of Float64, `[2.0,3.1]`. The integer value is coerced to Float64.

`vec(x)` converts x to 1-D array, vs. `[x]`

`x .> 0` vs. `merge(1,0,x>0)`    Note the . before >.
`x[x .> 0]` vs. `pack(x,x>0)`

`*` vs. `//` to concatenate strings

`rstrip("foo ")` vs. `trim("foo ")`

`println(2," ",5)` vs. `print*,2,5` -- Julia does not insert spaces between printed items by default.

`print(2)` vs. `write(*,"(i0)",advance="no") 2` -- Julia `print` does not append newline, unlike `println`
 
`#` vs. `!` for comments

`dog` and `Dog` are distinct variables in Julia but not in Fortran
