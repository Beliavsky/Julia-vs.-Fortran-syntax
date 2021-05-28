# Julia vs. Fortran syntax

Here are some notes on equivalent syntax in Julia and Fortran. Below, the Julia syntax appears first and the Fortran syntax appears after "vs.". Additions and corrections are welcomed -- please raise an issue.

`x = 1.0` vs. `x = 1.0d0` (Julia uses 64-bit floats)

`x = zeros(5)` # array of Float64 intialized to zero 

vs.

     real(kind=kind(1.0d0)), allocatable :: x(:)
     allocate (x(5),source=0.0d0)

`[]` vs. `()` to index array elements

`true` and `false` vs. `.true.` and `.false.`

`&` vs `.and.`

`|` vs. `.or.`

`!` vs. `.not.`

`^` vs. `**` for exponentiation.

`x = rand()` vs. `call random_number(x)` where `x` is a scalar

`x = rand(n)` vs. `call random_number(x)` where `x` is a vector of length `n` 

`x = rand(n1,n2)` vs. `call random_number(x)` where `x` has shape `[n1,n2]`

`sum(x)` has the same meaning in Julia and Fortran

`sum(x,dims=1)` vs. `sum(x,dim=1)`

`sum(x,dims=[1,2])` = `sum(x)` for 2-D array

In general, Julia has a `dims` optional argument for array functions vs. `dim` in Fortran,

`minimum(x)` and `maximum(x)` vs. `minval(x)` and `maxval(x)`

`size(x)` returns tuple, `size(x,dim)` returns scalar. Fortran `size(x)` returns a scalar. 
`size(x)` vs. `shape(x)`
`length(x)` vs. `size(x)`

`x = [2,3]` creates a 1-D array of integers 
`x = [2,3.1]` creates a 1-D array of Float64, `[2.0,3.1]`. The integer value is coerced to Float64. In Fortran the elements of an array constructor must have the same type.

`vec(x)` converts x to 1-D array, vs. `[x]`

`x .> 0` vs. `merge(1,0,x>0)`    Note the . before >.
`x[x .> 0]` vs. `pack(x,x>0)`

`*` vs. `//` to concatenate strings

`rstrip("foo ")` vs. `trim("foo ")`

`println(2," ",5)` vs. `print*,2,5` -- Julia does not insert spaces between printed items by default.

`print(2)` vs. `write(*,"(i0)",advance="no") 2` -- Julia `print` does not append newline, unlike `println`
 
`#` vs. `!` for comments

`dog` and `Dog` are distinct variables in Julia but not in Fortran

loops:

    for i in 1:3
        println(i," ",i^2)
    end

vs.

    do i=1,3
        print*,i,i**2
    end do
    
If block:

    if someVar > 10
        println("someVar is totally bigger than 10.")
    elseif someVar < 10    # This elseif clause is optional.
        println("someVar is smaller than 10.")
    else                    # The else clause is optional too.
        println("someVar is indeed 10.")
    end
    
vs.

    if (someVar > 10) then
        print*,"someVar is totally bigger than 10."
    else if (someVar < 10) then ! This elseif clause is optional.
        print*,"someVar is smaller than 10."
    else                    ! The else clause is optional too.
        print*,"someVar is indeed 10."
    end if

Exiting a loop early:

     for i in 1:5
          println(i)
          if i^2 > 4
               break
          end
     end

vs.

     do i=1,5
          print*,i
          if (i**2 > 4) exit
     end do
    
Function definition:

    function power(i,a)
        return i^a
    end

vs.

    integer function power(i,a)
        power = i**a
    end

Arrays of strings:

     x = ["boy","girl","man"]
     
vs.

     character (len=4), allocatable :: x(:)
     x = [character (len=4) :: "boy","girl","man"]
     x = ["boy ","girl","man "] ! alternative with padding, since the character variables in a Fortran array 
                                ! must have the same length.
