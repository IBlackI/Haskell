---
layout: post
title: Primitieve types
---

In de vorige blogpost heb ik mij via [de website van Haskell](https://www.haskell.org/) in de taal verdiept middels de tutorial. 
Echter moeten wij voor ASD-APP gebruik maken van het boek [Seven Languages in Seven Weeks](https://pragprog.com/book/btlang/seven-languages-in-seven-weeks) geschreven door Bruce Tate.
  
In de tutorial van Haskell heb ik al geleerd over Strings, Lijsten, Functions en Pattern Matching. In deze blogpost begin ik aan Day 1: Logical uit het boek.

# GHCi Installeren  
Voordat ik verder kan met het boek zal ik eerst een Haskell compiler op mijn systeem moeten hebben, de tutorial gebruikte een interactieve in-browser commandline interface, maar het boek gebruikt GHCi.  
GHCi is de (interactieve) compiler van Haskell. De installatie hiervan was simpelweg het [downloaden](https://www.haskell.org/platform/windows.html) van de website en uitvoeren van de installer. De applicatie WinGHCi is de windows variant van de GHCi console die in het boek gebruikt word.

# Herhaling
De eerste 3 hoofdstukken gaan over de primitieve waardes in Haskell dit zijn: 
1. Numbers
1. Character Data
1. Booleans  
Aangezien ik de tutorial al gehad heb zal ik hierbij alleen de dingen behandelen die ik nog niet wist.  
## Numbers
  Dit hoofdstuk bevat geen nieuwe stof, echter valt het mij wel op dat hierbij in de resultaten geen datatypes worden weergegeven in tegenstelling tot de online versie.  
## Character Data
Zoals in de tutorial ook al geleerd is een String een lijst van characters. Twee lijsten zijn ook niet samen te voegen doormiddel van `+`, bijvoorbeeld `"hello" + "world"`. Hier is een speciale operator (of misschien is het een functie) voor namelijk `++`.
## Booleans
Booleans zijn de enige primitieve datatypes die niet in de tutorial behandelt zijn. Hierbij valt gelijk de "niet gelijk aan" operator op. Deze is in veel andere talen `!=`, maar in LUA ben ik gewend dat het `~=` is. Haskell heeft zijn eigen "niet gelijk aan" operator `/=` het eerste wat mij tijdens het typen van deze operator opvalt is dat, in tegenstelling tot `!=` of `~=`, alle benodigde karakters aan de rechterkant van het toetsenbord zitten. De tutorial had het erover dat `fst` zo kort was omdat het vaak nodig is, maar het typen van `/=` gebeurt allebei met mijn rechter pink. Dit heeft als gevolg dat het lastiger en langzamer is om te typen. Een beetje tegenstrijdig, tenzij `/=` nauwlijks gebruikt word natuurlijk.

> In Haskell, if is a function, not a control structure, meaning it returns a value just like any other function.  
  
`if (5 == 5) then "true" else "false"` is dus een voorbeeld van een if functie aanroep, ik neem aan dat `then` en `else` hierbij ook parameters zijn. 

`:set +t` is blijkbaar de optie waarmee de datatypes van de output weer aangezet kunnen worden. 

> Rather than a pure type , you get a class , which is a description of a bunch of similar types. We’ll learn more in Section 8.4, **Classes**, on page 299.  

Bij het woord Classes denk ik gelijk aan de Object georienteerde manier, ik ben benieuwd wat dit in functioneel programmeren inhoud.  

# Functions
In de tutorial is ook kort het maken van een functie voorbij gekomen. Het boek gaat hier dieper op in, maar eerst wisselen we de interactieve GHCi in voor "echte" bestanden. In het boek staan links om de code die getoond wordt te downloaden en zelf te kunnen draaien. In Haskell wordt met modules gewerkt, in een module kunnen meerde regels code staan in tegenstelling tot GHCi waarbij je maar een regel code kan ingeven.  
```
Prelude> :load double.hs
[1 of 1] Compiling Main ( double.hs, interpreted )
Ok, modules loaded: Main.
* Main> double 5
10
```  
  
Helaas werkt bovenstaand voorbeeld niet in mijn Haskell omgeving, na even Googlen blijkt dit er aan te liggen dat de module [niet Main mag heten zonder meerdere regels aan IO code](https://www.reddit.com/r/haskellquestions/comments/3of62e/the_io_action_main_is_not_defined_in_module_main/cvwouf8/). Ik weet niet of dit door een update in Haskell of een fout in het boek komt. Na het aanpassen van de naam van de module werkt het wel. 

```HASKELL
double :: Integer -> Integer
double x = x + x
```

In Java bestaat een functie uit een header en een body bijvoorbeeld
```JAVA
//Header
public function String sayHello(String name) {
  //Body
  return "Hello, " + name;
}
```

Bij Haskell lijkt de Header uit 1.5 regels te bestaan, eerst de naam gevolgd door de types. Dan op de volgende regel weer de naam gevolgd door de body. Het twee keer definieren van de naam lijkt mij dubbel op, maar wellicht heeft dit als voordeel dat je bovenin een bestand alle functies met types kan definieren en pas later in het bestand declareren waardoor je snel zou kunnen zien wat een module kan.

## Recursie
Recusie is een onderwerp waar ik al vaker ervaring mee gehad heb, ik verwacht dat de implementatie hiervan (een functie die zichzelf aanroept) in Haskell niet anders zal zijn dan in andere talen. 
```
Prelude> let fact x = if x == 0 then 1 else fact (x - 1) * x
```
Dit is (behalve dan in 1 regel) zoals ik zou verwachten bij een recursieve functie.

```HASKELL
module Main where
  factorial :: Integer -> Integer
  factorial 0 = 1
  factorial x = x * factorial (x - 1)
```
De functie hierboven is wel anders dan ik zou verwachten, maar met de omschrijving erbij is het best logisch. Als de input `0` is dan is de output `1`. Als de input `x` is, is de output `x * factorial (x - 1)`.

### Guards
```HASKELL
module Main where
factorial :: Integer -> Integer
factorial x 
  | x > 1 = x*  factorial (x - 1)
  | otherwise = 1
```
Guards zijn een principe waarvan ik nog nooit eerder gehoord had, maar de theorie vind ik wel interessant. Een guard is een syntax waarmee je de input eerst kan vergelijken via een expressie en als er true uit komt voert hij de bijbehorende body uit. Ik zie niet wat het voorbeeld is ten op zichte van een if statement, wellicht omdat Haskell geen else if kent?

# Fin
In de volgende blogpost zal ik verder gaan met Day 1: Logical. In deze post is het voornamelijk herhaling geweest van de tutorial, maar er zijn ook zeker interessante nieuwe concepten voorbij gekomen. Ik ben wel van mening dat de syntax nog steeds niet altijd gelijk duidelijk is. 
