# Opdracht 2 — Inventory

[← Overzicht](../README.md) · [← Vorige](01-weapon.md) · [Volgende →](03-equipment.md)

## Situatie

Een speler kan meerdere wapens verzamelen. Je maakt een `Inventory` die wapens toevoegt, verwijdert, door elkaar husselt en hun gewicht optelt.

## Leerdoelen

`List<T>` gebruiken, constructor-overloading, collectiemethods, een berekende read-only property, `foreach` en een kopie van een lijst maken.

## Korte theorie

Een `List<Weapon>` bevat verwijzingen naar `Weapon`-objecten. Een nieuwe lijst kan dezelfde objecten bevatten zonder dat de volgorde van de originele lijst verandert.

```csharp
List<int> levels = new() { 3, 8, 5 };
int total = 0;
foreach (int level in levels) total += level;
```

## Opdrachten

### Stap 1 — Maak `Inventory`

Maak `Weapons/Inventory.cs` in namespace `RpgEquipment.Weapons`.

### Stap 2 — Voeg één private field toe

`weapons` van type `List<Weapon>`.

### Stap 3 — Voeg constructors toe

```csharp
public Inventory()
public Inventory(List<Weapon> weapons)
```

De eerste maakt een lege lijst; de tweede gebruikt de meegegeven lijst.

### Stap 4 — Voeg methods toe

| Method | Returntype | Gedrag |
|---|---|---|
| `Add(Weapon weapon)` | `void` | Voegt wapen toe |
| `Remove(Weapon weapon)` | `void` | Verwijdert wapen |
| `Clear()` | `void` | Maakt lijst leeg |
| `GetAllWeapons()` | `List<Weapon>` | Retourneert lijst |
| `Shuffle()` | `List<Weapon>` | Retourneert willekeurig geordende **kopie** |

`Shuffle()` mag de volgorde van het private field `weapons` **niet wijzigen**. Gebruik bijvoorbeeld `Random` en een lus; LINQ is niet nodig.

### Stap 5 — Voeg read-only properties toe

| Property | Type | Betekenis |
|---|---|---|
| `Count` | `int` | Aantal wapens |
| `TotalWeight` | `double` | Som van `Weight` van alle wapens |

Gebruik `foreach` om `TotalWeight` te berekenen.

### Stap 6 — Test

Maak drie wapens met gewichten `3.5`, `1.0` en `2.25` kg. Voeg ze toe aan een lege inventory.

- `Count` is eerst **3** en `TotalWeight` is **6.75**.
- Verwijder het derde wapen: `Count` is **2** en `TotalWeight` is **4.5**.
- Voeg het terug; `TotalWeight` is weer **6.75**.
- Controleer dat `Shuffle()` een lijst met dezelfde drie wapens oplevert en dat de originele volgorde behouden blijft.
- Controleer `Clear()` en een lege inventory.

## Denkvragen

1. Wat is het verschil tussen een `Inventory` en één `Weapon`?
2. Waarom heeft `TotalWeight` geen setter?
3. Waarom mag `Shuffle()` niet rechtstreeks de originele lijst veranderen?

## Eindcontrole

- [ ] Twee constructors, vijf methods en twee read-only properties.
- [ ] Gewicht wordt met een lus opgeteld.
- [ ] Shuffle behoudt de originele lijstvolgorde.
- [ ] Alle testscenario's werken.
