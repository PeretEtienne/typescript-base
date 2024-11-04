# Generics

Les génériques en typescript permettent de créer des composants réutilisable en fonctino du type de donnée qu'on leur passe.

```ts
type Pair<T> = {
  first: T;
  second: T;
};

const pair: Pair<number> = {
  first: 1,
  second: 2,
};

const pair2: Pair<string> = {
  first: "hello",
  second: "world",
};
```

Dans cet exemple, `Pair` est un type générique qui prend un type `T` en paramètre. On peut ensuite utiliser ce type en lui passant un type spécifique, comme `number` ou `string`.

Les génériques peuvent aussi être utilisés dans les fonctions:

```ts
function identity<T>(arg: T): T {
  return arg;
}

const value = identity(42); // value is number

function pair<TFirt, TSecond>(
  first: TFirst,
  second: TSecond,
): [TFrist, TSecond] {
  return [first, second];
}

const p = pair(1, "hello"); // p is [number, string]
```

Dans cet exemple, `identity` est une fonction générique qui prend un argument de type `T` et retourne un argument de type `T`. `pair` est une fonction générique qui prend deux arguments de types différents et retourne un tableau contenant ces deux arguments.

Les génériques peuvent aussi être utilisés avec des contraintes:

```ts
type Pair<T extends { length: number }> = {
  first: T;
  second: T;
};

const pair: Pair<string> = {
  first: "hello",
  second: "world",
};

const pair2: Pair<number> = {
  // Error: number does not have a length property
  first: 1,
  second: 2,
};
```

Dans cet exemple, `Pair` est un type générique qui prend un type `T` qui doit avoir une propriété `length`. Cela permet de s'assurer que les types passés à `Pair` ont une propriété `length`.
