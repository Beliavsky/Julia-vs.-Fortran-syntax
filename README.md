# Julia vs. Fortran syntax

Here are some notes on equivalent syntax in Julia and Fortran. Below, the Julia syntax appears first and the Fortran syntax appears after "vs.". Additions and corrections are welcomed -- please raise an issue.

`x = 1.0` vs. `x = 1.0d0` (Julia uses 64-bit floats by default)

`x = 1.0f0` vs. `x = 1.0` (single precision) 

`true` and `false` vs. `.true.` and `.false.`

`const n = 3` vs. `integer, parameter :: n = 3`

`const name = "Ed"` vs. `character (len=*), parameter :: name = "Ed"`

`const tf = true` vs. `logical, parameter :: tf = .true.`

Fortran does not have the update operations that Julia does, so
`x += 2` vs. `x = x + 2`
`x -= 2` vs. `x = x - 2`
`x *= 2` vs. `x = x * 2`
`x /= 2` vs. `x = x / 2`

`x = zeros(5)` # array of Float64 intialized to zero 

vs. Fortran

     real(kind=kind(1.0d0)), allocatable :: x(:)
     allocate (x(5),source=0.0d0)

`[]` vs. `()` to index array elements

`x[1:2:5]` vs. `x(1:5:2)` to get elements `[x(1),x(3),x(5)]` since Julia uses start:[step:]stop and Fortran uses start:stop[:step]

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

If `x` is an array, `sin.(x)` vs. `sin(x)` and the same for other functions that accept scalar arguments. 
Julia functions need a `.` before the argument(s) to broadcast. A user-defined Fortran function or subroutine
that accepts either scalar or array arguments is declared ELEMENTAL.

In general, Julia has a `dims` optional argument for array functions vs. `dim` in Fortran,

`minimum(x)` and `maximum(x)` vs. `minval(x)` and `maxval(x)`

`max(2,3.1)` vs. `max(real(2),3.1)` or `max(2.0,3.1)` -- the Fortran `max` requires arguments of the same type.

Julia `max.(x,y)` for arrays `x` and `y` is similar to Fortran `max(x,y)`, but Julia `max(x,y)` (without the `.`) for arrays x and y compares the first elements of the two arrays and returns the array with largest 1st element, looking at the 2nd elements etc. to break ties. Fortran has no simple equivalent.

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

`argmax(x)` vs. `maxloc(x,dim=1)` for 1-D array `x`

`sum([true,false,true])` vs. `count([.true.,.false.,.true.])`

For logical variables `x` and `y`, `x == y` vs. `x .eqv. y`

loops in Julia

    for i in 1:3
        println(i," ",i^2)
    end

vs. Fortran

    do i=1,3
        print*,i,i**2
    end do
    
If block in Julia

    if someVar > 10
        println("someVar is totally bigger than 10.")
    elseif someVar < 10    # This elseif clause is optional.
        println("someVar is smaller than 10.")
    else                    # The else clause is optional too.
        println("someVar is indeed 10.")
    end
    
vs. Fortran

    if (someVar > 10) then
        print*,"someVar is totally bigger than 10."
    else if (someVar < 10) then ! This elseif clause is optional.
        print*,"someVar is smaller than 10."
    else                    ! The else clause is optional too.
        print*,"someVar is indeed 10."
    end if
    
One-line if in Julia

     (i > 1) && println("i > 1") 

vs. Fortran

     if (i > 1) print*,"i > 1"

Exiting a loop early in Julia

     for i in 1:5
          println(i)
          if i^2 > 4
               break
          end
     end

vs. Fortran

     do i=1,5
          print*,i
          if (i**2 > 4) exit
     end do
    
Function definition in Julia

    function power(i,a)
        return i^a
    end

vs. Fortran

    integer function power(i,a)
        power = i**a
    end

Arrays of strings in Julia

     x = ["boy","girl","man"]
     
vs. Fortran

     character (len=4), allocatable :: x(:)
     x = [character (len=4) :: "boy","girl","man"]
     x = ["boy ","girl","man "] ! alternative with padding, since the character variables in a Fortran array 
                                ! must have the same length.
        
Module imports:        
`using Foo` vs. `use Foo`
`import Foo: bar` vs. `use Foo, only: bar`

In Fortran but not Julia, module imports must occur before the executable statements.
