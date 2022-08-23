That the Raku Programming Language supports the term τ (aka [`tau`](https://docs.raku.org/routine/tau)) as the ratio between the circumference and the radius of a circle? Just as it supports the term π (aka [`pi`](https://docs.raku.org/routine/pi)) as the ratio between the circumference and the diameter of a circle?

## NEXT ENTRY

That in the Raku Programming Language you can create [character classes](https://docs.raku.org/language/regexes#index-entry-regex_%3C[_]%3E-regex_%3C-[_]%3E-Enumerated_character_classes_and_ranges) that exclude certain characters? For instance, the character class `<[\d]-[7]>` would accept all numeric values, except 7. In natural language: create a character class by starting with `<[` consisting of all numeric values `\d]` except a characters class `-[` consisting of 7, ending the character class with `]>` .

## NEXT ENTRY

Sometimes you want to store the logic for some action in a variable, and then call that. By giving that variable the &-sigil, you don’t even need to specify that sigil when calling that logic. For example:

    my &surprise = Bool.pick
      ?? -> $value { $value * 2 }
      !! -> $value { $value / 2 }
    say surprise(42);  # 84 or 21

## NEXT ENTRY

That you can actually have a postfix condition (such as `if`) **and** a postfix iterator (such as `for`) in a single statement in the Raku Programming Language? An example showing all prime number from 1 to 10.

    .say if .is-prime for 1..10;
    2
    3
    5
    7

## NEXT ENTRY

That the [.head method](https://docs.raku.org/routine/head) can also be limited to number of elements from the **end**?

    say (1..5).head(*-2);  # (1 2 3)

And conversely, you can do the same with the [.tail method](https://docs.raku.org/routine/tail), to limit from the **beginning**:

    say (1..5).tail(*-2);  # (3 4 5)

## NEXT ENTRY

That you can use the [Z (zip)](https://docs.raku.org/routine/Z) operator to create `Pair`s by using `Z` as a meta-operator?

    say "a".."c" Z=> 1..*  # (a => 1 b => 2 c => 3)

And that you can use that to easily create a hash with mapping of strings to numerical values?

    my %hash = "a".."c" Z=> 1..*;
    say %hash;  # {a => 1, b => 2, c => 3}

## NEXT ENTRY

That you can start a `REPL` (aka a **Read Evaluate Print Loop**) while running code? By simply calling the `repl` subroutine you get a basic debugger with introspection. For instance:

    for ^5 -> $i {
        say $i;
        repl if $i == 2;
    }

The output (with user input in **bold**):

    0
    1
    2
    Type 'exit' to leave
    [0] > $i # bold
    2
    [1] > exit # bold
    3
    4

## NEXT ENTRY

That you can use the so-called “[capture markers](https://docs.raku.org/language/regexes#index-entry-regex_%3C(_)%3E-Capture_markers:_%3C(_)%3E)” `<(` and `)>` to indicate which part of the needle you actually want to capture in a match?

    say "foobarbaz" ~~ / foo <( \w+ )> baz /; # ｢bar｣

## NEXT ENTRY

That many phasers actually have a return value? You can e.g. use the `ENTER` phaser to “freeze” an entry value, and a `LEAVE` phaser to give you an adhoc idea of how long a scope took to execute:

given (2..5).pick {
    LEAVE say (now - ENTER now) ~ 'seconds  have passed';
    say "waiting $_ seconds";
    sleep $_;
}

## NEXT ENTRY

That you can use more than one adverb on hash slices? For instance, you can use the `:p` and `:delete` adverbs at the same time to easily split a hash into two hashes:

    my %h = a => 42, b => 666, c => 314, d => 737;
    my %s = %h<a c>:p:delete;

    say %h.kv;  # (d 737 b 666)
    say %s.kv;  # (a 42 c 314)

## NEXT ENTRY

That you can call the `.pairs` method on an array? It creates Pairs with the index as the key:

    my @a = <a b c d>;
    say @a.pairs; # (0 => a 1 => b 2 => c)

But maybe the `.antipairs` method is more useful tool, for instance to turn an array into a hash lookup:

    say @a.antipairs; # (a => 0 b => 1 c => 2)
    my %lookup = @a.antipairs;
    say %lookup<b>; # 1

## NEXT ENTRY

That if you want all defined values on sparsely populated arrays, you can do a [`.grep`](https://docs.raku.org/type/List#routine_grep):

    my @a;
    @a[3] = 42;
    @a[5] = 666;
    say @a.grep(*.defined); # (42 666)

But you can also use a [zen slice](https://docs.raku.org/language/subscripts#index-entry-Zen_slices) and the [`:v`](https://docs.raku.org/language/subscripts#index-entry-:v_(subscript_adverb)) adverb:

    say @a[]:v;  # (42 666)

The semantics between the `.defined` and `:v` are subtly different though: `.defined` checks the (virtual) value in the element, whereas `:v` will check for existence (in the case of arrays, this means “at least assigned to once, and not having been deleted with `:delete`“).

## NEXT ENTRY

That you can call the `.uniparse` method on a string with a unicode name, to get the associated grapheme? And to use the `.uniname` method to get back the name?

    say "butterfly".uniparse;  # 🦋
    say "🦋".uniname;  # BUTTERFLY

However, the given string must match exactly case-insensitive:

    say "love".uniparse;
    # Unrecognized character name [love]

Fortunately, there is a module called “[uniname-words](https://raku.land/zef:lizmat/uniname-words#synopsis)“, which (unsurprisingly) exports a subroutine called `uniname-words` that you can use to look up graphemes by (partial) name:

    use uniname-words;
    say "&chr($_) &uniname($_)" for uniname-words("love"); 
    # 🏩 LOVE HOTEL
    # 💌 LOVE LETTER
    # 🤟 I LOVE YOU HAND SIGN

The module also installs a script called “uw” so you can easily use it as a CLI. And you can also lookup with partial matching (instead of whole words).

## NEXT ENTRY

The simple approach to swapping two arrays, doesn’t work:

    my @a = 1,2,3; my @b = <a b c>;
    (@b, @a) = (@a,@b);
    say @a; say @b; 
    # []
    # (\Array_5056320939752 = [[] Array_5056320939752])

This is because the first array on the left-hand side will slurp the right-hand side, which causes `@b` in this example to become a self-referential structure (which is very hard to represent textually, hence the weird looking output), and `@a` to become empty. There **is** a way to do this, by thinking of the left-hand side as the signature of a subroutine, and the right hand side as the arguments:

    :(@b, @a) := (@a,@b);
    say @a; say @b;
    # [a b c]
    # [1 2 3]

The `:` prefix on the left hand side, makes `:(@b,@a)` a [Signature literal](https://docs.raku.org/type/Signature#Signature_literals), to which the arguments (the right hand side) are bound with `:=`. This process is called [destructuring](https://docs.raku.org/type/Signature#index-entry-destructuring_arguments). There are many other situations where you can use this! Kudos to [*Fernando Correa de Oliveira*](https://irclogs.raku.org/raku/2022-06-15.html#11:31-0005) for making yours truly remember.

## NEXT ENTRY

That there is a very succinct way of specifying an integer value with a named argument?

    say (:10times);  # times => 10

The syntax is colon, followed by an integer value, followed by the name of the named argument (in this case “times”). Note that the parentheses are needed to prevent it from being interpreted as a named argument to `say`.

The syntax exists because it could be considered more readable than the alternatives `times => 10 or :times(10)`. And you can take this even a step further for more readability:

    sub frobnicate(:st(:nd(:rd(:$th)))) { say $th }
    frobnicate :1st;  # 1
    frobnicate :2nd;  # 2
    frobnicate :3rd;  # 3
    frobnicate :4th;  # 4

Weird, isn’t it? Well, it is weird enough to actually make it to some of the core’s routines, such as (`subst`](https://docs.raku.org/routine/subst#Adverbs)!

## NEXT ENTRY

That you can refer to previously calculated values in the REPL?

    $ raku
    [0] > 42
    # 42
    [1] > 666
    # 666
    [2] > say $*0 + $*1
    # 708
    [2] > say @*_
    # (42 666)
    [2]

Note that each time a value has been calculated (without it having been explicitely displayed with e.g. `say`), the number before the prompt ( `>` ) is incremented. In any new expression, you can refer to these values by index: `$*0` (e.g. in say `$*0 + $*1`). The scalars are actually shortcuts to the underlying `@*_` array, which you can also access directly.

Please note that `$*0` (and friends) and `@*_` **only** exist in the REPL, and nowhere else!

## NEXT ENTRY

That you can use `map` as an alternative to `grep`?

    my @a = ^10;
    say @a.grep: * %% 2;  # (0 2 4 6 8)
    say @a.map: { $_ if $_ %% 2 }  # (0 2 4 6 8)

This is possible because a failed if (or `with` or `elsif` or etc…) will return `Empty`, which will cause the iterator to just skip that value. How can you check that? With [`do`](https://docs.raku.org/language/control#index-entry-control_flow_do-do)!

    dd do if 0 { }  # Empty

As to why you would use `map` over `grep`? Well, `map`‘s handling of `Empty` is highly optimized, causing the same code with `map` to be up-to 2x as fast as using `grep`. But note, this only affects the overhead of iterating: if your code inside the `grep` or `map` is very expensive, you won’t see much difference!

## NEXT ENTRY

That you can use the [hyper operator](https://docs.raku.org/language/operators#index-entry-hyper_%3C%3C-hyper_%3E%3E-hyper_%C2%AB-hyper_%C2%BB-Hyper_operators) `>>op>>` to run an operation on an array?

    my @a = ^5;
    say @a;          # [0 1 2 3 4]
    say @a >>/>> 2;  # [0 0.5 1 1.5 2]

But what if you want to change the contents of the array? Then you can use the assigning version of the operator used. In this case: `/=` instead of `/` :

    my @a = ^5;
    @a >>/=>> 2;
    say @a;  # [0 0.5 1 1.5 2]

Of course, you could also do `@a = @a >>/>> 2`, but that feels like you’re repeating yourself, `and` it is much less efficient. That’s because in the case of assignment by the hyperop, it does not need to create a result to be assigned to the array (because it can just return the left hand side of the hyperop).

## NEXT ENTRY

You can use the method `.none` (and friends) of `Any` in a method call chain?

    my \fib = 1, 1, * + * … ∞;
    say so fib[^10].none.is-prime;
    say so fib[^10].any.is-prime;
    say so fib[5..9].one.is-prime;

A `Junction` is a parallel value that wants to collaps into a single `Bool`. Any method call to a definite `Junction` will be forwarded to its members, coerced to `Bool` and collapsed with the `Junction`-type. The above `any`-form is equivalent to:

    say fib[^10].map(*.is-prime).reduce(&[||]);

[`Junctions`](https://docs.raku.org/type/Junction) come in many forms and can make your code quite neat.

## NEXT ENTRY

That there are two ways of indicating a `False` value with Raku’s standard interpretation of command line arguments?

    $ script-name --foo=False
    $ script-name --/foo

However, many people are used to be able to specify `--no-foo` as a way to indicate that the `foo` option is `False`. And this is not supported by Raku. Fortunately, there’s a simple trick that you can add to your script to allow this transparently:

    $_ .= subst(/^ '--no-' /, '--/') for @*ARGS;

This looks at the `raw` arguments (as available in `@*ARGS`) and changes any arguments starting with `--no-` to `--/` (which is an accepted format in Raku’s standard way of interpreting command line arguments). After that, it’s just as if users of your script typed `--/foo` instead of `--no-foo`.

## NEXT ENTRY

That when you’re using a `map`, you can return more than one value from the `Callable` block?

    $ say (^5).map({ $_, $_ + 1 })
    # ((0 1) (1 2) (2 3) (3 4) (4 5))

Well, actually in this case, it’s still a single value, which happens to be a `List`. However, you `can` really return multiple values, as long as you put them in a `Slip`.

    $ say (^5).map({ ($_, $_ + 1).Slip })
    # (0 1 1 2 2 3 3 4 4 5)

A `Slip` is a `List` that automatically flattens into an outer List (or other list-like container or iterable). You can also use the prefix pipe to create a `Slip`. In the above example, that would be: `|($_, $_ + 1)`.

## NEXT ENTRY

That you can spy on a chain of method calls when hunting a bug?

    (^10).grep(* %% 2).&{ $*ERR.say($_); $_ }.map(* ** 2).say;

This is also useful, if you need the output of more then one method-call in the same object.

    (1/10).&{ .numerator, .denominator }.say;

## NEXT ENTRY

That often there is no time to come up with a container name? In Raku many elements can be anonymous, including variables.

    react {
        whenever Supply.interval(1) {
            done if ++$ ≥ 5;
    
            once { say 'thinking'; next }
            say 'still thinking';
        }
        CLOSE { say 'done thinking'; }
    }


Containers that don't got a symbol-name can't be used in the wrong spot. Ideal for one-off counter and placeholders in [destructuring assignment](https://docs.raku.org/language/variables#index-entry-destructuring_assignment) and [`Signature`s](https://docs.raku.org/type/Signature#Signature_literals).

## PUBLISHED ENTRY (rakuweekly 2022-08-23)

That you can easily share raku data items with PHP code.

```
use Inline::Perl5;
use PHP::Serialization:from<Perl5> <serialize unserialize>;

my %a = %(a => 1, b => 2);          #{a => 1, b => 2}

my $php = serialize(%a);            #a:2:{s:1:"b";i:2;s:1:"a";i:1;}

my %b = unserialize($php);          #{a => 1, b => 2}
```

Inline::Perl5 is a simple way to access the vast library of Perl modules on [CPAN](https://www.cpan.org) (currently there are 208,288 modules listed).

