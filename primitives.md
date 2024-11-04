# Primitives

## bool

true ou false

## number

```ts
let decimal: number = 6;
let float: number = 3.14;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

## string

N'importe quelle chaîne de caractères

## array

Deux forme de déclaration d'un tableau

```ts
const arrayNumber: number[] = [1, 2, 3];
const arrayNumber: Array<number> = [1, 2, 3];
```

On préfèrera utiliser la version générique (Array<any>) car plus claire

## tuple

```ts
// Ici, on s'assure que notre variable tuple aura la forme [nubmer, number, string]
const tuple: [number, number, string] = [1, 2, "trois"];

// Ici on déduit (infer) le type de notre tuple
const tuple = [1, 2, "trois"] as const;

// Sans le `as const`, le type de tuple serait Array<number | string>
```

## enum

Les enums existent mais on va éviter de s'en servir

## unknown et any

Ils sont l'opposé l'un de l'autre.

`unknown` représente quelque chose que l'on ne connait pas. Typescript ne nous laissera rien faire dessus tant que l'on a pas déterminer plus précisément du type dont il s'agit
`any` représent n'importe qu'elle data et typescript nous laissera faire ce que l'on veut dessus. Any est donc bien trop laxiste et le but est d'en avoir le moins possible

## object

Type un peu trop large car en javascript beaucoup de choses sont considérés comme des objects en javascript

```ts
// Objet classique
const user: object = { name: "Alice", age: 25 };

// Fonction
const func: object = () => console.log("Hello");

// Tableau
const numbers: object = [1, 2, 3, 4, 5];

// Classe et instance de classe
class Car {
  constructor(public model: string) {}
}
const myCar: object = new Car("Tesla");

// Types spéciaux
const date: object = new Date();
const regex: object = /abc/;
const map: object = new Map();
const set: object = new Set();
```

Pour définir des objet, on préfera les définir plus précisément si on connait leur structure

```ts
const user: { name: string; age: number } = { name: "Alice", age: 25 };
```

ou bien plus génériquement

```ts
let user: Record<string, string | number>;
```
