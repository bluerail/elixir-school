---
layout: page
title: Controlestructuren
category: basics
order: 5
lang: nl
---

In deze les kijken we naar de controlestructuren die voor ons beschikbaar zijn binnen Elixir.

## Inhoud

- [`if` en `unless`](#if-and-unless)
- [`case`](#case)
- [`cond`](#cond)
- [`with`](#with)

## `if` en `unless`

Het zou kunnen dat je `if/2` eerder bent tegengekomen. Daarnaast zul je bekend zijn met `unless/2` als je met Ruby gewerkt hebt. In Elixir werken ze grotendeels op dezelfde manier maar zijn ze gedefinieerd als macro's in plaats van taalconstructies. Je kan hun implementatie terug vinden in de [Kernel module](http://elixir-lang.org/docs/stable/elixir/#!Kernel.html).

Opgemerkt dient te worden dat in Elixir de enige 'falsey' (onware) waardes `nil` en de boolean `false` zijn.

```elixir
iex> if String.valid?("Hallo") do
...>   "Geldige string!"
...> else
...>   "Ongeldige string."
...> end
"Geldige string!"

iex> if "een string waarde" do
...>   "Waar"
...> end
"Waar"
```

`unless/2` is te gebruiken als `if/2`, alleen werkt het op tegenovergestelde manier:

```elixir
iex> unless is_integer("hallo") do
...>   "Geen Int"
...> end
"Geen Int"
```

## `case`

We kunnen `case` gebruiken wanneer het nodig is om meerdere patterns te matchen:

```elixir
iex> case {:ok, "Hallo Wereld"} do
...>   {:ok, resultaat} -> resultaat
...>   {:error} -> "Uh oh!"
...>   _ -> "Catch all"
...> end
"Hallo Wereld"
```

De `_` variabele vormt een belangrijke toevoeging in `case` opdrachten. Zonder de `_` krijgen we een fout wanneer er geen passende match gevonden wordt.

```elixir
iex> case :even do
...>   :oneven -> "Oneven"
...> end
** (CaseClauseError) no case clause matching: :even

iex> case :even do
...>   :oneven -> "Oneven"
...>   _ -> "Niet Oneven"
...> end
"Niet Oneven"
```

Beschouw `_` als de `else` die "al het overige" matcht.
Omdat `case` zich beroept op pattern matching zijn dezelfde regels en beperkingen van toepassing. Als je van plan bent om te matchen met bestaande variabelen, dien je dus de pin `^` operator te gebruiken:

```elixir
iex> pie = 3.41
3.41
iex> case "kersentaart" do
...>   ^pie -> "Niet zo smakelijk"
...>   pie -> "Ik durf te wedden dat #{pie} smakelijk is!"
...> end
"Ik durf te wedden dat kersentaart smakelijk is!"
```

Een andere gave eigenschap van `case` is zijn ondersteuning voor guard clausules:

_Dit voorbeeld komt direct van de officiële Elixir [Getting Started](http://elixir-lang.org/getting-started/case-cond-and-if.html#case) handleiding._

```elixir
iex> case {1, 2, 3} do
...>   {1, x, 3} when x > 0 ->
...>     "Will match"
...>   _ ->
...>     "Won't match"
...> end
"Will match"
```

Check de officiële documentatie betreffende geoorloofde [Uitdrukkingen in guard clausules](http://elixir-lang.org/getting-started/case-cond-and-if.html#expressions-in-guard-clauses).

## `cond`

Indien het nodig is om voorwaardes (condities) te matchen, in plaats van waardes, kunnen we gebruik maken van `cond`;  dit is vergelijkbaar met `else if` of `elsif` in andere talen:


_Dit voorbeeld komt direct van de officiële Elixir [Getting Started](http://elixir-lang.org/getting-started/case-cond-and-if.html#case) handleiding._

```elixir
iex> cond do
...>   2 + 2 == 5 ->
...>     "This will not be true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this will"
...> end
"But this will"
```

Net als `case` zal `cond` een fout geven als er geen match gevonden wordt. Om dit te voorkomen, kunnen we een voorwaarde definiëren als `true`:

```elixir
iex> cond do
...>   7 + 1 == 0 -> "Incorrect"
...>   true -> "Catch all"
...> end
"Catch all"
```

## `with`

De speciale vorm `with` is handig wanneer je een genestelde `case` opdracht zou willen gebruiken of bij situaties die niet netjes bij elkaar 'gepiped' kunnen worden. De `with` uitdrukking bestaat uit het keyword, generatoren en uiteindelijk een uitdrukking.

Generatoren zullen we uitgebreider behandelen in de Lijstcomprehensies les maar voor nu hoeven we alleen te weten dat ze pattern matching gebruiken om de rechterzijde van de `<-` te vergelijken met de linkerzijde.

We beginnen met een simpel voorbeeld van `with` en maken het dan iets complexer:


```elixir
iex> gebruiker = %{voornaam: "Sean", achternaam: "Callan"}
iex> with {:ok, voornaam} <- Map.fetch(gebruiker, :voornaam),
...>      {:ok, achternaam} <- Map.fetch(gebruiker, :achternaam),
...>      do: achternaam <> ", " <> voornaam
"Callan, Sean"
```

In het geval dat een uitdrukking er niet in slaagt om te matchen, zal de niet-matchende waarde worden teruggegeven:

```elixir
iex> gebruiker = %{voornaam: "doomspork"}
%{voornaam: "doomspork"}
iex> with {:ok, voornaam} <- Map.fetch(gebruiker, :voornaam),
...>      {:ok, achternaam} <- Map.fetch(gebruiker, :achternaam),
...>      do: achternaam <> ", " <> voornaam
:error
```

Laten we nu eens kijken naar een groter voorbeeld zonder `with` en zien hoe we dit korter kunnen schrijven:


```elixir
case Repo.insert(changeset) do 
  {:ok, gebruiker} -> 
    case Guardian.encode_and_sign(resource, :token, claims) do
      {:ok, jwt, full_claims} ->
        important_stuff(jwt, full_claims)
      error -> error
    end
  error -> error
end
```

Als we nu `with` introduceren, ontstaat er code die makkelijker te begrijpen is en minder regels bevat:


```elixir
with 
  {:ok, gebruiker} <- Repo.insert(changeset),
  {:ok, jwt, full_claims} <- Guardian.encode_and_sign(gebruiker, :token),
  do: important_stuff(jwt, full_claims)
```
