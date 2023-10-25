# golf
A repo where I share sollutions I have found to golfing problems. 

## SOLIS-ID
Take a 7 digit number. Multiply the first number with 7, the second with 6 until you hit the last digit which you multiply with 1. Sum up all the results. If the sum is divisible by 11 the number is correct, if it is not you don't have a valid number or have made an error.

This is how the Student ID's of Utrecht University are checksumed. It has a similar structure to how ISBN's work. 

### python
```py
python -c "print(sum(int(x)*(7-i) for i,x in enumerate(input()))%11==0)"
```

### Perl
This one is kinda dumb since it uses a certain strategy where I wanted to first construct an expression and then eval it.
```pl
perl -ne'@d=split//;print eval("(". join("+", map{$d[$_]*7-$_)}0..6).")%11")."\n";'
```

### APL 
```APL
Sn ← {~11|+/(⌽⍳7)×⍎¨⍕⍵}
```
There are two problems with this one:
1. `~` does not work the same way as it would in Python for example.
   While APL does have a falsy and truthy 0 | 1, it is only that.
   Truthiness does not extend to 2 | 3 | etc..., like it would for example in language like lisp?, python, bash[^1] or JS.
   So we cannot do this hack. 
2. The multiplication is redundant. We can simplify.

[^1]: where it is reversed. i.e. 1, 2, 3 etc are falsy. Actually a better way if you think about it. (Since there is one and only one _correct_ way, but there are many ways to err). 

We can simplify using a cumulative sum:
```APL
Sn ← {0=11|+/+\⍎¨⍕⍵}
```
Credit goes to [Dzaima](https://aplwiki.com/wiki/Dzaima) over at ["The APL Farm"](https://aplwiki.com/wiki/APL_Farm). 
This exploits a fact illustrated by the following "proof", written by a friend of mine who studies math:
```
n*a + (n-1)*b + (n-2)*c + ...
=
a + a + ... + a (n times)
+
b + b + ... + b (n-1 times)
+
c + c + ... + c (n-2 times)
+
...
=
a
+
(a + b)
+
(a + b + c)
+
....
(n terms, of which a is in all n of them,
                   b is in (n-1) of them, ...)
```
Translating that back to Python we get:
```py
python -c "i=0;print(sum(i:=i+int(x)for x in input())%11==0)"
```
(Credit goes to `ovs` or `@skrao` over at the APL farm. )

After having posted the same problem to the Code Golf Discord, we got a few candidates:
```py
print(eval('+(i:=i+%s)'*7%(*input(i:=0),))%11<1)
```
Credit goes to @biz14. It uses a similar strategy as my own perl attempt, by constructing an expression and then evaluating it.
 
