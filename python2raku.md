## NEW ENTRY

Python uses the comparison operators for sets.
```
{1, 2, 3} < {1, 2, 3, 4, 5}  #True
{1, 5} < {1, 2, 3, 4, 5}     #True
{2, 3} < {2, 3}              #False
```

Raku uses the right operators instead.
```
42 ∈ 1..100;                 # True
(42, 66, 31)  ⊂ 1..100;      # True
(42, 666, 31) ⊂ 1..100;      # False
```

But all of them also have ascii alias, as shown in the documentation… but I usually just use ctrl+k on vim…

## NEW ENTRY

Remove punctuation in Python.

```
use string
use dict

from string import punctuation
sentence = "Do we agree, that python is clean?"
sentence.translate(
	str.maketrans(
		dict.fromkeys(punctuation)
	)
)
# 'Do we agree that python is clean'
```

Raku knows if a character is a punctuation by it’s Unicode properties, so you don’t need a module for that:

```
say "Hey, how are you doing?".subst(/ <punct> /, :g);
# 'Hey how are you doing'
```
