## Modeling Planning Domains

New techniques for modeling planning domains and problems could be explored such that these new techniques and representations could transpile to existing formats such as [PDDL](https://en.wikipedia.org/wiki/PDDL). Transpiling to PDDL could occur from [source code](https://en.wikipedia.org/wiki/Source_code), [abstract-syntax trees](https://en.wikipedia.org/wiki/Abstract_syntax_tree), [.NET](https://en.wikipedia.org/wiki/.NET_Framework) [assemblies](https://en.wikipedia.org/wiki/Assembly_(CLI)), or [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) [JARs](https://en.wikipedia.org/wiki/JAR_(file_format)).

Advantages of representing planning domains and problems in either assemblies or JARs include that multiple languages could be compiled to these formats while PDDL would be one key language to be decompiled from these. Of these many new languages which could be compiled to a .NET assembly or Java JAR, one is sketched below, inspired by C#.

### Types

One key ingredient of modeled planning domains is types. While, in C#, types have methods and properties, for modeling planning domains in a C#-inspired syntax, we might consider that types could have [_invariants_](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science).

```
public class Widget(Table t) extends Object()
{
    invariant(State state)
    {
        System.Diagnostics.Debug.Assert(state.Atop[this, t]);
    }
}
```

### Predicates

Another key ingredient of planning domains is predicates. Considering a C#-styled language, one might consider allowing modelers to define predicates using syntax resembling:

```
public predicate bool P1[Agent x, Widget y];
```

Exploring how predicates might be represented in .NET assemblies or Java JARs, one might base a syntax on the concept of [_extension members_](https://devblogs.microsoft.com/dotnet/csharp-exploring-extension-members/):

```
public predicate bool P2[this State state, Agent x, Widget y]
{
    get
    {
        return state.Get<bool>(Predicate.GetCurrentPredicate(), x, y);
    }
    set
    {
        state.Set<bool>(Predicate.GetCurrentPredicate(), value, x, y);
    }
    undefine
    {
        state.Undefine(Predicate.GetCurrentPredicate(), x, y);
    }
}
```

### Actions

Another key ingredient of planning domains is actions. Actions have preconditions and effects.

```
public action A(Agent x, Widget y) extends B(x, y)
{
    preconditions
    {
        P1[x, y];
    }

    effects
    {
        remove
        {
            P1[x, y];
        }
        add
        {
            P2[x, y];
        }
    }
}
```
