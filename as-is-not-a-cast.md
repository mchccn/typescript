# `as` is not a cast

Please, do not use the word "cast" to describe the `as` keyword!
That's far from what it actually does.
Casting is usually used to describe the coercion or mapping from one type to another.
Here is an example of casting in Java:

```java
double d = 1.23d;
int i = (int) d; // warning: lossy coercion!
```

We're simply casting a value of type `double` to type `int`, using `(int)` in front of the value. 
At *runtime*, this really does convert the value of `1.23d` to an int (`1`), but in TypeScript, this is not the case.
Given the following, similar code;

```ts
const str = "1.23";
const num = str as number;

console.log(num);
```

The result is actually `"1.23"`.
TypeScript does not (and will not — please refer to the [non-goals](https://github.com/Microsoft/TypeScript/wiki/TypeScript-Design-Goals#non-goals)) add runtime type casting.
The key here is in the name!
It's called an ***as***sertion.
Still doubting?
Refer to the [official documentation](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions) on the subject.

Use `as` sparingly, as it is basically lying to the compiler.
Only assert things if you are 100% sure.
This goes the same for `!` (non-null assertion), which should be used in the case where you just want to remove `null`/`undefined` from the type:

```ts
const a = null!; // never
const b = localStorage.getItem("key"); // string | null
const c = localStorage.getItem("key")!; // string
```

Originally, the syntax for assertions was different.
However, since it was so similar to casting in other languages, people started to confuse it for casting at *runtime*!
That is one of the reasons why we have this new `as` keyword to describe assertions.
See for yourself:

```ts
const str = "1.23";
const num = <number> d;
```

Chilling!
