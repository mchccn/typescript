## `boolean` is a union

This is rather a little known fact and common "gotcha" I'd like to share, rather than a full-length text. Let's just start with an example of a innocent-looking type which wraps a given boolean value:

```ts
type Wrap<T> = T extends boolean ? { bool: T } : never;
```

It seems to work when passing `true` and `false`:

```ts
type T = Wrap<true>;  // { bool: true }
type F = Wrap<false>; // { bool: false }
```

And also when we pass things that aren't booleans, like strings:

```ts
type X = Wrap<"true">; // never
```

All is well until we decide to pass `boolean`:

```ts
type B = Wrap<boolean>; // { bool: false } | { bool: true }
```

Which reveals the true nature of `boolean`; it's actually a union type under the hood.
Yes, internally, the `boolean` type is represented as `true | false`.
Essentially, it's shorthand (syntactical sugar, even) for the type `true | false`.

If you don't understand what just happened, see [distributive conditional types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types) in the official documentation. That's all for this note, for now.