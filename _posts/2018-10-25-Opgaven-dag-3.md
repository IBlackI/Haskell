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
(De rest van deze blog is geschreven nadat de uitwerking van de maze niet werkte en met een groep tijdens de les is besproken)

## De maze laten oplossen. 
>  Use a List monad to solve the maze.
Kijk dat is lekker simpel! Even een algorimte in een nieuwe taal uitwerken waarbij ik de syntax soms nog nauwlijks snap... Goed, wie niet waagt wie niet wint. De maze zoals hierboven beschreven is slechts in een richting te lopen, ik hoef mij dus geen zorgen te maken dat het algorimte in een loop zal komen. Om de maze op te lossen zal een recursieve oplossing gebruiken. Onderstaand is de uitwerking in pseudocode:
```PSEUDO
  solveNode:
    (als "de node" geen exits heeft geef geen oplossing terug)
    voor de eerste Exit van "de node" controleer of deze de eindbestemming is,
      zo ja: geef deze Exit terug.
      zo nee: Probeer "de Exits" van deze Exit te vinden.
              als die er niet zijn: Probeer de volgende Exit van "de node"
              als die er wel zijn: Probeer solveNode met "de Exits"
                      als solveNode geen oplossing geeft: probeer de volgende Exit van "de node"
                      als solveNode wel een oplossing geeft: voeg de Exit van "de node" aan het resultaat toe
```
in Haskell ziet dat er dan zo uit. De functie `solveMaze` is een wrapper die er voor zorgt dat je met de locatie van de eerste node kan beginnen in plaats van met de exits van de eerste node. `start`, `middle` en `exit` zijn 3 locaties van nodes om makkelijk de functies aan te kunnen roepen. E.G. `solveMaze start maze end`
```HASKELL
module Maze where
  import Data.Map (Map)
  import qualified Data.Map as Map
  
  type Exit = (Int, Int)
  type Node = Maybe [Exit]
  type Maze = Map Exit Node
  
  maze :: Maze
  maze = Map.fromList [((0, 0), Just [(0,1), (1,0)]), ((1,0), Nothing), ((0,1), Just [(1,1), (0,2)]), ((1,1), Nothing), ((0,2), Just [(0,3)]), ((0,3), Just [(0,4), (1,4)]), ((0,4), Nothing), ((1,4), Just [(1,5)]), ((1,5), Nothing)]
  
  getNode :: Exit -> Maze -> Maybe [Exit]
  getNode location maze = case Map.lookup location maze of
                 Nothing -> Nothing
                 Just nde -> nde


  solveMaze :: Exit -> Maze -> Exit -> [Exit]
  solveMaze start m finish = case getNode start m of
                                  Nothing -> []
                                  Just n -> case solveNode n m finish of
                                            Nothing -> []
                                            Just solution -> solution
                                  
  solveNode :: [Exit] -> Maze -> Exit -> Maybe [Exit]
  solveNode [] _ _ = Nothing
  solveNode (h:t) m finish = if h == finish
                             then Just [finish]
                             else case getNode h m of
                                  Nothing -> solveNode t m finish
                                  Just n ->  case solveNode n m finish of
                                             Nothing -> Nothing
                                             Just e -> Just (h:e)
  start :: Exit
  start = (0, 0)
  
  middle :: Exit
  middle = (0, 2)
  
  end :: Exit
  end = (1, 5)
```

De aanroep `solveMaze start maze end` geeft terug 
```
*Maze> solveMaze start maze end
[(0,1),(0,2),(0,3),(1,4),(1,5)]
it :: [Exit]
```
hij heeft dus een oplossing gevonden!
## Een grotere maze!
Nu het algoritme lijkt te werken is het tijd om het wat lastiger te maken.
``` MAZE
((0,0), [(0,1)]),
((0,1), [(0,2)]),
((0,2), [(1,2)]),
((1,2), [(1,3), (2,2)]),
((1,3), [(1,4)]),
((1,4), [(2,4)]),
((2,0), Nothing),
((2,2), [(3,2)]),
((2,4), [(3,4)]),
((3,0), [(2,0), (4,0)]),
((3,1), [(3,0)]),
((3,2), [(3,1), (3,3), (4,2)]),
((3,3), [(3,4)]),
((3,4), [(3,5)]),
((3,5), [(4,5)]),
((4,0), Nothing),
((4,2), [(5,2)]),
((4,5), [(5,5)]),
((5,1), [(6,1)]),
((5,2), [(5,1)]),
((5,4), [(6,4)]),
((5,5), [(5,4)]),
((6,0), Nothing),
((6,1), [(6,0)]),
((6,3), Nothing),
((6,4), [(6,3)])
```
Bovenstaande is een mooie definitie van een maze, maar om je te helpen is hieronder een visualisatie van die definitie. 
![Maze](images/maze.png)
de definitie van die maze kan je gebruiken in haskell met deze regel code:
``` HASKELL
  largeMaze :: Maze
  largeMaze = Map.fromList [((0,0), Just [(0,1)]), ((0,1), Just [(0,2)]),((0,2), Just [(1,2)]),((1,2), Just [(1,3), (2,2)]),((1,3), Just [(1,4)]),((1,4), Just [(2,4)]),((2,0), Nothing),((2,2), Just [(3,2)]),((2,4), Just [(3,4)]),((3,0), Just [(2,0), (4,0)]),((3,1), Just [(3,0)]),((3,2), Just [(3,1), (3,3), (4,2)]),((3,3), Just [(3,4)]),((3,4), Just [(3,5)]),((3,5), Just [(4,5)]),((4,0), Nothing),((4,2), Just [(5,2)]),((4,5), Just [(5,5)]),((5,1), Just [(6,1)]),((5,2), Just [(5,1)]),((5,4), Just [(6,4)]),((5,5), Just [(5,4)]),((6,0), Nothing),((6,1), Just [(6,0)]),((6,3), Nothing),((6,4), Just [(6,3)])]
```
Het draaien van het `solveMaze (0,0) largeMaze (6,0)` op deze largeMaze geeft 
```
*Maze> solveMaze (0,0) largeMaze (6,0)
[(0,1),(0,2),(1,2),(2,2),(3,2),(4,2),(5,2),(5,1),(6,1),(6,0)]
it :: [Exit]
```
En dat klopt!
# Conclusie
Hoewel deze opgaven als zeer lastig op mij over kwamen ben ik er wel trots op dat ik deze heb weten op te lossen. Ik weet niet zeker of de hashtable opgave gemaakt is zoals bedoeld, maar de maze opgave(n) zijn zeker goed. De solveNode heeft veel moeite gekost om werkend te krijgen, echter is het na kritsch kijken met een groep wel gelukt. Het bedenken van de pseudocode viel wel mee, maar ik bleef vastlopen op verschillende Type errors. De Haskell compiler geeft wel aan wat hij krijgt en wat hij verwacht, maar dat vond ik toch lastig om te vertalen naar de locatie van het daadwerkelijke probleem. Dit doet mij wel denken aan het leren van Object Oriented programmeren in Java, daarbij kreeg je in de console ook errors als NullPointerException zonder dat ik wist waar dat door kon komen. Haskell vind ik een interessante taal, echter blijf ik de syntax zoals eerder in deze blog omschreven onduidelijk vinden. De opgave in het boek ben ik niet altijd uit gekomen, en zoals in het boek omschreven staat heeft Haskell geen grote community, hierdoor vond ik het ook vaak lastig om de antwoorden te vinden op problemen die ik tegen kwam. 

