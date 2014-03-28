Lists
=====

Not an Array
------------

A list is not an Array. According to the perlfaq4:

> "(...)A list is a fixed collection of scalars. An array is a variable
> that holds a variable collection of scalars(...)"

List values are separated by commas or by the "=>" operator. Although they 
seem igual, they're not: when using the "=>" operator, and the left-hand 
operand has only letters, digits and underscore, it will be interpreted as a 
string.

    sub one { return 'uno'; }

    %hash1 = ( one   ,  1 ); # ( 'uno' => 1 )
    %hash2 = ( one   => 1 ); # ( 'one' => 1 )
    %hash3 = ( one() => 1 ); # ( 'uno' => 1 )

**NOTE:** Using the "=>" operator will not transform the List into an Hash. 
It's the variable, used to store the List's values, that can be an Hash or 
not. 

The number of items in a List is fixed but its values can be changed:

    ($c, $d) = ($d, $c); # switch $c and $d values

    ($first, @rest) = ('one', 'two', 'three'); # $first = 'one'
                                               # @rest  = ('two', 'three')

    (undef, undef, $last) = ('one', 'two', 'three'); # $last = 'three'
    $last = ('one', 'two', 'three');                 # same thing
    $last = ('one', 'two', 'three')[2];              # same thing

In the last two examples, another difference between a List and an Array:
in scalar context an Array will return the number or items and, a List, the 
last item's value.

    @array = ('one', 'two', 'three'); 

    $count = @array;                  # 3
    $last  = ('four', 'five', 'six'); # 'six'


Interpolation
-------------

Each value of a List is automatically interpolated in list context:

    sub one { return 'one', 1 }

    %hash  = ( 'two' => 2 );
    @array = ( 3 ) 
    $four  = 'four,4';

    @list = ( one(), %hash, @array, split( q{,}, $four ) ) 
        # ( 'one', 1, 'two', 2, 3, 'four', 4 )


Subroutines
-----------

Functions receives a List of values. The way this values are stored, define
the data type:

    test( 'one', 1 )
    ...
    sub test {
        my @array  = @_; # ( 'one' ,  1 )
        my %hash   = @_; # ( 'one' => 1 )
        my $scalar = @_; # 2
        ...
    }

Bare in mind that, despite the fact that a function receives a List, the 
variable "@\_", is an Array and, in scalar context, an Array returns the 
number of items.

Functions also returns a List of values. Once more, the way this values are 
stored, define the data type:

    sub test { return ( 'one' => 1 ) }

    @array   = test(); # ( 'one' ,  1 )
    %hash    = test(); # ( 'one' => 1 )
    $scalar  = test(); # 1
    ($first) = test(); # 'one'

    $count = @array; # 2

As said before, values inside a list are automatically interpolated in list 
context. In functions, this behaviour can be affected by prototypes:

    sub with_proto ($) { return @_; }
    sub without_proto  { return @_; }

    @array = ('one', 'two', 'three');

    with_proto( @array )    # (3)
    without_proto( @array ) # ('one', 'two', 'three')

I'll try to explain prototypes in another tutorial.


See Also
--------

* man perlfaq4
* man perldata
* man perlsub
