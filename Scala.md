# scala-cheatsheet

### Best Practice
#### Map

* Prefer immutable val over immutable var over mutable val over mutable var


### Quick examples


#### Scala Sequences
Original link: https://alvinalexander.com/scala/examples-scala-sequences-collection-methods-seq-list-array-buffer

##### Symbol methods
```
val evens = List(2, 4, 6)
val odds = List(1, 3, 5)
```

```
evens ++ odds          # List(2, 4, 6, 1, 3, 5)
evens ++: odds         # List(2, 4, 6, 1, 3, 5)
0 +: evens             # List(0, 2, 4, 6)
odds :+ 7              # List(1, 3, 5, 7)
0 :: evens             # List(0, 2, 4, 6)
evens :: odds          # List(List(2, 4, 6), 1, 3, 5)
evens ::: odds         # List(2, 4, 6, 1, 3, 5)
(0 /: evens)(_ + _)    # 12
(evens :\ 0)(_ + _)    # 12
```

##### More methods about sequence
```
val a = "foo bar baz"
val foo = "foo"
val bar = "bar"
val names = List("Al", "Christina", "Kim")
val firstTen = (1 to 10).toList
val fiveToFifteen = (5 to 15).toList
```
```
foo * 3                            # foofoofoo
a.capitalize                       # Foo bar baz

# collect and collectFirst take a partial function
firstTen.collect{case x if x % 2 == 0 => x}   # List(2, 4, 6, 8, 10)
firstTen.collectFirst{case x if x > 1 => x}   # Some(2)

a.count(_ == 'a')                  # 2

a.diff("foo")                      # " bar baz"
a.distinct                         # "fo barz"
a.drop(4)                          # "bar baz"
a.dropRight(2)                     # "foo bar b"
a.dropWhile(_ != ' ')              # " bar baz"

a.endsWith("baz")                  # true
evens.exists(_ > 2)                # true

a.filter(_ != 'a')                 # "foo br bz"
a.filterNot(_ != 'a')              # "aa"
a.filterNot(_ == 'a')              # "foo br bz"
firstTen.find(_ > 4)               # Some(5)
names.flatten                      # List(A, l, C, h, r, i, s, t, i, n, a, K, i, m)
names.flatMap(_.toUpperCase)       # List(A, L, C, H, R, I, S, T, I, N, A, K, I, M)
firstTen.fold(0)(_ + _)            # 55
firstTen.foldLeft(0)(_ - _)        # -55
firstTen.foldRight(0)(_ - _)       # -5
evens.forall(_ >= 2)               # true
a.foreach(println(_))              # prints one character per line
a.foreach(println)                 # prints one character per line
a.getBytes.foreach(println)        # prints the byte value of each character, one value per line

firstTen.groupBy(_ > 5)            # Map(false -> List(1, 2, 3, 4, 5), true -> List(6, 7, 8, 9, 10))
firstTen.grouped(2)                # Iterator[List[Int]] = non-empty iterator
firstTen.grouped(2).toList         # List(List(1, 2), List(3, 4), List(5, 6), List(7, 8), List(9, 10))
firstTen.grouped(5).toList         # List(List(1, 2, 3, 4, 5), List(6, 7, 8, 9, 10))

firstTen.hasDefiniteSize           # true
firstTen.toStream.hasDefiniteSize  # false (changes to 'true' after you consume the stream)
a.head                             # f
a.headOption                       # Some(f)

a.indexOf('a')                     # 5
firstTen.indexOf(5)                # 4
firstTen.indexOfSlice(Seq(4,5,6))  # 3
firstTen.indexWhere(_ == 5)        # 4
firstTen.indices                   # Range(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
"foo".indices                      # Range(0, 1, 2)
"foo bar".init                     # "foo ba"

"foo".isDefinedAt(1)               # true
"foo".isDefinedAt(3)               # false

# isEmpty
List(1,2,3).isEmpty                # false
Nil.isEmpty                        # true
None.isEmpty                       # true
Some(1).isEmpty                    # false

firstTen.intersect(fiveToFifteen)                  # List(5, 6, 7, 8, 9, 10)
a.isEmpty                                          # false

List(1,2,3).last                                   # 3
a.lastIndexOf('o')                                 # 2
List(1,1,2,2,1,1,3,3).lastIndexOfSlice(Seq(1,1))   # 4
List(1,1,2,2,1,1,3,3).lastIndexWhere(_ == 1)       # 5
List(1,2,3).lastOption                             # Some(3)
a.length                           # 11

a.map(_.toUpper)                   # FOO BAR BAZ
a.map(_.byteValue)                 # Vector(102, 111, 111, 32, 98, 97, 114, 32, 98, 97, 122)
names.max                          # "Kim"
names.min                          # "Al"
a.mkString(",")                    # f,o,o, ,b,a,r, ,b,a,z
a.mkString("->", ",", "<-")        # ->f,o,o, ,b,a,r, ,b,a,z<-

a.nonEmpty                         # true

a.par                              # a parallel array, ParArray(f, o, o,  , b, a, r,  , b, a, z)
a.partition(_ > 'e')               # (foorz, " ba ba")  # a Tuple2
firstTen.partition(_ > 5)          # (List(6, 7, 8, 9, 10),List(1, 2, 3, 4, 5))

firstTen.reduce(_ + _)             # 55
firstTen.reduceLeft(_ - _)         # -53
firstTen.reduceRight(_ - _)        # Int = -5
a.replace('o', 'x')                # fxx bar baz
a.replace("o", "x")                # fxx bar baz
a.replaceAll("o", "x")             # fxx bar baz
a.replaceFirst("o", "x")           # fxo bar baz
firstTen.reverse                   # List(10, 9, 8, 7, 6, 5, 4, 3, 2, 1)

# (more on scan, scanLeft, and scanRight below)
Seq(1,2,3,4,5).scan(1)(_ + _)                     # List(1, 2, 4, 7, 11, 16)
Seq(1,2,3,4,5).scanLeft(1)(_ + _)                 # List(1, 2, 4, 7, 11, 16)
Seq(1,2,3,4,5).scanRight(1)(_ + _)                # List(16, 15, 13, 10, 6, 1)

# segmentLength
List(1,2,3,4,5,4,3,2,1).segmentLength(_ > 3, 0)   # 0
List(1,2,3,4,5,4,3,2,1).segmentLength(_ > 3, 1)   # 0
List(1,2,3,4,5,4,3,2,1).segmentLength(_ > 3, 2)   # 0
List(1,2,3,4,5,4,3,2,1).segmentLength(_ > 3, 3)   # 3
List(1,2,3,4,5,4,3,2,1).segmentLength(_ > 3, 4)   # 2

a.size                             # 11
a.slice(0,5)                       # foo b
a.slice(2,9)                       # o bar b

firstTen.sliding(2)                # Iterator[List[Int]] = non-empty iterator
firstTen.sliding(2).toList         # List(List(1, 2), List(2, 3), List(3, 4), List(4, 5), List(5, 6), List(6, 7), List(7, 8), List(8, 9), List(9, 10))
firstTen.sliding(4).toList         # List(List(1, 2, 3, 4), List(2, 3, 4, 5), List(3, 4, 5, 6), List(4, 5, 6, 7), List(5, 6, 7, 8), List(6, 7, 8, 9), List(7, 8, 9, 10))
firstTen.sliding(2,2).toList       # List(List(1, 2), List(3, 4), List(5, 6), List(7, 8), List(9, 10))
firstTen.sliding(2,3).toList       # List(List(1, 2), List(4, 5), List(7, 8), List(10))
firstTen.sliding(2,4).toList       # List(List(1, 2), List(5, 6), List(9, 10))

a.sortBy                           # this is a bit long; see below
a.sortWith(_ < _)                  # "  aabbfoorz"
a.sortWith(_ > _)                  # "zroofbbaa  "
a.sorted                           # "  aabbfoorz"

firstTen.span(_ < 5)               # (List(1, 2, 3, 4),List(5, 6, 7, 8, 9, 10))
a.split(" ")                       # Array(foo, bar, baz)
a.splitAt(3)                       # (foo," bar baz")
firstTen.startsWith(Seq(1,2))      # true

a.tail                             # oo bar baz
a.take(3)                          # foo
a.takeRight(3)                     # baz
a.takeWhile(_ != 'r')              # foo ba
a.toArray                          # Array(f, o, o,  , b, a, r,  , b, a, z)
a.toBuffer                         # ArrayBuffer(f, o, o,  , b, a, r,  , b, a, z)
a.toList                           # List(f, o, o,  , b, a, r,  , b, a, z)
Seq(1,1,2,2,3,3).toSet             # Set(1, 2, 3)
firstTen.toStream                  # scala.collection.immutable.Stream[Int] = Stream(1, ?)
a.toLowerCase                      # foo bar baz
a.toUpperCase                      # FOO BAR BAZ
a.toVector                         # Vector(f, o, o,  , b, a, r,  , b, a, z)
a.trim                             # "foo bar baz"

evens.union(odds)                  # List(2, 4, 6, 1, 3, 5)
unzip                              # see below
Seq(1,2,3).updated(0,10)           # List(10, 2, 3)

firstTen.view                      # scala.collection.SeqView[Int,List[Int]] = SeqView(...)

firstTen.withFilter(_ > 5)                    # scala.collection.generic.FilterMonadic[Int,List[Int]]
firstTen.withFilter(_ > 5).map(_ * 1)         # List[Int] = List(6, 7, 8, 9, 10)

a.zip(0 to 10)                                # Vector((f,10), (o,11), (o,12), ( ,13), (b,14), (a,15), (r,16), ( ,17), (b,18), (a,19), (z,20))
Seq(1,2,3).zipAll(Seq('a', 'b'), 0, 'z')      # List((1,a), (2,b), (3,z))
Seq(1,2).zipAll(Seq('a', 'b', 'c'), 0, 'z')   # List((1,a), (2,b), (0,c))
a.zipWithIndex                                # Vector((f,0), (o,1), (o,2), ( ,3), (b,4), (a,5), (r,6), ( ,7), (b,8), (a,9), (z,10))
```
