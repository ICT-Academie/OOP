# Opdracht 3 — Equipment en refactoring

[← Overzicht](../README.md) · [← Vorige](02-inventory.md) · [Volgende →](04-sword.md)

## Situatie

Wapens en andere uitrusting hebben eigenschappen gemeen. Je maakt een abstracte basisklasse `Equipment` en verplaatst gedeelde eigenschappen uit `Weapon` naar deze klasse. Bestaande tests blijven werken.

## Leerdoelen

Abstracte klasse, `protected` fields, `static readonly`, method-overloading, abstracte method, `DateTime`, berekende property en refactoring toepassen.

## Theorie

### 1. Waarom een abstracte basisklasse?

Als verschillende klassen dezelfde gegevens en methods nodig hebben, kun je die in een **basisklasse** zetten. Een afgeleide klasse erft de toegankelijke onderdelen daarvan. Een **abstracte klasse** dient als gemeenschappelijke basis en kan niet rechtstreeks met `new` worden gemaakt.

```csharp
public abstract class QuestReward
{
    protected string name = "";
    public abstract string Describe();
}
```

Een abstracte method zoals `Describe()` heeft hier geen implementatie. Een concrete afgeleide klasse moet deze method implementeren.

### 2. `private`, `protected` en `public`

- `private`: rechtstreeks toegankelijk binnen de klasse waarin het field staat.
- `protected`: rechtstreeks toegankelijk binnen de klasse en haar afgeleide klassen.
- `public`: toegankelijk voor andere code die het object gebruikt.

Gebruik `protected` alleen wanneer afgeleide klassen echt toegang nodig hebben. Voor andere code zijn publieke properties vaak duidelijker.

### 3. `static readonly`

`static` betekent dat een field bij de **klasse** hoort in plaats van bij ieder afzonderlijk object. `readonly` betekent dat je het field alleen bij declaratie of in een geschikte constructor kunt toewijzen. Samen kun je hiermee een gedeelde, niet opnieuw toe te wijzen waarde vastleggen.

```csharp
public class ShopRules
{
    public static readonly double TaxPercentage = 10.0;
}
```

### 4. Method-overloading

Bij **overloading** hebben methods dezelfde naam, maar verschillende parameters:

```csharp
public string ShowQuest() { return "Unknown quest"; }
public string ShowQuest(string name) { return "Quest: " + name; }
```

C# kiest de passende method op basis van de meegegeven argumenten. Overloading is iets anders dan **overriding**: daarbij vervangt een afgeleide klasse de implementatie van een geërfde method.

### 5. `DateTime`, `decimal` en berekende properties

`DateTime` beschrijft een datum en tijd; `DateTime.Now` geeft de huidige lokale datum en tijd. `default(DateTime)` is de standaardwaarde van een nog niet ingestelde `DateTime`. Met het verschil tussen twee datums kun je een `TimeSpan` berekenen.

Voor geldbedragen gebruik je doorgaans `decimal` in plaats van `double`, bijvoorbeeld `100m`. De `m` geeft aan dat de waarde een `decimal` is. Een read-only property kan een berekening maken uit een andere property en een percentage.

### 6. Refactoring zonder gedrag te verliezen

**Refactoring** betekent dat je de structuur van bestaande code verbetert zonder het bedoelde gedrag te veranderen. Wanneer je gedeelde fields naar een basisklasse verplaatst, moeten de bestaande constructors, methods, properties en tests blijven werken. Test daarom ook eerdere opdrachten opnieuw.

## Opdrachten

### Stap 1 — Maak de abstracte klasse

Maak `Equipment/Equipment.cs` met namespace `RpgEquipment.Equipment`. Definieer `public abstract class Equipment`.

### Stap 2 — Voeg protected fields toe

| Field | Type | Functie |
|---|---|---|
| `id` | `int` | Uniek itemnummer |
| `name` | `string` | Itemnaam |
| `description` | `string` | Beschrijving |
| `weight` | `double` | Gewicht |
| `value` | `decimal` | Basiswaarde in goud |
| `createdAt` | `DateTime` | Moment waarop het item is gemaakt |
| `marketFeePercentage` | `double` | Gedeeld percentage voor verkoopkosten |

Definieer het laatste field als `protected static readonly double marketFeePercentage = 10.0;`. Een markt rekent 10% verkoopkosten; dat is een logische toepassing van een percentageberekening. Je hoeft geen expliciete constructor voor `Equipment` te schrijven.

### Stap 3 — Maak methods

| Method | Returntype | Gedrag |
|---|---|---|
| `DisplayIdentity()` | `string` | `Item: [id]` |
| `DisplayIdentity(bool nameInfo, bool weightInfo)` | `string` | Voegt optioneel naam en gewicht toe |
| `GetItemAge()` | `string` | `Age [dagen] days` of `Age unknown` |
| `DescribeAbility()` | `string` | Abstracte method, geen implementatie |

Bij de overload voeg je ` Name: [name]` en/of ` Weight: [weight]` toe. Gebruik `DateTime.Now` en `default(DateTime)` om te bepalen of `createdAt` is ingevuld. Declareer `public abstract string DescribeAbility();`.

### Stap 4 — Voeg properties toe

| Property | Type | get | set |
|---|---|:---:|:---:|
| `Id` | `int` | ✓ | ✓ |
| `Name` | `string` | ✓ | ✓ |
| `Description` | `string` | ✓ | ✓ |
| `Weight` | `double` | ✓ | ✓ |
| `Value` | `decimal` | ✓ | ✓ |
| `NetSaleValue` | `decimal` | ✓ | — |
| `CreatedAt` | `DateTime` | ✓ | ✓ |

`NetSaleValue` is `Value` na aftrek van 10% marktvergoeding. Gebruik `decimal` voor de berekening; denk aan de conversie vanuit `double`.

### Stap 5 — Refactor `Weapon`

Laat `Weapon` erven van `Equipment`. Verwijder de dubbele fields en properties `id`, `name`, `description` en `weight` uit `Weapon`: deze komen voortaan uit de basisklasse. Laat de bestaande vier constructors, acht oorspronkelijke propertynamen en methods van opdracht 1 bruikbaar blijven; geërfde properties tellen mee.

`Weapon` moet nu `DescribeAbility()` implementeren, bijvoorbeeld met de tekst `Weapon has no special ability`. Zorg dat de tests van opdrachten 1 en 2 blijven werken. Voeg `Value` en `CreatedAt` toe aan je testscenario.

### Stap 6 — Test

- `new Equipment()` mag niet compileerbaar zijn.
- `Weapon` moet wel compileerbaar zijn.
- `Value = 100m` geeft `NetSaleValue = 90m`.
- `GetItemAge()` geeft `Age unknown` wanneer geen datum is ingevuld.
- `DisplayIdentity(true, true)` bevat zowel naam als gewicht.
- Alle bestaande tests van `Weapon` en `Inventory` slagen nog.

## Denkvragen

1. Waarom verplaats je gedeelde fields naar `Equipment`?
2. Wat is het verschil tussen `private` en `protected`?
3. Waarom is `NetSaleValue` read-only?
4. Waarom is `DescribeAbility()` abstract?

## Eindcontrole

- [ ] Abstracte klasse met zeven protected fields, waaronder `static readonly`.
- [ ] Twee overloaded identity-methods, leeftijdsmethod en abstracte method.
- [ ] Berekende verkoopwaarde werkt.
- [ ] Refactoring heeft eerdere opdrachten niet kapotgemaakt.
