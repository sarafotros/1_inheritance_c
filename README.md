
# Inheritance

## What's the point of inheritance?

Mainly, it's for reusability. As you have identified a bunch of class that have shared properties/methods, by identifying a base class you can abstract away a lot of the shared plumbing into a single place. From there, it can be easily applied to the class with `:`. 

The code becomes easier to read and maintain when things are grouped properly. Code and classes are cleaner so there is less noise to read through. This code can then be extended a lot easier too.

[MSDN on Inheritance](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/inheritance#abstract-and-virtual-methods)

## An Example

Fields and methods can be inherited from a `base`  (parent) class into a `derived` (child) class with the `:` symbol:

```c#
public class Animal // this is the base class
{
    public string Species; // this is a property of the base class
    public void MakeNoise() // this is a method of the base class
    {
        Console.WriteLine("unknown animal sound");
    }
}
public class Lion 
    : Animal // this is Lion inherits from Animal
{
    public string Species = "Lion"; // this property comes from Animal
    public void MakeNoise() // this method comes from Animal
    {
        Console.WriteLine("ROOOOOAARRRR!!");
    }
}
public class Warthog 
    : Animal // this is Warthog inherits from Animal
{
    public string Species = "Warthog"; // this property comes from Animal
    public void MakeNoise() // this method comes from Animal
    {
        Console.WriteLine("Hakuna");
    }
    public void DoWarthogStuff()
    {
        //...
    }
}
public class Meerkat 
    : Animal // this is Meerkat inherits from Animal
{
    public string Species = "Meerkat"; // this property comes from Animal
    public void MakeNoise() // this method comes from Animal
    {
        Console.WritLine("Matata");
    }
    public void DoMeerkatStuff()
    {
        //...
    }
}
```

usage:

```c#
// all 3 are of type Animal
// their instance is set to Lion/Warthog/Meerkat but because their type is animal this code is valid
Animal simba = new Lion();
Animal pumba = new Warthog();
Animal timon = new Meerkat();

simba.MakeNoise(); // OUTPUT: ROOOOOAARRRR!!
pumba.MakeNoise(); // OUTPUT: Hakuna
timon.MakeNoise(); // OUTPUT: Matata
```

## Another example

We have the concept of a `Vehicle` which is a way to move something from A to B. Every vehicle has a top speed.

So we have the same structure as before:

- a `base` class with a property
- 3 `derived` classes which make use of the property

```c#
public class Vehicle
{
    public int TopSpeed = 0;
}
public class Car 
    : Vehicle
{
	public int TopSpeed = 100;
}
public class Truck
    : Vehicle
{
	public int TopSpeed = 70;
}
public class BlackpoolBeachDonkey
    : Vehicle
{
	public int TopSpeed = 1;
}
```

Now that we have finished mapping out the requirements, the PM says "actually, we need to also track the type of fuel that the vehicles use so that we can make sure we always have some available".  Without inheritance, we'd have to go into every type of vehicle and make sure to add the new `FuelType` property. If a class was missed, well tough luck.

There's a better way. If instead we added the `FuelType` property to the `base` class, then any `derived` class who `inherits` from the `base` will be forced to implement the new member (we'd get a red squiggle and compile error too 👍).

Give it a go.

## Multi-Level Inheritance

Inheritance all the way down. A `derived` class can `inherit` from a `base` class, which itself is a `derived` class of another `base` class. A good way to picture this is a family tree:

```c#
public class GrandParent
{
	// genetic properties are inherited like this
	public bool BigNose = true;
}
public class Parent : GrandParent
{
    public bool NeedsGlasses = true;
	public bool HasABigNose()
    {    
        return this.BigNose; // property from Parent
    }
}
public class Child : Parent
{
    // the child can access properties on both Parent and GrandParent
    public void WillNeedGlasses()
    {
        return this.NeedsGlasses; // property from Parent
    }
    public void HasTheirGrandparentsNose()
    {
        return this.BigNose; // property from GrandParent
    }
}
```

### Semi-Tangent

A child has 2 biological parents:

```c#
public class Parent { ... }
public class Mother : Parent { ... }
public class Father : Parent { ... }
```

Unfortunately, we can't inherit from multiple `base` classes so something like this wouldn't work:

```c#
public class Child : Mother, Father {}
```

If for whatever reason you need 2 `base` classes, a way around it is to have those `base` classes as members of the class that you want to be the `derived` class:

```c#
public class Child
{
    public Mother Mum { get; private set; }
    public Father Dad { get; private set; }
    
    public Child(Mother mum, Father dad)
    {
        this.Mum = mum;
        this.Dad = dad;
    }
}
...
public void Create()
{
	var mum = new Mum();
    var dad = new Dad();
    var me = new Child(mum, dad);
}

```

## Advanced bits

### Abstract & Virtual

When a `base` class doesn't make sense to be an instance on it's own, it should be made into an `abstract` class.  From the examples above `Animal`, `Vehicle`, and `Parent` would be abstract.

`abstract` classes, methods, and properties don't have any code to them, it's basically just a shell. Any `derived` class that has a `base` class with an abstract method/property MUST override the `base` class' implementation with it's own.

```c#
public abstract class Vehicle
{
    public abstract int TopSpeed;
}
public class BlackpoolBeachDonkey 
    : Vehicle
{
	public override int TopSpeed = 1;
}
```

`virtual` methods/properties already have an implementation. They can either be left alone, or overridden:

```c#
public abstract class Vehicle
{
    public virtual int TopSpeed = 1;
}
public class BlackpoolBeachDonkey 
    : Vehicle
{ 
	// we don't need to override TopSpeed as we are happy with the default implementation
}
public class TeslaModelS
{
    public override int TopSpeed = 200;
}
```

# Your turn!

Using what you now know about inheritance, try the tasks below:

## 1: Channel your inner Sir David Attenborough

Map the [animal taxonomy](https://examples.yourdictionary.com/basic-types-of-animals-and-their-characteristics.html) to a class structure. You'll need to identify which classes will be `base`, which classes will be `derived` and how they'll all work together.

# Notes:

[STACK:  What do do if I need more than one base class in C#?](https://softwareengineering.stackexchange.com/questions/312327/what-to-do-if-i-need-more-than-one-base-class-in-c)

[BLOG: complete intro](https://blog.submain.com/c-inheritance-complete-introduction/)

[VIDEO: Inheritance tutorial](https://www.youtube.com/watch?v=EiBCF7rYRtI)
