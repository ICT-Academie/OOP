# Opdracht 5 — MagicStaff

[← Overzicht](../README.md) · [← Vorige](04-sword.md) · [Volgende →](06-equipmentset.md)

## Situatie

Een magische staf kan een spreuk activeren, heeft een vaste maximale mana, een basiswaarde voor magische schade en een multiplier. Je gebruikt een interface om vast te leggen welke functionaliteit magische items bieden.

## Leerdoelen

Interface implementeren, `readonly` field, constructor, overriding, berekende property en een status die wisselt toepassen.

## Korte theorie

Een interface legt vast welke members een klasse aanbiedt. Een `readonly` field kan na initialisatie niet zomaar worden gewijzigd. Een read-only property kan een berekening uitvoeren.

```csharp
public interface IRechargeable
{
    bool IsCharged { get; }
}
```

## Opdrachten

### Stap 1 — Maak interface en klasse

Maak `Interfaces/IMagic.cs` in namespace `RpgEquipment.Interfaces`:

```csharp
public interface IMagic
{
    string GetMagicInfo();
    int TotalMagicDamage { get; }
}
```

Maak `MagicStaff` in `RpgEquipment.Weapons`, afgeleid van `Weapon`, en implementeer `IMagic`.

### Stap 2 — Voeg private fields toe

| Field | Type | Startwaarde |
|---|---|---|
| `maxMana` | `int` (`readonly`) | `700` |
| `baseMagicDamage` | `int` | `0` |
| `magicMultiplier` | `int` | `0` |
| `isActivated` | `bool` | `false` |

### Stap 3 — Constructor

Maak `public MagicStaff(int id)` en gebruik `base(id)`.

### Stap 4 — Methods

| Method | Returntype | Gedrag |
|---|---|---|
| `GetMagicInfo()` | `string` | `Magic damage: [TotalMagicDamage].` |
| `DescribeAbility()` | `string` | Override; `Max mana: 700.` |
| `ToggleActivation()` | `void` | Wisselt `isActivated` tussen true en false |

### Stap 5 — Properties

| Property | Type | get | set |
|---|---|:---:|:---:|
| `MaxMana` | `int` | ✓ | — |
| `BaseMagicDamage` | `int` | ✓ | ✓ |
| `MagicMultiplier` | `int` | ✓ | ✓ |
| `TotalMagicDamage` | `int` | ✓ | — |
| `IsActivated` | `bool` | ✓ | — |

`TotalMagicDamage` is `BaseMagicDamage * MagicMultiplier`.

### Stap 6 — Test

Maak een `MagicStaff` met ID 200, `BaseMagicDamage = 40` en `MagicMultiplier = 3`.

- `MaxMana` is **700**.
- `TotalMagicDamage` is **120**.
- `GetMagicInfo()` geeft `Magic damage: 120.`.
- `IsActivated` begint op `false`, wordt `true` na één toggle en weer `false` na twee toggles.
- Controleer dat `MagicStaff` via een variabele van type `IMagic` kan worden aangesproken.

## Denkvragen

1. Wat is het verschil tussen een interface en een abstracte klasse?
2. Waarom is `TotalMagicDamage` read-only?
3. Waarom is `maxMana` readonly?

## Eindcontrole

- [ ] `IMagic` correct geïmplementeerd.
- [ ] Vier private fields, inclusief readonly en bool-status.
- [ ] Constructor, drie methods en vijf properties.
- [ ] Berekening en statuswissel werken.
