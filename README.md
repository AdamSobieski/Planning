## Modeling Planning Domains

New techniques for modeling planning domains and problems can be explored such that these new techniques and representations transpile to existing formats such as [PDDL](https://en.wikipedia.org/wiki/PDDL). One could transpile to PDDL from [source code](https://en.wikipedia.org/wiki/Source_code), [abstract-syntax trees](https://en.wikipedia.org/wiki/Abstract_syntax_tree), [.NET](https://en.wikipedia.org/wiki/.NET_Framework) [assemblies](https://en.wikipedia.org/wiki/Assembly_(CLI)), or [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) [JARs](https://en.wikipedia.org/wiki/JAR_(file_format)).

Advantages of representing planning domains and problems in .NET assemblies or Java JARs include that multiple new languages could be compiled to these formats while PDDL would be one key language to be decompiled from these.

How should planning domain and problem definitions be represented in .NET assemblies and/or Java JARs?

### Types

A key ingredient of modeled planning domains are types.Types could have [_invariants_](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science).

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

Another key ingredient of planning domains are predicates. Considering a C#-styled language, one might consider allowing modelers to define predicates using syntax resembling:

```
public predicate bool P1[Agent x, Widget y];
```

The concept here, regardless of syntax, is that predicates can be considered as being extension members of a provided `State` type.

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

Similarly, numerical fluents can be defined by replacing `bool` with a numerical type, e.g., `float`.

```
public predicate float P3[Agent x, Widget y];
```

### Actions

Another key ingredient of planning domains are actions. Actions can have preconditions and effects.

```
public action A(this State state, Agent x, Widget y) extends B(state, x, y)
{
    preconditions
    {
        System.Diagnostics.Debug.Assert(state.P1[x, y]);
    }

    effects
    {
        state.P1[x, y] = null;
        state.P2[x, y] = true;
    }
}
```
