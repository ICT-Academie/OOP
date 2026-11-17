# Opdracht 3 — Equipment en refactoring

[← Overzicht](../README.md) · [← Vorige](02-inventory.md) · [Volgende →](04-sword.md)

## Situatie

Wapens en andere uitrusting hebben eigenschappen gemeen. Je maakt een abstracte basisklasse `Equipment` en verplaatst gedeelde eigenschappen uit `Weapon` naar deze klasse. Bestaande tests blijven werken.

## Leerdoelen

Abstracte klasse, `protected` fields, `static readonly`, method-overloading, abstracte method, `DateTime`, berekende property en refactoring toepassen.

## Korte theorie

Een abstracte klasse kan niet rechtstreeks met `new` worden aangemaakt. `protected` fields zijn bereikbaar voor afgeleide klassen. Een `static readonly` field heeft één gedeelde waarde die na initialisatie niet opnieuw kan worden toegewezen. **Overloading** betekent: dezelfde methodnaam met andere parameters.

```csharp
public abstract class Item
{
    protected string name = "";
    public abstract string Describe();
}
```

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
