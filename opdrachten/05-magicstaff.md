# Opdracht 5 — MagicStaff

[← Overzicht](../README.md) · [← Vorige](04-sword.md) · [Volgende →](06-equipmentset.md)

## Situatie

Een magische staf kan een spreuk activeren, heeft een vaste maximale mana, een basiswaarde voor magische schade en een multiplier. Je gebruikt een interface om vast te leggen welke functionaliteit magische items bieden.

## Leerdoelen

Interface implementeren, `readonly` field, constructor, overriding, berekende property en een status die wisselt toepassen.

## Theorie

### 1. Wat is een interface?

Een **interface** beschrijft welke publieke members een klasse moet aanbieden, zonder de gewone implementatie voor die klasse uit te schrijven. Verschillende klassen kunnen dezelfde interface implementeren.

```csharp
public interface IUsable
{
    string Use();
}

public class Potion : IUsable
{
    public string Use()
    {
        return "Potion consumed";
    }
}
```

Door `: IUsable` moet `Potion` de method `Use()` aanbieden. Andere code kan een object via het interfacetype gebruiken:

```csharp
IUsable item = new Potion();
Console.WriteLine(item.Use());
```

Een interface is anders dan een abstracte basisklasse: een klasse kan meerdere interfaces implementeren, maar slechts van één klasse erven.

### 2. `readonly` en een read-only property

Een `readonly` field krijgt zijn waarde bij declaratie of in de constructor. Daarna kan het niet opnieuw worden toegewezen. Een **read-only property** heeft alleen een `get` en kan een berekening teruggeven.

```csharp
public class Potion
{
    private readonly int maxCharges = 5;
    private int chargesUsed;

    public int ChargesLeft
    {
        get { return maxCharges - chargesUsed; }
    }
}
```

`ChargesLeft` heeft geen setter: de waarde volgt uit twee fields.

### 3. Status wijzigen met een method

Een `bool` heeft de waarde `true` of `false`. Met `!` draai je die waarde om:

```csharp
bool isOpen = false;
isOpen = !isOpen; // true
isOpen = !isOpen; // false
```

Zo kan een method een aan/uit-status wisselen zonder dat andere code rechtstreeks bij het private field kan.

### 4. Overriding naast een interface

Een klasse kan tegelijk een method uit een interface implementeren én een geërfde abstracte method met `override` invullen. Het zijn twee verschillende verplichtingen: de interface bepaalt **welke functionaliteit beschikbaar is**, de basisklasse bepaalt **wat afgeleide klassen gemeen hebben**.

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
