# My Unity C# Style Guide

Throughout my programming career I've found that when asking the internet a styling question, the most common answer almost always "it depends" or to have two authoritative sources constantly contradict each other (I'm looking at you, MSDN and Google style guides). If people can agree on one thing, however, is that to be consistent is more important than to be "right", whatever that means in this context. For this reason, this document will serve as the basis for my programming style and recommendations; I can only promise they'll be as objectively subjective as anyone else's. [Obligatory xkcd reference](https://xkcd.com/927/).
# General

 - Column width limit at 100
 - [Allman Indentation style](https://en.wikipedia.org/wiki/Indentation_style#Allman_style)
	excerpt:

		while (x == y)
		{
		    something();
		    somethingelse();
		}
		finalthing();

## Casing
|Element         |Case                           |Example                       |
|----------------|-------------------------------|------------------------------|
|Class names     |Pascal Case                    |`class ExampleClass {}`       |
|Enumerator names|Pascal Case                    |`enum ExampleEnum {}`       |
|Interface names |Pascal Case                    |`interface ExampleInterface {}`|
|private Class members |camelCase with leading underscore |`_myRenderer`        |
|public Class members  |camelCase                |`someClass.someMember`        |

## About Reserved Identifiers
Due to `MonoBehaviour`'s class members, several commonly needed variable names such as `renderer` and `collider` are already taken. In those cases, use a `my` prefix (regardless of underscore usage) if the component is attached to the Game Object, or a single identifier word otherwise, i.e.: `iconRenderer`

### Rect Transform
Every Game Object comes with a `Transform` component if in World Space or a `Rect Transform` component if in Canvas Space. However, due to Unity's peculiarities, assign a `GetComponent<RectTransform>` result to a local variable. `rt`, `_rt`, `rectTransform` and `_rectTransform` are all acceptable, so long as there's consistency within the same file.

## Protection levels & Inspector
 - Variables are of protection level `private` unless contextually necessary. When private, protection level is only explicitly stated to make a point about encapsulation sensibilities, i.e.: `private byte passKey` communicates that it should never be made public.
 - Serialize `MonoBehaviour` private members when possible. If their value is always assigned via script, use `[ReadOnly]`:
`[SerializeField] private int _myVariable`
 - Hide public variables when only meant to be accessed by other classes:
	 - `[HideInInspector] public int myVariable`

## Methods & Functions
 - When delegating methods to Event Triggers, do not use Trigger naming conventions, i.e.: `OnButtonWasClicked()`. Instead, use a name that describes the procedure as with other methods

## Documentation
 - Code should be as self-explanatory as possible. Thus, use comments to explain **why** something is the way it is instead of **how** it works.
 - Repeated use of triple slash `/// comment` is prefferable to opening and closing commenting brackets `/* comment */`
 - Comment Methods with the `<Summary>` XML tag

## Delegates and Execution Order

 - Maintain naming convention consistency throughout scripts when referring to recurring managers and components.
 - Assign component references inside `Awake()` when located in the same Game Object as the current script.
 - Assign component references inside `Start()` when outside the GameObject, even if within the same hierarchy.
 - The `Find-` family of `MonoBehaviour` methods is recommended for expediency as long as they're seldom used inside Event Triggers and **never** inside `Update()`, `FixedUpdate()` and `LateUpdate()`

## Line Breaks
Prioritize breaking after variable assignment over parameter declaration, i.e.:

    Vector3 somePosition =
            GetSomePosition(parameter1, parameter2, parameter3)

over

    Vector3 somePosition = GetSomePosition(
		    parameter1, parameter2, parameter3)

Dispense with braces entirely when nesting single-line loops, i.e.:

    for each (int number in series)
        DoSomething();
over 

    for each (int number in series)
    {
        DoSomething();
    }

Use braces for most method bodies. When returning a value defined in a single line, use arrow notation instead, i.e.:

	void GetDouble(int value) => value * 2;
## Interfaces
## Enumerators
 - All enumerators are of protection level `Public` and available in a single `Enumerators.cs` file inside `Assets/Scripts/Utilities`.
 - Enumerator variables are singular, even if they represent a group of elements, i.e. `Color.Red` over `Colors.Red`
 - Members are always ordered contextually over alphabetically, i.e.:

		public enum Metals
		{
		    Copper,
		    Iron,
		    Gold,
		    Platinum
		}
