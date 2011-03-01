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
    "abc".rmatch? { "ab" + "c" }

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? { "xyz".or "abc" }

!SLIDE
# Ever tried to search for "&"? #

!SLIDE ruby
    @@@ ruby
    "symbols.*&+galore".rmatch? { ".*&+" }

!SLIDE
# Precedence #

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? { "ab" + ( "z".or "c" ) }

!SLIDE
# Repetition #

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? { 1.or_more "a" }

    "aaabc".rmatch? { 5.or_less "a" }

    "aaabc".rmatch? { 3.exactly "a" }

    "aaabc".rmatch? { (1..5).of "a" }

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? { 0.or_more "a" }

    "aaabc".rmatch? { any "a" }

!SLIDE ruby
    @@@ ruby
    "aaabc".rmatch? { 1.or_more "a" }

    "aaabc".rmatch? { some "a" }

!SLIDE
# Character sets #

!SLIDE ruby
    @@@ ruby
    "abc1234.&*".rmatch? { 10.exactly any_char }

!SLIDE ruby
    @@@ ruby
    "abc1234".rmatch? { 3.exactly letter }

!SLIDE ruby
    @@@ ruby
    "abc1234".rmatch? { 4.exactly digit }

!SLIDE ruby
    @@@ ruby
    "abc_123".rmatch? { 7.exactly word_char }

!SLIDE ruby
    @@@ ruby
    " ".rmatch? { whitespace }

!SLIDE ruby
    @@@ ruby
    "abc".rmatch? { 3.exactly "a".."c" }

!SLIDE
# Negation #

!SLIDE ruby
    @@@ ruby
    "x".rmatch? { word_char.not "x" } # => nil

    "y".rmatch? { word_char.not "x" }

!SLIDE ruby
    @@@ ruby
    "x".rmatch? { _not "x" } # => nil

    "y".rmatch? { _not "x" }

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
