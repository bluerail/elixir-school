---
layout: page
title: Pipe Operator 
category: basics
order: 6 
lang: nl
---

De pipe operator `|>` geeft het resultaat van een uitdrukking mee als de eerste parameter van een andere uitdrukking.

## Inhoud

- [Introductie](#introduction)
- [Voorbeelden](#examples)
- [Best Practices](#best-practices)

## Introductie

Bij programmeren wordt het nogal eens snel een rommeltje. Zo rommelig dat functie-aanroepen (function calls) zo ingebed worden dat ze bijna niet meer te volgen zijn. Bekijk de volgende genestelde functies eens:


```elixir
foo(bar(baz(nieuwe_functie(andere_functie()))))
```

We geven hier de waarde `andere_functie/1` aan `nieuwe_functie/1` en `nieuwe_functie/1` aan `baz/1`, `baz/1` aan `bar/1` en uiteindelijk het resultaat van `bar/1` aan `foo/1`. Elixir haakt op een handige manier in op deze syntactische chaos door ons de pipe operator te geven. De pipe operator, die eruitziet als `|>` *neemt het resultaat van een uitdrukking en geeft dat door*. Laten we nog eens kijken naar het stukje code van hierboven, maar dan herschreven met de pipe operator.


```elixir
andere_functie() |> nieuwe_functie() |> baz() |> bar() |> foo()
```

De pipe neemt het resultaat van de linkerkant en geeft het door aan de rechterkant.

## Voorbeelden

Voor deze reeks voorbeelden gebruiken we Elixir's String module.

- String tokeniseren (losjes)

```shell
iex> "Elixir is geweldig" |> String.split
["Elixir", "is", "geweldig"]
```

- Alle tokens in hoofdletters

```shell
iex> "Elixir is geweldig" |> String.split |> Enum.map( &String.upcase/1 )
["ELIXIR", "IS", "GEWELDIG"]
```

- Het laatste stuk controleren

```shell
iex> "elixir" |> String.ends_with?("ixir")
true
```

## Best Practices

Wanneer het aantal argumenten van een functie meer dan 1 is, zorg dan dat je haakjes gebruikt. In Elixir maakt dit niet zoveel uit, maar het kan een verschil maken voor andere programmeurs die anders wellicht jouw code verkeerd interpreteren. Als we naar ons tweede voorbeeld kijken en de haakjes verwijderen rondom `Enum.map/2`, dan krijgen we de volgende waarschuwing.

```shell
iex> "Elixir is geweldig" |> String.split |> Enum.map &String.upcase/1
iex: warning: you are piping into a function call without parentheses, which may be ambiguous. Please wrap the function you are piping into in parenthesis. For example:

foo 1 |> bar 2 |> baz 3

Should be written as:

foo(1) |> bar(2) |> baz(3)

["ELIXIR", "IS", "GEWELDIG"]
```

Wat vertaalt naar:

*Waarschuwing: je probeert te 'pipen' naar een functie-aanroep zonder haakjes. Dit kan dubbelzinnige betekenissen hebben. Zet alsjeblieft haakjes rondom de functie waar je naartoe wil pipen. Bijvoorbeeld:*

```shell
foo 1 |> bar 2 |> baz 3
```

*Zou geschreven moeten worden als:*

```
foo(1) |> bar(2) |> baz(3)
```
