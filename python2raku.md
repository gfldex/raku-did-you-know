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
