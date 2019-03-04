# Sacala For Expression
__For expression is a swiss army knife for iterations__

## Genereal Syntax :
```
for ( sequence(s)/[filter(s)]/[assignment(s)] ) yield { expr }
```
## Lets keep the yield aside (for the moment)
```
for ( seqeunce(s)/[filter(s)]/[assignment(s)]){ expr }
```
#### A simple example 
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
#### Question : 
Can we genereate the same output without a for loop ?
```
abrvs.foreach { abrv =>
  if (states.contains(abrv)) {
    println(states(abrv))
  } else {
    println("N/A")
  }
}
```

The for loop is just a syntactic sugar for higher order conrol abstractions.
The scala compiler will convert the for loop to one of the following control abstractions.
1. foreach
2. map
3. flatMap
4. withFilter

In other words scala does not have a for loop it is systactic sugar for these methods.The real purpose of the scala for expression is to write the code in a way that makes more sense.You should use the for expression when you think your code is getting too cryptic using those methods, and it makes more sense using a for expression.Once you know that for is a syntactic sugar it beacomes easy understanding the for expression.

## Lets bring the "yield" back to the structure.
In the absance of the yield for behaves like the foreach method , now  the for loop will behave like the map method.
```
for (abrv <- abrvs)  yield {
  if (states.contains(abrv)) {
    states(abrv)
  } else {
    "N/A"
  }
}
```

```
abrvs.map { abrv =>
  if (states.contains(abrv)) {
    states(abrv)
  } else {
    "N/A"
  }
}
```
## Scala for expression permits multiple generators each additional genereator will cause to an additional nested loop.
```
for ( i <- 1 to 3; j <- 1 to 2) println(s"i=$i j=$j")
```

### If the code between parantheses getting too complex you can use curly braces
```
for {
	i <- 1 to 3; 
	j <- 1 to 2; 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")
```
### Semi colons are optional
```
for {
	i <- 1 to 3 
	j <- 1 to 2 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")
```
### For each generartion we can apply a filter
```
Putting a filter to one of the generators, a filter to eliminate the odd numbers
for {
	i <- 1 to 3 
	j <- 1 to 2; if(j%2==0) 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")
```
### You can also write it like this one expression on each line
```
for {
	i <- 1 to 3 
	j <- 1 to 2
	if(j%2==0) 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")
```
### Lets take a better example to understand the power and the flexibility of the for expression
Define the people csv string
```
val people ="""name,family_name,addres,city,state,zipcode
John,Doe,120 jefferson st.,Riverside, NJ, 08075
Jack,McGinnis,220 hobo Av.,Phila, PA,09119
John Da Man,Repici,120 Jefferson St.,Riverside, NJ,08075
Stephen,Tyler,7452 Terrace At the Plaza road,SomeTown,SD, 91234
Mike,Anne,9th Terrace plc,Desert City,CO,00123
"""
```
