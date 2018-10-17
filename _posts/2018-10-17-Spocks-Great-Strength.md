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
