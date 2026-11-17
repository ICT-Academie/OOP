# Opdracht 1 — Weapon

[← Overzicht](../README.md) · [← Vorige](00-rpg-item.md) · [Volgende →](02-inventory.md)

## Situatie

Je bouwt een herbruikbare `Weapon`-klasse. Ieder wapen heeft een naam, een beschrijving, een categorie, een gewicht en een tijd tussen twee aanvallen.

## Leerdoelen

- Verschil tussen klasse en object uitleggen.
- Zes private fields en een enum gebruiken.
- Vier overloaded constructors schrijven.
- Methods met returntypes gebruiken.
- Read-only, write-only en read/write properties toepassen.
- Een Class Library vanuit een Console App testen.

## Benodigdheden

.NET 10 SDK, C#-IDE en een nieuwe solution.

## Korte theorie

`private` fields zijn alleen binnen de klasse rechtstreeks toegankelijk. Properties bepalen hoe andere code gegevens leest en wijzigt. Constructors worden uitgevoerd bij `new`. `TimeSpan` bewaart een tijdsduur; `TotalSeconds` levert een `double` op.

```csharp
public class Potion
{
    private string name = "";
    public Potion(string name) { this.name = name; }
    public string Name { get { return name; } set { name = value; } }
}
```

## Opdrachten

### Stap 1 — Maak de solution

Maak `RpgEquipmentSystem` met een Class Library `RpgEquipment` en een Console App `Tester`. Voeg een projectreferentie van `Tester` naar `RpgEquipment` toe.

```bash
dotnet new sln -n RpgEquipmentSystem
dotnet new classlib -n RpgEquipment -f net10.0
dotnet new console -n Tester -f net10.0
dotnet sln add RpgEquipment/RpgEquipment.csproj Tester/Tester.csproj
dotnet add Tester/Tester.csproj reference RpgEquipment/RpgEquipment.csproj
```

Maak `Weapons/Weapon.cs` in namespace `RpgEquipment.Weapons`.

### Stap 2 — Maak fields en enum

| Private field | Type | Betekenis |
|---|---|---|
| `id` | `int` | Identificatie |
| `name` | `string` | Wapennaam |
| `description` | `string` | Beschrijving |
| `weight` | `double` | Gewicht in kg |
| `category` | `WeaponCategory` | Soort wapen |
| `attackCooldown` | `TimeSpan` | Tijd tussen aanvallen |

Initialiseer strings veilig met `""`. Definieer een publieke enum `WeaponCategory` met **precies 21** waarden:

`Sword, Axe, Bow, Dagger, Staff, Spear, Hammer, Mace, Crossbow, Wand, Scythe, Halberd, Flail, Whip, Claw, Shield, Greatsword, Katana, Rapier, ThrowingKnife, Other`.

### Stap 3 — Schrijf vier constructors

```csharp
public Weapon()
public Weapon(int id)
public Weapon(int id, string name)
public Weapon(int id, string name, string description)
```

Sla alle meegegeven parameters op. Alle vier vormen moeten compileerbaar zijn.

### Stap 4 — Schrijf methods

| Method | Returntype | Gedrag |
|---|---|---|
| `GetAttackCooldown()` | `TimeSpan` | Retourneert de ingestelde cooldown |
| `GetCooldownInSeconds()` | `int` | Retourneert het gehele aantal seconden |

Gebruik `TotalSeconds` en zet dit om naar `int`.

### Stap 5 — Schrijf properties

| Property | Type | get | set | Gedrag |
|---|---|:---:|:---:|---|
| `Id` | `int` | ✓ | ✓ | ID |
| `Name` | `string` | ✓ | ✓ | Naam |
| `Description` | `string` | ✓ | ✓ | Beschrijving |
| `DisplayName` | `string` | ✓ | — | Naam met categorie, anders `"unknown"` |
| `AttackCooldown` | `TimeSpan` | — | ✓ | Schrijft cooldown |
| `DisplayCooldown` | `string` | ✓ | — | Leesbare cooldown |
| `Category` | `WeaponCategory` | ✓ | ✓ | Categorie |
| `Weight` | `double` | ✓ | ✓ | Gewicht |

`DisplayName` geeft exact `"unknown"` terug wanneer `Name` leeg is; anders bijvoorbeeld `"Sword: Iron Sword"`. `DisplayCooldown` geeft een leesbare weergave van de `TimeSpan`. Lees de cooldown uitsluitend via `GetAttackCooldown()`.

### Stap 6 — Test

Maak `Weapon sword = new Weapon(1, "Iron Sword", "A sturdy iron blade");`. Stel `Category` in op `WeaponCategory.Sword`, `Weight` op `3.5` en `AttackCooldown` op `new TimeSpan(0, 0, 4)`.

Controleer: `DisplayName` is `Sword: Iron Sword`; `GetCooldownInSeconds()` is `4`. Test ook een wapen zonder naam: `DisplayName` is `unknown`. Maak daarnaast objecten met alle vier constructors.

## Denkvragen

1. Waarom is `AttackCooldown` write-only, maar bestaat `GetAttackCooldown()` wel?
2. Waarom is `DisplayName` read-only?
3. Welke constructor wordt gebruikt bij `new Weapon(3, "Bow")`?

## Eindcontrole

- [ ] Zes private fields en enum met 21 waarden.
- [ ] Vier constructors en twee methods.
- [ ] Acht properties met de juiste accessors.
- [ ] Testproject compileert en controlewaarden kloppen.
