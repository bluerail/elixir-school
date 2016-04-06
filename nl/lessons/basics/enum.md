---
layout: page
title: Enum
category: basics
order: 3
lang: nl
---

Een set van algoritmes voor het opsommen (enumerate) van verzamelingen.

## Inhoud

- [Enum](#enum)
  - [all?](#all)
  - [any?](#any)
  - [chunk](#chunk)
  - [chunk_by](#chunk_by)
  - [each](#each)
  - [map](#map)
  - [min](#min)
  - [max](#max)
  - [reduce](#reduce)
  - [sort](#sort)
  - [uniq](#uniq)

## Enum

De `Enum` module bevat meer dan honderd functies voor het werken met de verzamelingen die we in de vorige les bestudeerd hebben.

Deze les behandelt slechts een deel van de beschikbare functies. Bezoek de officiële documentatie [`Enum`](http://elixir-lang.org/docs/v1.0/elixir/Enum.html) voor een complete lijst van functies; voor luie opsomming kun je de [`Stream`](http://elixir-lang.org/docs/v1.0/elixir/Stream.html) module gebruiken.

### all?

Wanneer we `all?` (en veel andere functies van `Enum`) gebruiken, geven we een functie die we toepassen op de items van onze verzameling. In het geval van `all?` met de gehele verzameling beoordeeld worden als `true`, anders zal `false` worden teruggegeven:

```elixir
iex> Enum.all?(["foo", "bar", "hallo"], fn(s) -> String.length(s) == 3 end)
false
iex> Enum.all?(["foo", "bar", "hallo"], fn(s) -> String.length(s) > 1 end)
true
```

### any?

In tegenstelling tot het bovenstaande geeft `any?` `true` terug als ten minste één item beoordeeld wordt als `true`:

```elixir
iex> Enum.any?(["foo", "bar", "hallo"], fn(s) -> String.length(s) == 5 end)
true
```

### chunk

Als je jouw verzameling wil opbreken in kleinere groepjes, dan wil je waarschijnlijk `chunk` gebruiken:

```elixir
iex> Enum.chunk([1, 2, 3, 4, 5, 6], 2)
[[1, 2], [3, 4], [5, 6]]
```

Er zijn een aantal opties voor `chunk`, maar daar gaan we hier niet verder op in. Check [`chunk/2`](http://elixir-lang.org/docs/v1.0/elixir/Enum.html#chunk/2) in de officiële documentatie om meer te leren.

### chunk_by

Mochten we onze verzameling willen groeperen op basis van iets anders dan de grootte, dan kunnen we de `chunk_by` methode gebruiken:

```elixir
iex> Enum.chunk_by(["one", "two", "three", "four", "five"], fn(x) -> String.length(x) end)
[["one", "two"], ["three"], ["four", "five"]]

iex> Enum.chunk_by(["een", "twee", "drie", "vier", "vijf"], fn(x) -> String.length(x) end)
[["een"], ["twee", "drie", "vier", "vijf"]]
```

### each

Soms is het nodig om te itereren over een verzameling zonder nieuwe waardes te produceren. In dit geval gebruiken we `each`:


```elixir
iex> Enum.each(["een", "twee", "drie"], fn(s) -> IO.puts(s) end)
een
twee
drie
```

__Noot__: De `each` methode geeft de atom `:ok` terug.

### map

Wanneer we onze functie willen toepassen op elk item en daarmee een nieuwe verzameling willen produceren, gebruik dan de `map` functie:

```elixir
iex> Enum.map([0, 1, 2, 3], fn(x) -> x - 1 end)
[-1, 0, 1, 2]
```

### min

Vind de `min` waarde in onze verzameling:

```elixir
iex> Enum.min([5, 3, 0, -1])
-1
```

### max

Geeft de `max` waarde terug in de verzameling:

```elixir
iex> Enum.max([5, 3, 0, -1])
5
```

### reduce

Met `reduce` kunnen we onze verzameling destilleren naar een enkele waarde. We leveren dan een optionele accumulator mee (`10` in dit voorbeeld) welke wordt meegegeven in onze functie. De eerste waarde wordt gebruikt indien geen accumulator meegegeven wordt:

```elixir
iex> Enum.reduce([1, 2, 3], 10, fn(x, acc) -> x + acc end)
16
iex> Enum.reduce([1, 2, 3], fn(x, acc) -> x + acc end)
6
```

### sort

Het sorteren van onze verzamelingen is vergemakkelijkt met maar liefst twee `sort` functies. De eerste mogelijkheid gebruikt Elixir's sortering op term om de gesorteerde order te bepalen:

```elixir
iex> Enum.sort([5, 6, 1, 3, -1, 4])
[-1, 1, 3, 4, 5, 6]

iex> Enum.sort([:foo, "bar", Enum, -1, 4])
[-1, 4, Enum, :foo, "bar"]
```

De andere mogelijkheid staat ons toe om een sorteerfunctie mee te geven:


```elixir
# met onze functie
iex> Enum.sort([%{:val => 4}, %{:val => 1}], fn(x, y) -> x[:val] > y[:val] end)
[%{val: 4}, %{val: 1}]

# zonder
iex> Enum.sort([%{:aantal => 4}, %{:aantal => 1}])
[%{aantal: 1}, %{aantal: 4}]
```

### uniq

We kunnen `uniq` gebruiken om dubbele items uit onze verzamelingen te verwijderen:

```elixir
iex> Enum.uniq([1, 2, 2, 3, 3, 3, 4, 4, 4, 4])
[1, 2, 3, 4]
```
