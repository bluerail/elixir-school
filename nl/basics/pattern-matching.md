---
layout: page
title: Pattern Matching
category: basics
order: 4
lang: nl
---

Pattern matching (patroonherkenning) is een krachtig onderdeel van Elixir. Het stelt ons in staat om overeenkomende waardes, datastructuren en zelfs functies te vinden. In deze les zullen we zien hoe pattern matching wordt gebruikt.

## Inhoud

- [Match operator](#match-operator)
- [Pin operator](#pin-operator)

## Match operator

De `=` operator is onze match operator. Wat een verrassing! Via de match operator kunnen we waardes toewijzen en vervolgens matchen. Laten we eens een kijkje nemen:

```elixir
iex> x = 1
1
```

Laten we eens een eenvoudige match proberen:

```elixir
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

Laten we dat eens proberen met een paar van onze verzamelingen:

```elixir
# Lijsten
iex> lijst = [1, 2, 3]
iex> [1, 2, 3] = lijst
[1, 2, 3]
iex> [] = lijst
** (MatchError) no match of right hand side value: [1, 2, 3]

iex> [1|tail] = lijst
[1, 2, 3]
iex> tail
[2, 3]
iex> [2|_] = lijst
** (MatchError) no match of right hand side value: [1, 2, 3]

# Tuples
iex> {:ok, value} = {:ok, "Succesvol!"}
{:ok, "Succesvol!"}
iex> value
"Succesvol!"
iex> {:ok, value} = {:fout}
** (MatchError) no match of right hand side value: {:fout}
```

## Pin operator

We hebben net geleerd dat de match operator zorgt voor toewijzing wanneer de linkerzijde van de vergelijking een variabele bevat. In sommige gevallen is dit gedrag (het opnieuw toewijzen van een variabele) onwenselijk. Voor deze situaties hebben we de pin operator: `^`.

Wanneer we een variabele (vast) pinnen, matchen we op de bestaande waarde in plaats van een nieuwe waarde vast te leggen. Laten we eens kijken hoe dit in zijn werk gaat:

```elixir
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {x, ^x} = {2, 1}
{2, 1}
iex> x
2
```

Elixir 1.2 introduceerde ondersteuning voor pins in map, keys en functie clausules:

```elixir
iex> key = "hallo"
"hallo"
iex> %{^key => value} = %{"hallo" => "wereld"}
%{"hallo" => "wereld"}
iex> value
"wereld"
iex> %{^key => value} = %{:hallo => "wereld"}
** (MatchError) no match of right hand side value: %{hallo: "wereld"}
```

Een voorbeeld van pinning in een functie clausule:

```elixir
iex> greeting = "Hello"
"Hello"
iex> greet = fn
...>   (^greeting, name) -> "Hi #{name}"
...>   (greeting, name) -> "#{greeting}, #{name}"
...> end
#Function<12.54118792/2 in :erl_eval.expr/5>
iex> greet.("Hello", "Sean")
"Hi Sean"
iex> greet.("Mornin'", "Sean")
"Mornin', Sean"
```
