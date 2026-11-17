# Opdracht 4 — Sword

[← Overzicht](../README.md) · [← Vorige](03-equipment.md) · [Volgende →](05-magicstaff.md)

## Situatie

Je maakt een gespecialiseerd wapen: een `Sword`. Een zwaard erft de algemene eigenschappen van `Weapon` en krijgt een eigen zwaardtype en abilitybeschrijving.

## Leerdoelen

Overerving, constructor met `base`, enum, property en overriding toepassen.

## Theorie

### 1. Overerving: `is een`

Bij **overerving** breidt een klasse een andere klasse uit. Een `Sword` **is een** `Weapon`. Daardoor hoeft `Sword` algemene gegevens zoals naam en ID niet opnieuw te definiëren.

```csharp
public class Character
{
    public string Name { get; set; } = "";
}

public class Mage : Character
{
    public int Mana { get; set; }
}
```

Een `Mage` heeft zowel de geërfde property `Name` als de eigen property `Mana`.

### 2. Een constructor van de basisklasse aanroepen

Met `base(...)` geef je argumenten door aan een constructor van de basisklasse:

```csharp
public class Character
{
    public Character(int id) { Id = id; }
    public int Id { get; }
}

public class Mage : Character
{
    public Mage(int id) : base(id) { }
}
```

Hier initialiseert `Character` de geërfde ID. `Mage` hoeft dat niet nog eens te doen.

### 3. `virtual`, `abstract` en `override`

Een method met `virtual` heeft een basisimplementatie die een afgeleide klasse **mag** vervangen. Een abstracte method heeft geen basisimplementatie en moet door een concrete afgeleide klasse worden ingevuld. Met `override` geef je de eigen implementatie:

```csharp
public abstract class Character
{
    public abstract string GetRole();
}

public class Mage : Character
{
    public override string GetRole()
    {
        return "Spellcaster";
    }
}
```

### 4. Een `enum` met `switch`

Een enum bevat vaste keuzewaarden. Met een `switch` kun je voor elke waarde een andere uitkomst kiezen. Vergeet niet wat er moet gebeuren bij een onbekende of standaardwaarde.

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
