## Introduction

New techniques for modeling planning domain and problem definitions can be explored such that these new techniques and representations transpile to [PDDL](https://en.wikipedia.org/wiki/PDDL). One could transpile to PDDL from [source code](https://en.wikipedia.org/wiki/Source_code), [abstract-syntax trees](https://en.wikipedia.org/wiki/Abstract_syntax_tree), [.NET](https://en.wikipedia.org/wiki/.NET_Framework) [assemblies](https://en.wikipedia.org/wiki/Assembly_(CLI)), and/or [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) [JARs](https://en.wikipedia.org/wiki/JAR_(file_format)).

Advantages of representing planning domains and problems in .NET assemblies and/or Java JARs include that many new languages could be compiled to these formats. PDDL would be one key language to be decompiled from these formats.

How should planning domain and problem definitions be represented in .NET assemblies and/or Java JARs?

## Modeling Planning Domains

### Types

A key ingredient of modeled planning domains are types. Types could define [_invariants_](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science).

```
public class Widget(Table table) : Object(), MultipleInheritance()
{
    public void Invariant(State state)
    {
        Object.Invariant(state);
        MultipleInheritance.Invariant(state);

        System.Diagnostics.Debug.Assert(state.Atop[this, table]);
    }
}
```

### Predicates

Another key ingredient of planning domains are predicates. Considering a C#-styled language, one might allow modelers to define predicates using syntax resembling:

```
public predicate bool P1[Agent x, Widget y];
```

A concept here, regardless of syntax, is that predicates can be considered as being extension members of a provided `State` type.

```
public static class Extension
{
    extension(State state)
    {
        public bool? P2[Agent x, Widget y]
        {
            get
            {
                return state.Get<bool>(Predicate.GetCurrentPredicate(), x, y);
            }
            set
            {
                if(value.HasValue)
                {
                    state.Set<bool>(Predicate.GetCurrentPredicate(), value.Value, x, y);
                }
                else
                {
                    state.Undefine(Predicate.GetCurrentPredicate(), x, y);
                }
            }
        }
    }
}
```

### Numerical Fluents

Similarly, numerical fluents can be defined by replacing `bool` with a numerical type, e.g., `int` or `float`.

```
public predicate int P3[Agent x, Widget y];
```
```
public predicate float P4[Agent x, Widget y];
```

### Object Fluents

Similarly, object fluents can be defined by replacing `bool` with an object type.

```
public predicate object P5[Agent x, Widget y];
```
```
public predicate Gizmo P6[Agent x, Widget y];
```

### Actions

Another key ingredient of planning domains are actions. Actions have preconditions and effects.

```
public action A(Agent x, Widget y) : B(x, y), C(x, y)
{
    public void Preconditions(State state)
    {
        B(x, y).Preconditions(state);
        C(x, y).Preconditions(state);

        System.Diagnostics.Debug.Assert(state.P1[x, y]);
    }

    public void Effects(State state)
    {
        B(x, y).Effects(state);
        C(x, y).Effects(state);

        state.P1[x, y] = null;
        state.P2[x, y] = true;
    }
}
```
