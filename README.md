# SacalaForExpression
__For expression is a swiss army knife for iterations__

## Genereal Syntax :
```
for ( sequence(s)/[filter(s)]/[assignment(s)] ) yield { expr }
```
### Lets keep the yield aside (for the moment)
```
for ( seqeunce(s)/[filter(s)]/[assignment(s)]){ expr }
```
Lets start as simple as possible 
```
val range = 1 to 10
for ( i <- range) println(i)
```
Or
We  write 1 to 5 it gives a range collection and the for expression iterates through it.
```
for ( i <- 1 to 5 ) println(i)
```
### Lets keep the yield aside (for the moment)
