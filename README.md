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

#The body of the for loop is quite simple a just println lets move  lets add some code to it
val states = Map("AL" -> "Alabama", "AK" -> "Alaska")
for ( country <- states.keys
	country match {
		case "India" => println("Delhi")
		case "USA"   => println("Washington D.C.")
		case "Japan" => println("Tokyo")
		case _ => println("I don't know")
	}
}

# Question : 
# Given this list can we genereate the same output without a for loop
val countries = List("India", "USA", "China", "Japan")

# All you need to do is to print the capital of the country in the list
countries.foreach { country =>
	country match {
		case "India" => println("Delhi")
		case "USA"   => println("Washington D.C.")
		case "Japan" => println("Tokyo")
		case _ => println("I don't know")
	}
}

# The for loop is just a syntactic sugar for higher order conrol abstractions.
# The scala compiler will convert the for loop to one of the following control abstractions.
# 	1. foreach
#		2. map
#		3. flatMap
#		2. withFilter
# In other words scala does not have a for loop it is systactic sugar for these methods.
# The real purpose of the scala for expression is to write the code in a way that makes more sense.You should use the for expression when you think your code is getting too cryptic using those methods, and it makes more sense using a for expression.

# Once you know that for is a syntactic sugar it beacomes easy understanding the for expression. Lets bring the "yield" back to the structure.In the absance of the yield for behaves like the foreach method.

for ( country <- List("India", "USA", "China", "Japan")) yield {
	country match {
		case "India" => "Delhi"
		case "USA"   => "Washington D.C."
		case "Japan" => "Tokyo"
		case _ => "I don't know"
	}
}

#In this case the for loops behaves like the map function


# As I said the most basic type of the for loop generator is  in the following form e <- col, but scala for expression permits multiple generators each additional genereator will cause to an additional nested loop.
for ( i <- 1 to 3; j <- 1 to 2) println(s"i=$i j=$j")

# You can extend the nesting further
for ( i <- 1 to 3; j <- 1 to 2; k <- 1 to 2) println(s"i=$i j=$j k=$k")

# If the code between parantheses getting too complex you can use curly braces
for {
	i <- 1 to 3; 
	j <- 1 to 2; 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")

# Semi colons are optional
	for {
	i <- 1 to 3 
	j <- 1 to 2 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")

# For each generartion we can apply a filter
# Putting a filter to one of the generators
# A filter to eliminate the odd numbers
for {
	i <- 1 to 3 
	j <- 1 to 2; if(j%2==0) 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")

#You can also write it like this one expression on each line
for {
	i <- 1 to 3 
	j <- 1 to 2
	if(j%2==0) 
	k <- 1 to 2 } println(s"i=$i j=$j k=$k")


# Lets take a better example to understand the power and the flexibility of the for expression
# I have this file

emp.txt
employee_id, first_name, last_name,email,phone_number,hire_date,job_id,salary,commision_pct,maanger_id,deparment_id
100,Seteven,King,SKING,515.103.4567,17-JUN-1987,AD_PRES,2400,NULL,NULL,90
101,Neena,Kochar,NKOCHAR,515.193.4568,21-SEP-1989,AS_VP,17000,NULL,100,90
102,Lex,De Haan,LDEHAAN,515.183.4569,13-JAN-1993,AS_VP,17000,NULL,100,90
103,Alex,Honnold,AHUNNOLD,515.173.4567,03-JAN-1990,IT_PROG,9000,NULL,102,60
104,Bruce,Ernst,BERNST,515.163.4568,21-MAY-1991,IT_PROG,6000,NULL,103,60
105,David,Austin,DAUSTIN,515.153.4569,25-JUN-1997,IT_PROG,4800,NULL,103,60
106,Valli,Pataballa,VPATABALLA,515.143.4560,05-FEB-1998,IT_PROG,4800,NULL,103,60
107,Diana,Lorentz,DLORENTZ,515.133.5567,07-FEB-1999,IT_PROG,4200,NULL,103,60
108,Nancy,Greenberg,NGRENBERG,514.123.4569,17-AUG-1994,FI_MGR,12000,NULL,101,100
109,Daniel,Faviet,DFAVIET,515.113.4169,16-AUG-1994,FI_ACCOUNT,9000,NULL,108,100


import scala.io._
for (line <- Source.fromFile("emp.txt").getLines().toList) println(line)

# This for expression prints all off the records
# But I am not intrested on all of these fields, I just want to see employee_id, first_name, last_name, department_id
import scala.io._
for (line <- Source.fromFile("emp.txt").getLines().toList)  {
	val fields = line.split(",")
	println(fields(0) + "--" +fields(1)+"--"+fields(2)+"--"+fields(10))
}
# As you see here the for loop also allows you to embed and assignement as part of the genereator
# Same as we embed filer for the generator
# So the code should look like this
import scala.io._
for { 
    line <- Source.fromFile("emp.txt").getLines().toList 
    fields = line.split(",")
} println(fields(0) + "--" +fields(1)+"--"+fields(2)+"--"+fields(10))
# The idea is expand the for expression instead of expanding the body as simple as possible
# assignment inside the for  (not in the body)


# Task I am only intested in depertment 60 can you filter out others ?
import scala.io._
for { 
    line <- Source.fromFile("emp.txt").getLines().toList 
    fields = line.split(",")
    if(fields(10)=="60")
} println(fields(0) + "--" +fields(1)+"--"+fields(2)+"--"+fields(10))

#Lets bring yield back

for { 
    line <- Source.fromFile("emp.txt").getLines().toList 
    fields = line.split(",")
    if(fields(10)=="60")
} yield (fields(0),fields(1), fields(10))


# Recaping the basics
# The syntax of for expression in scala is
# for (seq) yield { expr }
# In the seq part you can define
1. A Generator - A collection
2. A Filter  - An if contition
3. A definition -  An assignment


# Syntacti sugar translation in details


#Exception chanining without for
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

#Exception chaning with for
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





