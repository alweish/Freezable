[![Chat on Gitter](https://img.shields.io/gitter/room/fody/fody.svg?style=flat&max-age=86400)](https://gitter.im/Fody/Fody)
[![NuGet Status](http://img.shields.io/nuget/v/Freezable.Fody.svg?style=flat&max-age=86400)](https://www.nuget.org/packages/Freezable.Fody/)


## This is an add-in for [Fody](https://github.com/Fody/Home/)

![Icon](https://raw.githubusercontent.com/Fody/Freezable/master/package_icon.png)

Implements the Freezable pattern


## Usage

See also [Fody usage](https://github.com/Fody/Home/blob/master/pages/usage.md).


### NuGet installation

Install the [Freezable.Fody NuGet package](https://nuget.org/packages/Freezable.Fody/) and update the [Fody NuGet package](https://nuget.org/packages/Fody/):

```powershell
PM> Install-Package Fody
PM> Install-Package Freezable.Fody
```

The `Install-Package Fody` is required since NuGet always defaults to the oldest, and most buggy, version of any dependency.


### Add to FodyWeavers.xml

Add `<Freezable/>` to [FodyWeavers.xml](https://github.com/Fody/Home/blob/master/pages/usage.md#add-fodyweaversxml)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Weavers>
  <Freezable/>
</Weavers>
```


### Add an interface

```csharp
public interface IFreezable
{
    void Freeze();
}
```


### Add a freezable class

```csharp
public class Person : IFreezable
{
    bool isFrozen;
    public string Name { get; set; }

    public void Freeze()
    {
        isFrozen = true;
    }
}
```


### What gets compiled

```csharp
public class Person : IFreezable
{
    volatile bool isFrozen;
    public void Freeze()
    {
        isFrozen = true;
    }

    void CheckIfFrozen()
    {
        if (isFrozen)
        {
            throw new InvalidOperationException("Attempted to modify a frozen instance");
        }
    }

    string name;
    public string Name
    {
        get { return name; }
        set
        {
            CheckIfFrozen();
            name = value;
        }
    }
}
```


## Icon

Icon courtesy of [The Noun Project](http://thenounproject.com)