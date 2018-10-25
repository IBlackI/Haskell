---
layout: post
title: Opgaven dag 3
---

# Hashmap
Na wat basis zoekwerk lijkt het er op dat Haskell al een module voor een [HashTable](https://hackage.haskell.org/package/base-4.2.0.2/docs/Data-HashTable.html) heeft. Gezien het bij dit onderwerp over Maybe Monads gaat vermoed ik dat het de bedoeling is deze te gebruiken in plaats van zelf een implementatie voor een HashTable te maken.

> Write a function that looks up a hash table value that uses the Maybe monad. Write a hash that stores other hashes, several levels deep. Use the Maybe monad to retrieve an element for a hashkey several levels deep.

```HASKELL
module Hashed where 
  import Data.Map (Map)
  import qualified Data.Map as Map
  
  ages :: Map String Integer
  ages = Map.fromList [("Joe", 35), ("Mary", 37), ("Irma", 16)]
  
  findAge :: String -> String
  findAge name = case Map.lookup name ages of
                 Nothing  -> "I don't know the age of " ++ name ++ "."
                 Just age -> name ++ " is " ++ show age ++ " years old."
```
Bovenstaande code is een verbetering van de code in [dit stackoverflow antwoord (waar ik de code ook weer verbeterd heb).](https://stackoverflow.com/a/20505647) Die code is al in staat om in de functie `findAge` een Maybe Monad te gebruiken en als mogelijk de value bij een key terug te geven. 
```HASKELL
module Hashed where 
  import Data.Map (Map)
  import qualified Data.Map as Map
  
  ages :: Map String Integer
  ages = Map.fromList [("Joe", 35), ("Mary", 37), ("Irma", 16)]
  
  findAge :: String -> String
  findAge name = case Map.lookup name ages of
                 Nothing  -> "I don't know the age of " ++ name ++ "."
                 Just age -> name ++ " is " ++ show age ++ " years old."

  nestedHashes :: Map String String
  nestedHashes = Map.fromList [("Hello", "World"), ("World", "This"), ("This", "That"), ("That", "Foo"), ("Foo", "Bar"), ("Adam", "Savage")]
  
  findHashed :: String -> String
  findHashed hash = case Map.lookup hash nestedHashes of
                 Nothing -> hash
                 Just string -> findHashed(show string)
```

Bovenstaande uitwerking is een uitbreiding op de eerste, deze heeft een functie `findHashed` die door gebruik van een Maybe Monad net zo lang door een HashTable blijft gaan totdat hij niet meer dieper kan. Echter denk ik niet dat ik het Hash gedeelte hier juist in verwerkt heb aangezien dit ook met een gewone Map zou kunnen, maar op de [hackage](http://hackage.haskell.org/package/hashmap-1.3.3/docs/Data-HashMap.html#t:Map) pagina staat dat een HashMap deprecated is en je een Map moet gebruiken. Ik denk dat het deze oplossing niet helemaal is wat er bedoeld was, maar ik vind hem voldoende.

# Maze
>  Represent a maze in Haskell. You’ll need a Maze type and a Node type, as well as a function to return a node given its coordinates. The node should have a list of exits to other nodes.

Voordat ik aan deze opdracht begin: Deze (samen met de volgende) opdracht klinkt lastiger dan wat ik als Challenge zou doen, ik heb bijna geen idee waar ik zou moeten beginnen, laat staan een algoritme schrijven die dit kan oplossen.

Als eerste is het bepalen wat een Maze nou eigenlijk is. Volgens de opdracht is een Maze een type, en een Node is een type met een lijst van exits. Ook moet er een functie zijn die op basis van coordinaten een Node terug kan geven. Hieruit kan ik concluderen dat de lijst van exits dus een lijst van de coordinaten van de volgende nodes zijn. `node = [Exit]` De exit was de locatie van de volgende node, oftewel een coordinaat `Exit = (Int, Int)`. De maze zou volgens mij een Map kunnen zijn waarbij de Exit de key van een Node kan zijn. `Maze = Map Exit Node`. Dat zou er dan volgens mij zo uit moeten komen te zien:
``` HASKELL
module Maze where 
  import Data.Map (Map)
  import qualified Data.Map as Map
  
  type Exit = (Int, Int)
  type Node = Maybe [Exit]
  
  maze :: Map Exit Node
  maze = Map.fromList [((0, 0), Just [(0,1), (1,0)]), ((1,0), Nothing), ((0,1), Just [(1,1), (0,2)]), ((1,1), Nothing), ((0,2), Just [(0,3)]), ((0,3), Just [(0,4), (1,4)]), ((0,4), Nothing), ((1,4), Just [(1,5)]), ((1,5), Nothing)]
```