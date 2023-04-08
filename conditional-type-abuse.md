I often see conditional types being (ab)used to change the type of a parameter depending on another parameter, maybe like this:

```ts
function conditional<Bool extends boolean>(bool: Bool, arg: Bool extends true ? string : number): number {
    if (bool) return Number(arg);

    return arg;
}
```

The problem with this approach is that `arg` is never narrowed correctly based on the narrowing of `bool`. In this example, on the second return statement, we see the following error message:

> ```
> Type 'string | number' is not assignable to type 'number'.
>     Type 'string' is not assignable to type 'number'.
> ```

This is due to the fact that the compiler cannot relate generic type parameters with control flow, as described in [#33912](https://github.com/microsoft/TypeScript/issues/33912). One of the described workarounds would be to use an assertion using the [`as` keyword](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions):

```ts
function conditional<Bool extends boolean>(bool: Bool, arg: Bool extends true ? string : number): number {
    if (bool) return Number(arg);

    return arg as number;
}
```

However, most of the times, this is not necessary if you use a clever refactoring. In this case, using discriminated unions would allow the compiler to narrow the type of `arg` when narrowing `bool`.
