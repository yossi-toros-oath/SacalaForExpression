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
#### Lets start with a simple example 
```
val range = 1 to 10
for ( i <- range) println(i)
```
Or
We  write 1 to 5 it gives a range collection and the for expression iterates through it.
```
for ( i <- 1 to 5 ) println(i)
```
#### The body of the for loop is quite simple a just println lets move  lets add some code to it
```
val states = Map("AL" -> "Alabama", "AK" -> "Alaska")
val abrvs = List("AL", "AK", "NY")
for (abrv <- abrvs) {
  if (states.contains(abrv)) {
    println(states(abrv))
  } else {
    println("N/A")
  }
}
```

