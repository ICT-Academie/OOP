# Opdracht 2 — Inventory

[← Overzicht](../README.md) · [← Vorige](01-weapon.md) · [Volgende →](03-equipment.md)

## Situatie

Een speler kan meerdere wapens verzamelen. Je maakt een `Inventory` die wapens toevoegt, verwijdert, door elkaar husselt en hun gewicht optelt.

## Leerdoelen

`List<T>` gebruiken, constructor-overloading, collectiemethods, een berekende read-only property, `foreach` en een kopie van een lijst maken.

## Theorie

### 1. Een lijst met objecten

Met `List<T>` bewaar je meerdere waarden van hetzelfde type. `T` is het type dat in de lijst mag staan. Een `List<Weapon>` bevat dus `Weapon`-objecten; een `List<int>` bevat gehele getallen.

```csharp
List<string> quests = new List<string>();
quests.Add("Find the key");
quests.Add("Defeat the boss");
Console.WriteLine(quests.Count); // 2
quests.Remove("Find the key");
```

`Add` voegt toe, `Remove` verwijdert één overeenkomend element, `Clear` maakt de lijst leeg en `Count` geeft het aantal elementen.

### 2. Een lijst doorlopen met `foreach`

Met `foreach` verwerk je elk element van een lijst. Je hoeft zelf geen index bij te houden:

```csharp
List<int> levels = new() { 3, 8, 5 };
int total = 0;
foreach (int level in levels)
{
    total += level;
}
Console.WriteLine(total); // 16
```

`total += level` betekent hetzelfde als `total = total + level`. Op deze manier kun je ook gewichten van objecten optellen.

### 3. Constructors en berekende properties

Een constructor kan een lege lijst maken of een bestaande lijst ontvangen. Dit zijn twee verschillende parameterlijsten en dus **constructor-overloading**. Een **berekende read-only property** kan telkens opnieuw een waarde uit de huidige lijst berekenen:

```csharp
public class QuestLog
{
    private List<string> quests = new();

    public int Count
    {
        get { return quests.Count; }
    }
}
```

Een setter is hier niet logisch: het aantal volgt uit de inhoud van de lijst.

### 4. Een kopie maken zonder de originele volgorde te wijzigen

```csharp
List<int> original = new() { 1, 2, 3 };
List<int> copy = new(original);
copy.Reverse();
```

Na `Reverse()` is `copy` omgekeerd, maar `original` blijft `[1, 2, 3]`. Bij lijsten met objecten kopieer je op deze manier **de lijst**, niet de objecten zelf: beide lijsten kunnen naar dezelfde objecten verwijzen. Voor een willekeurige volgorde kun je een `Random` en een lus gebruiken.

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
