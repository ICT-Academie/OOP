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

## Theorie

### 1. Klassen, objecten en private fields

Een **klasse** is een beschrijving van gegevens en gedrag; een **object** is een concreet exemplaar dat je met `new` aanmaakt. Met een `private` field sla je gegevens op die niet rechtstreeks vanuit andere klassen gewijzigd kunnen worden. Een publieke **property** bepaalt hoe andere code die gegevens mag lezen en aanpassen.

```csharp
public class Potion
{
    private string name = "";

    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

Bij `potion.Name = "Healing"` wordt de `set` uitgevoerd. Bij `Console.WriteLine(potion.Name)` wordt de `get` uitgevoerd. Binnen een setter bevat `value` de nieuwe waarde.

### 2. Constructors en constructor-overloading

Een **constructor** wordt automatisch uitgevoerd wanneer je een object met `new` maakt. Je gebruikt een constructor om een object meteen beginwaarden te geven. De constructor heeft **dezelfde naam als de klasse** en **geen returntype** (dus ook niet `void`).

```csharp
public class Potion
{
    private string name = "";
    private int healing;

    public Potion(string name)
    {
        this.name = name;
    }

    public Potion(string name, int healing)
    {
        this.name = name;
        this.healing = healing;
    }
}

Potion first = new Potion("Small Potion");
Potion second = new Potion("Large Potion", 80);
```

`this.name` is het field van het huidige object. `name` zonder `this` is de parameter die aan de constructor is meegegeven. Beide constructors heten `Potion`, maar hebben andere **parameterlijsten**. Dit heet **constructor-overloading**. C# kiest de constructor die past bij de argumenten achter `new`.

Een parameterloze constructor, zoals `public Potion() { }`, laat je een object zonder argumenten maken. Zodra je zelf constructors definieert, moet je een parameterloze constructor expliciet toevoegen als je `new Potion()` wilt blijven gebruiken.

### 3. Methods en returntypes

Een **method** voert een handeling uit of berekent iets. Het **returntype** bepaalt welke waarde terugkomt. `void` betekent dat een method niets teruggeeft.

```csharp
public int DoubleHealing(int amount)
{
    return amount * 2;
}
```

Bij een method met returntype `int` moet je een geheel getal teruggeven. `return` levert die waarde aan de aanroepende code.

### 4. Enums en `TimeSpan`

Een **enum** geeft een vaste verzameling keuzes een naam. Zo voorkom je dat je overal losse tekstwaarden moet vergelijken:

```csharp
public enum PotionRarity
{
    Common,
    Rare,
    Legendary
}
```

Een **`TimeSpan`** stelt een tijdsduur voor. `new TimeSpan(0, 0, 3)` betekent 0 uur, 0 minuten en 3 seconden. De property `TotalSeconds` geeft de volledige duur als `double` terug. Als een opdracht een `int` vraagt, moet je bewust bepalen hoe je die waarde omzet, bijvoorbeeld met `(int)duration.TotalSeconds`.

### 5. Verschillende soorten properties

- **Read/write:** heeft zowel `get` als `set` en kan gelezen en gewijzigd worden.
- **Read-only:** heeft alleen `get`; andere code kan de waarde niet via die property instellen.
- **Write-only:** heeft alleen `set`; andere code kan via die property wel een waarde instellen, maar niet uitlezen.
- **Berekend:** de `get` stelt een resultaat samen uit andere gegevens, in plaats van een afzonderlijk field terug te geven.

Schrijf de volledige `Weapon`-klasse zelf. De voorbeelden gebruiken bewust een andere klasse.

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
