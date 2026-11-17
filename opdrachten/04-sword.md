# Opdracht 4 — Sword

[← Overzicht](../README.md) · [← Vorige](03-equipment.md) · [Volgende →](05-magicstaff.md)

## Situatie

Je maakt een gespecialiseerd wapen: een `Sword`. Een zwaard erft de algemene eigenschappen van `Weapon` en krijgt een eigen zwaardtype en abilitybeschrijving.

## Leerdoelen

Overerving, constructor met `base`, enum, property en overriding toepassen.

## Korte theorie

Een afgeleide klasse kan een method van een basisklasse overschrijven als deze `virtual` of `abstract` is.

```csharp
public enum PotionSize { Unknown, Small, Medium, Large }
```

## Opdrachten

### Stap 1 — Maak de klasse

Maak `Weapons/Sword.cs` in namespace `RpgEquipment.Weapons`. Laat `Sword` erven van `Weapon`.

### Stap 2 — Maak een enum en field

Maak publieke enum `SwordType` met **vier** waarden: `Unknown`, `ShortSword`, `LongSword`, `GreatSword`.

Voeg een private field `swordType` van type `SwordType` toe.

### Stap 3 — Constructor

Maak `public Sword(int id)`. Gebruik `base(id)` om de ID door te geven aan de basisklasse.

### Stap 4 — Override

Overschrijf `DescribeAbility()` met een `switch` op `swordType`.

| Type | Exacte returnwaarde |
|---|---|
| `ShortSword` | `Ability: quick slash.` |
| `LongSword` | `Ability: balanced strike.` |
| `GreatSword` | `Ability: heavy cleave.` |
| `Unknown` of anders | `Ability unknown` |

### Stap 5 — Property

Maak een publieke read/write property `SwordType` van type `SwordType` die het private field gebruikt.

### Stap 6 — Test

Maak `Sword sword = new Sword(100);`, stel `Name` in op `Knight Sword`, `SwordType` op `SwordType.GreatSword` en controleer `DescribeAbility()`.

Test alle vier enumwaarden. Controleer dat geërfde properties `Id`, `Name`, `Weight` en `Value` werken.

## Denkvragen

1. Waarom hoeft `Sword` geen eigen `Id`-field te hebben?
2. Wat doet `base(id)`?
3. Waarom gebruik je `override`?

## Eindcontrole

- [ ] Vier enumwaarden en één private field.
- [ ] Constructor geeft ID door.
- [ ] Override geeft de juiste tekst voor alle vier gevallen.
- [ ] Geërfde properties werken.
