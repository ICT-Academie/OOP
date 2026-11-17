# Opdracht 0 — Beschrijf een RPG-item

[← Overzicht](../README.md) · [Volgende →](01-weapon.md)

## Situatie

In een RPG heeft ieder item eigenschappen. Een Iron Sword en een Fire Sword zijn allebei wapens, maar hebben andere waarden.

## Leerdoelen

- Je onderscheidt een klasse van een object.
- Je herkent properties en kiest passende datatypes.
- Je maakt twee objecten van dezelfde klasse.

## Benodigdheden

.NET 10 en een Console App.

## Korte theorie

Een **klasse** is een beschrijving; een **object** is één exemplaar. Een **property** is een kenmerk. `string` bewaart tekst, `int` gehele getallen, `double` kommagetallen en `bool` `true` of `false`.

```csharp
public class Potion
{
    public string Name { get; set; } = "";
    public int Healing { get; set; }
}
```

## Opdrachten

### Stap 1 — Kies je item

Kies een zwaard, bijl, boog of staf. Noteer minimaal vijf eigenschappen met een voorbeeldwaarde en datatype. Gebruik in ieder geval een `string`, `int`, `double` en `bool`.

### Stap 2 — Maak een klasse

Maak `Weapon.cs` met de vijf properties. Gebruik voor deze introductie eenvoudige auto-properties.

### Stap 3 — Maak objecten

Maak in `Program.cs` twee verschillende wapens met `new Weapon()`. Geef ieder object eigen waarden.

### Stap 4 — Toon gegevens

Toon van beide objecten minimaal drie eigenschappen met `Console.WriteLine()`.

## Testscenario's

- Maak een Iron Sword met Damage 15 en een Fire Sword met Damage 35.
- Controleer dat beide objecten verschillende waarden tonen.
- Wijzig de naam van één object. Controleer dat het andere object niet verandert.

## Denkvragen

1. Wat is het verschil tussen de klasse `Weapon` en een Iron Sword-object?
2. Waarom is `Damage` een `int` en `Name` een `string`?
3. Wat gebeurt er als je twee objecten van dezelfde klasse maakt?

## Eindcontrole

- [ ] Minimaal vijf eigenschappen beschreven.
- [ ] Klasse gemaakt met passende datatypes.
- [ ] Twee objecten aangemaakt en getoond.

**Let op:** in opdracht 1 vervang je deze oefenversie door een uitgebreidere `Weapon` met private fields.
