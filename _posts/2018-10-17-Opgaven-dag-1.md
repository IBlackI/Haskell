---
layout: post
title: Opgaven dag 1
---
# How many different ways can you find to write allEven ?

```Haskell
Prelude> let input = [1..12]
Prelude> [x | x <- input, even x]
[2,4,6,8,10,12]
```

Hiernaast heb ik ook geprobeerd een uitwerking doormiddel van guards te maken, echter lukte dit niet.

# Write a function that takes a list and returns the same list in reverse.

```Haskell
module Reverse where
  myReverse :: [Integer] -> [Integer]
  myReverse [] = []
  myReverse (h:t) = myReverse(t):h
```
Helaas bleek deze oplossing niet te werken. De `:` operator is om een element aan de voorkant van een lijst toe tevoegen, en zoals in dit geval werkt het toevoegen van een lijst aan een nummer niet.
De oplossing hiervoor is het gebruik van de `++` operator. Het resultaat is onderstaande uitwerking.

```Haskell
module Reverse where
  myReverse :: [Integer] -> [Integer]
  myReverse [] = []
  myReverse (h:t) = myReverse(t) ++ [h]
```
Als resultaat:
```Haskell
*Reverse> let list = [1..12]
list :: (Num a, Enum a) => [a]
*Reverse> myReverse list
[12,11,10,9,8,7,6,5,4,3,2,1]
it :: [Integer]
```

# Write a function that builds two-tuples with all possible combinations of two of the colors black, white, blue, yellow, and red. Note that you should include only one of (black, blue) and (blue, black)
```Haskell
Prelude> let colors = ["Black", "White", "Blue", "Yellow", "Red"]
colors :: [[Char]]
Prelude>  [(a, b) | a <- colors, b <- colors, a < b]
[("Black","White"),("Black","Blue"),("Black","Yellow"),("Black","Red"),("White","Yellow"),("Blue","White"),("Blue","Yellow"),("Blue","Red"),("Red","White"),("Red","Yellow")]
it :: [([Char], [Char])]
Prelude> 
```

# Write a list comprehension to build a childhood multiplication table. The table would be a list of three-tuples where the first two are integers from 1–12 and the third is the product of the first two.
```Haskell
Prelude> [(a, b, a*b) | a <- [1..12], b <- [1..12]]
[(1,1,1),(1,2,2),(1,3,3),(1,4,4),(1,5,5),(1,6,6),(1,7,7),(1,8,8),(1,9,9),(1,10,10),(1,11,11),(1,12,12),(2,1,2),(2,2,4),(2,3,6),(2,4,8),(2,5,10),(2,6,12),(2,7,14),(2,8,16),(2,9,18),(2,10,20),(2,11,22),(2,12,24),(3,1,3),(3,2,6),(3,3,9),(3,4,12),(3,5,15),(3,6,18),(3,7,21),(3,8,24),(3,9,27),(3,10,30),(3,11,33),(3,12,36),(4,1,4),(4,2,8),(4,3,12),(4,4,16),(4,5,20),(4,6,24),(4,7,28),(4,8,32),(4,9,36),(4,10,40),(4,11,44),(4,12,48),(5,1,5),(5,2,10),(5,3,15),(5,4,20),(5,5,25),(5,6,30),(5,7,35),(5,8,40),(5,9,45),(5,10,50),(5,11,55),(5,12,60),(6,1,6),(6,2,12),(6,3,18),(6,4,24),(6,5,30),(6,6,36),(6,7,42),(6,8,48),(6,9,54),(6,10,60),(6,11,66),(6,12,72),(7,1,7),(7,2,14),(7,3,21),(7,4,28),(7,5,35),(7,6,42),(7,7,49),(7,8,56),(7,9,63),(7,10,70),(7,11,77),(7,12,84),(8,1,8),(8,2,16),(8,3,24),(8,4,32),(8,5,40),(8,6,48),(8,7,56),(8,8,64),(8,9,72),(8,10,80),(8,11,88),(8,12,96),(9,1,9),(9,2,18),(9,3,27),(9,4,36),(9,5,45),(9,6,54),(9,7,63),(9,8,72),(9,9,81),(9,10,90),(9,11,99),(9,12,108),(10,1,10),(10,2,20),(10,3,30),(10,4,40),(10,5,50),(10,6,60),(10,7,70),(10,8,80),(10,9,90),(10,10,100),(10,11,110),(10,12,120),(11,1,11),(11,2,22),(11,3,33),(11,4,44),(11,5,55),(11,6,66),(11,7,77),(11,8,88),(11,9,99),(11,10,110),(11,11,121),(11,12,132),(12,1,12),(12,2,24),(12,3,36),(12,4,48),(12,5,60),(12,6,72),(12,7,84),(12,8,96),(12,9,108),(12,10,120),(12,11,132),(12,12,144)]
it :: (Num c, Enum c) => [(c, c, c)]
```

# Solve the map-coloring problem
```
Prelude> let states = ["Tennesee", "Mississippi", "Alabama", "Georgia", "Florida"]
states :: [[Char]]
Prelude> let colors = ["Red", "Green", "Blue"]
Prelude> [(state, color) | state <- states, color <- colors]
[("Tennesee","Red"),("Tennesee","Green"),("Tennesee","Blue"),("Mississippi","Red"),("Mississippi","Green"),("Mississippi","Blue"),("Alabama","Red"),("Alabama","Green"),("Alabama","Blue"),("Georgia","Red"),("Georgia","Green"),("Georgia","Blue"),("Florida","Red"),("Florida","Green"),("Florida","Blue")]
it :: [([Char], [Char])]
```
Bovenstaande uitwerking geeft alle states met alle mogelijke colors terug.  Het blijkt echter niet mogelijk om bij list comprehension tussen verschillende entries te filteren, de filters werken alleen op de huidige entry. Om het met list comprehension te doen zal de lijst dus alle states en colors moeten bevatten in 1 entry.
Na wat puzzelen kwam ik helaas niet veel verder, gelukkig stond onderstaande uitwerking online
```Haskell
  colors = ["red", "green", "blue"]
  mapColoring = [ ("Tennesee", t, "Mississippi", m, "Alabama", a, "Georgia", g, "Florida", f) |
                  t <- colors, m <- colors, a <- colors, g <- colors, f <- colors,
                  m /= t, m /= a,
                  a /= t, a /= g, a /= f,
                  g /= f, g /= t ]
```

# Reflectie
De syntax van Haskell begint inmiddels duidelijk te worden. Het functioneel denken lukt voor veel problemen wel, echter blijf ik het vrij lastig vinden. 
Volgende blogpost zal over dag 2 uit het boek gaan.


