---
id: version-9.0.0-rc.5-schema-union-type
title: Union Type
original_id: schema-union-type
---

The union type represents a set of object types. This means that by using a union as a field type we can say that the returned object will be one of the object types specified in the union types set.

The union is represented like the following in the GraphQL SDL syntax.

```GraphQL
union FooBarBaz = Foo | Bar | Baz
```

In contrast to an interface the types do not have to share a set of fields. In order to query fields on a union we always need to use a fragment.

```GraphQL
query {
    field {
        ... on Foo
        {
            foo
        }
        ... on Bar
        {
            bar
        }
        ... on Baz
        {
            baz
        }
    }
}
```

In order to specify a union type we can use the GraphQL SDL syntax or we can use the union schema type class. Since there is no union type in C# we cannot infer a union type from a POCO type.

```csharp
public class FooBarBaz
    : UnionType
{
    protected override void Configure(IUnionTypeDescriptor descriptor)
    {
        descriptor.Name("FooBarBaz");
        descriptor.Type<FooType>();
        descriptor.Type<BarType>();
        descriptor.Type<BazType>();
    }
}
```

In order to make this more convenient and infer if an object type belongs to a union type set we backed in support for marker interfaces.

```csharp
public interface IFooBarBaz { }

public class FooBarBaz
    : UnionType<IFooBarBaz>
{
}
```

In this case the set is inferred from the types in the schema.
