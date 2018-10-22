---
layout: post
title: Opgaven Dag 2
---

> Write a sort that takes a list and returns a sorted list.

```
module MySort where
  mySort [] = []
  mySort (h:t) = (mySort k) ++ [h] ++ (mySort g)
    where
      k = filter (< h) t
      g = filter (> h) t
```

Deze implementatie lijkt op de Quicksort en Mergesort die wij in de course behandeld hebben, hij blijft het probleem verkleinen totdat er lijsten van 1 groot zijn, deze worden samengevoegd en uiteindelijk is lijst hierdoor gesorteerd. 

> Write a sort that takes a list and a function that compares its two arguments and then returns a sorted list.  

Deze vraag heb ik oprecht een tijd overnagedacht en ik heb hier geen oplossing voor. 

>  Write a Haskell function to convert a string to a number. The string should be in the form of $2,345,678.99 and can possibly have leading zeros.

```
module ToDouble where
  toDouble (h:t) = read (filter (/= ',') t) :: Double
```
Door het gebruik van [de read function](http://zvon.org/other/haskell/Outputprelude/read_f.html) is het mogelijk om een string naar een ander datatype te casten. Echter werkt deze functie niet met komma's. Een string is een lijst van characters, met de filter functie worden alle komma characters dus uit de lijst gefiltered.

> Write a function that takes an argument x and returns a lazy sequence that has every third number, starting with x . Then, write a function that includes every fifth number, beginning with y. Combine these functions through composition to return every eighth number, beginning with x + y.

```
module Multiples where
    multiples x y = [x, x+y ..]
    threes = take 5 (multiples 1 3)
    fives = take 5 (multiples 1 5)
    eights = zipWith (+) threes fives
```
Deze opgave was relatief makkelijk, echter begreep ik niet wat er met de laatste zin bedoeld werd... [hier](https://github.com/kikito/7-languages-in-7-weeks/blob/master/7-haskell/day-2/lazy.hs) is een uitwerking van iemand andres, daar werd de zipWith functie gebruikt. Ik betwijfel of dit de bedoeling was, maar het lijkt te werken. ish...

>Use a partially applied function to define a function that will return half of a number and another that will append \n to the end of any string.


```
module Partially where
  halve x = (x/2) 
  append x = reverse ('n':'\\':(reverse x))
```
