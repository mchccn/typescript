## Conditional type abuse

I often see conditional types being (ab)used to change the type of a parameter depending on another parameter, maybe like this:

```ts
function conditional<Bool extends boolean>(bool: Bool, arg: Bool extends true ? string : number): number {
    if (bool) return Number(arg);

    return arg;
}
```

The problem with this approach is that `arg` is never narrowed correctly based on the narrowing of `bool`.
In this example, on the second return statement, we see the following error message:

> ```
> Type 'string | number' is not assignable to type 'number'.
>     Type 'string' is not assignable to type 'number'.
> ```

This is due to the fact that the compiler cannot relate generic type parameters with control flow, as described in [#33912](https://github.com/microsoft/TypeScript/issues/33912).
One of the described workarounds would be to use an assertion using the [`as` keyword](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions):

```ts
function conditional<Bool extends boolean>(bool: Bool, arg: Bool extends true ? string : number): number {
    if (bool) return Number(arg);

    return arg as number;
}
```

However, most of the times, this is not necessary if you use a clever refactoring.
In this case, using discriminated unions would allow the compiler to narrow the type of `arg` when narrowing `bool`.

```ts
function conditional(...[bool, arg]: [true, string] | [false, number]): number {
    if (bool) return Number(arg);

    return arg;
}
```

Now, when `bool` is narrowed to a truthy value (in this case, just `true`), `arg`'s type is inferred to be `string`.
Likewise, in the second return statement, `arg`'s type is now `number`.
TypeScript is also smart enough to show this union of tuples as overloads, but admittedly, the parameter names are not very helpful.
However, it's a simple fix; just label the tuples:

```ts
function conditional(...[bool, arg]: [bool: true, arg: string] | [bool: false, arg: number]): number {
    if (bool) return Number(arg);

    return arg;
}
```

Using this discriminated union, you cannot call this function with something of type `boolean`.
Only the types `true` or `false` are allowed. 
No problem, you say, just add another member to the union to handle this case:

```ts
function conditional(...[bool, arg]: [bool: true, arg: string] | [bool: false, arg: number] | [bool: boolean, arg: string | number]): number {
    if (bool) return Number(arg);

    return arg;
} //       ~~~ Type 'string | number' is not assignable to type 'number'.
```

Uh oh, we're back to square one again.
In the case where you still need to support calls with union types (`boolean` is just shorthand for `true | false`), or if the return type also depends on the arguments given, you should consider using overloads:

```ts
function conditional(bool: true, arg: string): number;
function conditional(bool: false, arg: number): number;
function conditional(bool: boolean, arg: string | number): number;
function conditional(bool: boolean, arg: string | number) {
    if (bool) return Number(arg);

    return arg;
}
```

It is important that the overload with `boolean` is last, since the first overload that matches a call will be used.
Therefore, it is better to order your overloads from most specific to least specific.
One important caveat of using overloads is that TypeScript no longer checks if the implementation strictly matches the available call signatures.
For example, you can flip the `bool` inside and TypeScript won't complain:

```ts
function conditional(bool: true, arg: string): number;
function conditional(bool: false, arg: number): number;
function conditional(bool: boolean, arg: string | number): number;
function conditional(bool: boolean, arg: string | number) {
    if (!bool) return Number(arg);
    //  ^
    return arg;
}
```

This concludes my note on conditional type abuse.
Hopefully, you now know when conditional types are a bad fit, and what you can do to achieve the desired behavior.
