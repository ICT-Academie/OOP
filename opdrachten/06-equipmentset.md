# Opdracht 6 — EquipmentSet

[← Overzicht](../README.md) · [← Vorige](05-magicstaff.md)

## Situatie

Je bouwt een `EquipmentSet` die een bestaande inventory met wapens kan koppelen. Een complete set kan worden geactiveerd en een bonus berekenen. Je combineert overerving, twee interfaces, compositie en null-controles.

## Leerdoelen

Twee interfaces implementeren, een object als field gebruiken, abstracte method overschrijven, null controleren, gekoppelde collecties controleren en onderdelen samen testen.

## Korte theorie

Bij **compositie** heeft een object een verwijzing naar een ander object. Een `EquipmentSet` **heeft een** `Inventory`; een `EquipmentSet` **is geen** `Inventory`. Een niet-gekoppelde inventory is `null`.

```csharp
public class Quest
{
    private string? reward;
    public bool HasReward() { return reward != null; }
}
```

## Opdrachten

### Stap 1 — Maak de klasse

Maak `Equipment/EquipmentSet.cs` in namespace `RpgEquipment.Equipment`. Laat `EquipmentSet` erven van `Equipment`.

### Stap 2 — Maak twee interfaces

Maak `Interfaces/IInventoryHolder.cs`:

```csharp
using RpgEquipment.Weapons;

public interface IInventoryHolder
{
    bool HasWeapons();
    void AttachInventory(Inventory inventory);
    void DetachInventory();
    Inventory? Inventory { get; }
}
```

Maak `Interfaces/ISetBonus.cs`:

```csharp
public interface ISetBonus
{
    string GetBonusInfo();
    int TotalBonus { get; }
}
```

Plaats beide interfaces in namespace `RpgEquipment.Interfaces` en laat `EquipmentSet` beide implementeren.

### Stap 3 — Voeg private fields toe

| Field | Type | Betekenis |
|---|---|---|
| `inventory` | `Inventory?` | Gekoppelde inventory of `null` |
| `bonusPerWeapon` | `int` | Bonuspunten per wapen |
| `requiredWeapons` | `int` | Minimaal aantal wapens voor een bonus |
| `isActive` | `bool` | Of de set actief is |

### Stap 4 — Constructor

Maak `public EquipmentSet(int id)`. Sla `id` op in het geërfde field. Zet `requiredWeapons` standaard op **3**.

### Stap 5 — Implementeer methods

| Method | Returntype | Gedrag |
|---|---|---|
| `HasWeapons()` | `bool` | `true` als inventory bestaat en `Count > 0` |
| `AttachInventory(Inventory inventory)` | `void` | Koppelt inventory |
| `DetachInventory()` | `void` | Zet koppeling op `null` |
| `GetBonusInfo()` | `string` | `Set bonus: [TotalBonus].` |
| `DescribeAbility()` | `string` | Override; beschrijft actieve setbonus of meldt dat set niet actief is |
| `ToggleActive()` | `void` | Wisselt `isActive` |

### Stap 6 — Properties

| Property | Type | get | set |
|---|---|:---:|:---:|
| `Inventory` | `Inventory?` | ✓ | — |
| `BonusPerWeapon` | `int` | ✓ | ✓ |
| `RequiredWeapons` | `int` | ✓ | ✓ |
| `IsActive` | `bool` | ✓ | — |
| `TotalBonus` | `int` | ✓ | — |

`TotalBonus` is **0** als de set niet actief is, als geen inventory is gekoppeld of als `Inventory.Count` kleiner is dan `RequiredWeapons`. Anders is `Inventory.Count * BonusPerWeapon`.

### Stap 7 — Test de volledige keten

Maak drie `Weapon`-objecten en voeg ze toe aan een `Inventory`. Maak `EquipmentSet set = new EquipmentSet(300);`, stel `BonusPerWeapon = 5` in en koppel de inventory.

- Zonder inventory: `HasWeapons()` is `false`, `Inventory` is `null` en `TotalBonus` is **0**.
- Na koppelen: `HasWeapons()` is `true`, maar `TotalBonus` blijft **0** zolang de set niet actief is.
- Na activeren: `TotalBonus` is **15**.
- Na verwijderen van één wapen: `TotalBonus` is **0** (minder dan drie wapens).
- Na opnieuw toevoegen: `TotalBonus` is weer **15**.
- Na `DetachInventory()`: `HasWeapons()` is `false` en `TotalBonus` is **0**.
- Test ook een lege gekoppelde inventory en beide interfaces via interfacevariabelen.
- Controleer opnieuw dat `Sword` en `MagicStaff` nog werken.

## Denkvragen

1. Waarom gebruikt `EquipmentSet` compositie in plaats van overerving van `Inventory`?
2. Waarom controleer je op `null` voordat je `Count` leest?
3. Wat is het voordeel van twee aparte interfaces?
4. Waarom heeft `TotalBonus` geen setter?

## Eindcontrole

- [ ] Twee interfaces geïmplementeerd.
- [ ] Vier private fields en één constructor.
- [ ] Methods, properties en null-controles werken.
- [ ] Setbonus verandert bij activeren, verwijderen en ontkoppelen.
- [ ] Alle eerdere opdrachten blijven compileerbaar.
