---
layout: post
title: Spock's Great Strength
---
# Higher-Order Functions
## Anonymous Functions
Een tussen haakjes gedefinieerde functie waarvan de eerste parameter begint met een `\`. Zoals in het boek omschreven:  
`(\param1 .. paramn -> function_body)`

## map and where
### map
Een fuctie die voor elke waarde in een list een functie aanroept. 
```Haskell
map (\x -> x * x) [1, 2, 3]
```
In bovestaand voorbeeld roept map voor elke waarde in `[1,2,3]` de (anonieme) functie `(\x -> x * x)` aan.
### where
Is een manier om een lokale variabele of functie te defineeren. 
### section
Gedeelte van een functie, bijvoorbeeld `+1` dit zal als `x+1` gebruikt worden omdat `+` twee parameters nodig heeft en de eerste mist.
Als voorbeeld:
```Haskell
 map (+ 1) [1, 2, 3]
```

## filter, foldl, foldr
### filter
Een functie die een functie op elke waarde in een lijst toepast. Als de functie true terug geeft blijft de waarde in de lijst, zo niet wordt deze eruit gefilterd.
### foldl, foldr, fold, foldl'
Tja, de titel doet het al vermoeden, functies die allemaal het zelfde doen maar net een beetje anders. Er wordt geen uitleg geven over het nut van deze functies, maar na even googlen kom ik al snel tot de conclusie dat het allemaal functies zijn die een operatie op een lijst uit zouden moeten kunnen voeren. Echter lijken ze allemaal [een probleem](https://wiki.haskell.org/Foldr_Foldl_Foldl') te hebben. Mocht je deze functie dus gebruiken zal je blijkbaar eerst moeten onderzoeken welke van de varianten nou echt gaat werken. Dit is anders dan ik in een taal zoals Java, PHP, JS, Lua of C# gewend ben. Daar heb je een functie die doet wat hij belooft, en als er variates zijn dan is het eind resultaat ook daadwerkelijk anders.

Haskell is blijkbaar erg afhankelijk van deze Higher-Order functions.

# Partially Applied Functions and Currying

> Every function in Haskell has one parameter.

Gelijk een mooi statement om het hoofdstuk mee te openen. In het hoofdstuk Anonymous Functions heb ik volgens mij duidelijk laten zien dat een functie n parameters kon hebben... Blijkbaar bevat Haskell een process genaamd Currying, dit process maakt van een functie met meerdere parameters functies met maar 1 parameter. 

`let sum x y = x + y` bevat twee parameters, maar aangezien Haskell alleen maar met functies met 1 parameter werkt zal hij deze functie ombouwen. Bij de aanroep `sum 1 2` wordt dit eerst een anonieme functie namelijk: `(\y -> 1 + y)`, deze wordt vervolgens met als parameter 2 uitgevoerd. 

Het maximum van 1 parameter is een limitatie waar ik nooit aan gedacht had, maar het is achteraf gezien ook niet zo'n limiteerende factor, zeker gezien Haskell dit alleen "onder water" heeft. 

# Lazy Evaluation
Een beetje het zelfde principe als bijvoorbeeld lazy loading van plaatjes op een website. "Ik ga alleen iets uitwerken als daar specifiek om gevraagd word"
In Haskell uit Lazy Evaluation zich in bijvoorbeeld infinite lists. De range syntax die we al eerder gezien hebben: `[1..12]` kon ook geschreven worden als `[1,2..12]` sterker nog de `12` kon weggelaten worden: `[1,2..]` een range die oneindig door gaat. Doormiddel van de functie `take` konden we de lengte die we wilden hebben bepalen.
```Haskell
Prelude> take 5 [1,2..]
[1,2,3,4,5]
it :: (Num a, Enum a) => [a]
```
Het zelf maken van een infinite list is eigenlijk een recusieve functie zonder stopconditie... Klinkt als een slecht idee, maar werkt in combinatie met de functies [take en drop](http://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#v:take) wel.


In deze blogpost zijn een aantal interessante concepten voorbij gekomen. Ik heb het gevoel dat ik de basis van Haskell redelijk door heb... Of dat echt zo is zullen we in de volgende blogpost bij de uitwerkingen van de opgaven zien.
