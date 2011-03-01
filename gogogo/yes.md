!SLIDE subsection
# Examples! #

!SLIDE ruby
    @@@ ruby
    "Your string" =~ /string/

!SLIDE ruby
    @@@ ruby
    "Your string".rmatch? do "string" end

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? do "ab" + "c" end

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? do "xyz".or "abc" end

!SLIDE
# Ever tried to search for "&"? #

!SLIDE ruby
    @@@ ruby
    "symbols.*&+galore".rmatch? do ".*&+" end

!SLIDE
# Precedence #

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? do "ab" + ( "z".or "c" ) end

!SLIDE
# Repetition #

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? do 1.or_more "a" end

    "aaabc".rmatch? do 5.or_less "a" end

    "aaabc".rmatch? do 3.exactly "a" end

    "aaabc".rmatch? do (1..5).of "a" end

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? do 0.or_more "a" end

    "aaabc".rmatch? do any "a" end

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? do 1.or_more "a" end

    "aaabc".rmatch? do some "a" end

!SLIDE
# Character sets #

!SLIDE ruby
    @@@ ruby
    "abc1234.&*".rmatch? do 10.exactly any_char end

!SLIDE ruby
    @@@ ruby
    "abc1234".rmatch? do 3.exactly letter end

!SLIDE ruby
    @@@ ruby
    "abc1234".rmatch? do 4.exactly digit end

!SLIDE ruby
    @@@ ruby
    "abc_123".rmatch? do 7.exactly word_char end

!SLIDE ruby
    @@@ ruby
    " ".rmatch? do whitespace end

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? do 3.exactly "a".."c" end

!SLIDE
# Negation #

!SLIDE ruby
    @@@ ruby
    "x".rmatch? do word_char.not "x" end # => nil

    "y".rmatch? do word_char.not "x" end

!SLIDE ruby
    @@@ ruby
    "x".rmatch? do _not "x" end # => nil

    "y".rmatch? do _not "x" end

!SLIDE
# Grouping #

!SLIDE
# Works the old way #

!SLIDE ruby
    @@@ ruby
    match = "abc".rmatch do
        group( "a" + group( "b" ) ) + "c"
    end

    match.to_a #=> [ "abc", "ab", "b" ]

!SLIDE
# Named groups #

!SLIDE ruby
    @@@ ruby
    match = "abcde".rmatch? do
        group( :word, any( word_char ) )
    end

    match[ :word ] #=> "abcde"

!SLIDE
# Or use blocks #

!SLIDE ruby
    @@@ ruby
    match = "abcde".rmatch? do
        group :word do
            any word_char
        end
    end

    match[ :word ] #=> "abcde"

!SLIDE ruby
    @@@ ruby
    order = "1234567890       Central Processing"

    match = order.rmatch? do
        group :serial do
            some digit
        end \
        + some whitespace + \
        group :source do
            any any_char
        end
    end

    puts "Order #" + match[ :serial ]
    puts "Shipped from " + match[ :source ]
