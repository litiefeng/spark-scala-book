# sdfa

```scala
//通过不同的方式计算 wordcount, 并排序

val sc = getSparkContext()
val words = List.fill(100)("n" + Random.nextInt(5))
val wordRdd = sc.parallelize(words).map((_, 1))
println("-------通过reducebyKey----")
wordRdd.reduceByKey(_ + _).collect().sortBy(_._1).foreach(println)
println("-------groupByKey----")
wordRdd.groupByKey().map(t=> (t._1,t._2.sum)).collect().sortBy(_._1).foreach(println)
println("------foldByKey-----")
wordRdd.foldByKey(0)(_+_).collect().sortWith(_._1<_._1).foreach(println)

println("------aggregateByKey-----")
wordRdd.aggregateByKey(0)((u,v)=>u+v,(u1,u2)=>u1+u2)
  .collect().sortBy(_._1).foreach(println)
println("------combineByKey-----")
wordRdd.combineByKey[Int]((v:Int)=>v,(c:Int,v:Int)=>c+v,(v1:Int,v2:Int)=>v1+v2)
  .collect().sortBy(_._1).foreach(println)
```



