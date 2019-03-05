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
```scala
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
## Lets take a better example to understand the power and the flexibility of the for expression
Define the people csv string
```
val people ="""first_name,last_name,addres,city,state,zipcode
John,Doe,120 jefferson st.,Riverside, NJ, 08075
Jack,McGinnis,220 hobo Av.,Phila, PA,09119
John Da Man,Repici,120 Jefferson St.,Riverside, NJ,08075
Stephen,Tyler,7452 Terrace At the Plaza road,SomeTown,SD, 91234
Mike,Anne,9th Terrace plc,Desert City,CO,00123"""
```

### Print the csv line by line
```
for (line <- people.split("\\n").toList) println(line)
```
### Print first_name, last_name, state fields only
```
for (line <- people.split("\\n").toList) {
	val fields = line.split(",")
     	println(fields(0) + "," + fields(1) + "," + fields(4))
}
```
### Make it more ala-scala
The idea is expand the for expression instead of expanding the body as simple as possible assignment inside the for  (not in the body)
```
for (line <- people.split("\\n").toList) {
	fields = line.split(",")   	
} println(fields(0) + "," + fields(1) + "," + fields(4))
```

### Print first_name, last_name, state fields only where the state is NJ
```
for {
  line <- people.split("\\n").toList
  fields = line.split(",")
  if (fields(4).trim == "NJ")
} println(fields(0) + "," + fields(1) + "," + fields(4))
```

### Do not print generete a list of tuples
```
for {
  line <- people.split("\\n").toList
  fields = line.split(",")
  if (fields(4).trim == "NJ")
} yield (fields(0) , fields(1) , fields(4))
```
## Recaping the basics
### The syntax of for expression in scala is
### for (seq) yield { expr }
### In the seq part you can define
1. A Generator - A collection
2. A Filter  - An if contition
3. A definition -  An assignment

## Exception chanining without for
```
def run(args: Array[String]): Try[Unit] = {

        val parsedArgs = verifyArgs(args)
        if (parsedArgs.isFailure) {
          return Failure(parsedArgs.failed.get)
        }
      
        val config = loadConfig(parsedArgs.get.environment)
        if(config.isFailure) {
          return Failure(config.failed.get)
        }
      
        val session = buildSparkSession(config.get)
        if (session.isFailure) {
          return Failure(session.failed.get)
        }
      
        val (sparkSession, sparkMetrics, metricsClient) = session.get
      
        val params = buildParams(parsedArgs.get, config.get)
        
        if(params.isFailure) {
          return Failure(params.failed.get)
        }

        measure(params.get, sparkSession, sparkMetrics, metricsClient)
    }
  }
```

## Exception chaning with for
```
def run(args: Array[String]): Try[Unit] = {
      for {
        parsedArgs <- verifyArgs(args)
        config <- loadConfig(parsedArgs.environment)
        (sparkSession, sparkMetrics, metricsClient) <- buildSparkSession(config)
        params <- buildParams(parsedArgs, config)
        executionResult <- measure(params,
                                   sparkSession,
                                   sparkMetrics,
                                   metricsClient)
      } yield executionResult
    }
```









