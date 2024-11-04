# Other types

## Type

Contrairement aux interfaces qui ne peuvent que décrire des objets, on peut tout faire avec les types. Historiquement, les interfaces étaient considérés comme plus performante mais ce n'est désormais plus vrai et donc au lieu d'alterner entre type et interface, le mieux est probablement de rester sur "type"

```ts
type A = string; // on peut faire des alias de type primitif
type B = {
  id: number;
  name: string;
}; // des objets
type C = [string, string]; // des tuples
type D = "DEBUG" | "INFO" | "WARN"; // des unions
```

## Interface

Comme dit précédemment, on ne peut décrire avec les interfaces que des objets.

```ts
interface A {
  name: string;
}

// les interfaces redéclarées mergent avec les précédentes

interface A {
  lastname: string;
}

// Maintenant l'interface A correspond à { name: string; lastname: string; }

interface B {
  address: {
    city: string;
  };
}

// Pour merger 2 interfaces on utilise le mot clef `extends`

interface C extends A, B {
  additionalProperties: string;
}

// Ca donne le type { name: string; lastname: string; address: { city: string }; additionalProperties: string }
```

## Union

Un type union est à voir un peu comme un OR

```ts
type BloodSample = "ffpe" | "blood"

type Random = "abc" | number | [string, number] | (a: number, b: number) => number // Une union peut comprendre autant de type différent que l'on souhaite

type NetworkLoadingState = {
  state: "loading";
};
type NetworkFailedState = {
  state: "failed";
  code: number;
};
type NetworkSuccessState = {
  state: "success";
  response: {
    title: string;
    duration: number;
    summary: string;
  };
};

// Une union "discriminée" est une union ou chacun des membre à une propriétée commune qui permet d'identifier chaque membre de l'union, ici "state"
type NetworkState =
  | NetworkLoadingState
  | NetworkFailedState
  | NetworkSuccessState;

function logger(state: NetworkState): string {
  switch (state.state) {
    case "loading":
      return "Downloading...";
    case "failed":
      // Ici typescript comprend que state a forcement pour type NetworkFaildedState et donc la propriété "code" est safe pour être accédée
      return `Error ${state.code} downloading`;
    case "success":
      return `Downloaded ${state.response.title} - ${state.response.summary}`;
  }
}

```

## Intersection

Les intersections servent à étendre des types, comme le fait le mot "extends" pour les interfaces

```ts
type A = {
  name: string;
};

type B = A & {
  lastname: string;
};

// Donne le type { name: string; lastname: string; }

// On peut merge deux types avec la même propriété si dans les deux types la propriété à le même type. Si ce n'est pas le cas, la propriété dans le type résultant aura le type `never` qui signifie qu'une erreur s'est produite
```

## Indexed access type

On peut acceder au type d'elements de tuples, d'array ou bien d'autre type

```ts
type Car = {
  wheel: 4;
  brand: "Ferrari";
};

type Brand = Car["brand"]; // Brand a le type "Ferrari"

type UseStateReturn = [string, (a: string) => void];

type ReturnValue = UseStateReturn[0]; // string

// On peut également obtenir un tuple de toutes les valeurs d'un tuple en indexant avec `number` directement

type Fruits = ["Banana", "Apple", "Pear"];

type FruitsUnion = Fruits[number]; // "Banana" | "Apple" | "Pear"
```

## typeof

typeof est un keyword plus qu'utile, puisqu'il permet d'infer (de déduire) un type depuis une valeur

```ts
const person = {
  name: "Etienne",
  address: "Toulouse",
};

type Person = typeof person;
//   ^? {name: string; address: string}

function f() {
  return {
    x: 10,
    y: "foo",
  };
}

type FunctionType = typeof f;
//   ^? () => {x: number, y: string}
```

## keyof

keyof permet de récupérer les clés d'un objet sous forme d'union

```ts
type Person = {
  name: string;
  age: number;
  description: string;
};

type PersonKeys = keyof Person; // "name" | "age" | "description"

type PersonValues = Person[PersonKeys]; // string | number | string

// tu peux combiner avec typeof pour récupérer les clés d'un objet

const person = {
  name: "Etienne",
  address: "Toulouse",
};

type PersonKeys = keyof typeof person; // "name" | "address"
```

## Template literals

Ca permet de créer des types à partir de string et ça s'avère très puissant

```ts
type Classes = "primary" | "secondary" | "tertiary";

type ButtonClass = `${Classes}-button`; // "primary-button" | "secondary-button" | "tertiary-button"

type ButtonClass2 = `${Classes}-button-${"small" | "medium" | "large"}`; // "primary-button-small" | "primary-button-medium" | "primary-button-large" | "secondary-button-small" | "secondary-button-medium" | "secondary-button-large" | "tertiary-button-small" | "tertiary-button-medium" | "tertiary-button-large"
```

## as const

as const permet de dire à typescript que la valeur ne doit pas être étendue

```ts
const person = {
  name: "Etienne",
  address: "Toulouse",
};

type Person = typeof person; // {name: string; address: string}

const person2 = {
  name: "Etienne",
  address: "Toulouse",
} as const;

type Person2 = typeof person2; // {readonly name: "Etienne"; readonly address: "Toulouse"}
```

## satisfies

satisfies permet de vérifier si un type satisfait une condition

Le problème est quand tu fais ça :

```ts
type Person = {
  name: string;
  age: number;
};

const person: Person = {
  name: "Etienne",
  age: 32,
};

// Si tu hover sur `person`, typescript va te dire que c'est de type Person, mais si tu fais ça :

const person2: Person = {
  name: "Etienne",
  age: 32,
} satisfies Person;

// Typescript va garder le type de `person2` comme étant { name: 'Etienne', age: 32 } tout en vérifiant que cela satisfait bien le type Person
```

## Utils

### Omit/Pick

Omit permet de retirer des propriétés d'un objet

```ts
type Person = {
  name: string;
  age: number;
  address: string;
};

type PersonWithoutAge = Omit<Person, "age" | "name">; // {address: string}
```

Pick permet de ne garder que certaines propriétés d'un objet

```ts
type Person = {
  name: string;
  age: number;
  address: string;
};

type PersonName = Pick<Person, "name">; // {name: string}
```

### Exclude/Extract

Exclude permet de retirer des types d'une union

```ts
type A = "a" | "b" | "c";

type B = Exclude<A, "a" | "b">; // "c"
```

Extract permet de ne garder que certains types d'une union

```ts
type A = "a" | "b" | "c";

type B = Extract<A, "a" | "b">; // "a" | "b"
```

### Partial/Required

Partial permet de rendre toutes les propriétés d'un objet optionnel

```ts
type Person = {
  name: string;
  age: number;
};

type PersonPartial = Partial<Person>; // {name?: string; age?: number}
```

Required permet de rendre toutes les propriétés d'un objet obligatoire

```ts
type Person = {
  name?: string;
  age?: number;
};

type PersonRequired = Required<Person>; // {name: string; age: number}
```

### Readonly

Readonly permet de rendre un objet immuable

```ts
type Person = {
  name: string;
  age: number;
  description: string;
};

type PersonReadonly = Readonly<Person>; // {readonly name: string; readonly age: number}
```

### Record

Record permet de créer un objet à partir d'une union

```ts
type A = "a" | "b" | "c";

type B = Record<A, number>; // {a: number; b: number; c: number}
```

### Awaited

Awaited permet de récupérer le type d'une promesse

```ts
type A = Promise<string>;

type B = Awaited<A>; // string
```

### ReturnType

ReturnType permet de récupérer le type de retour d'une fonction

```ts
function f() {
  return {
    x: 10,
    y: "foo",
  };
}

type FunctionType = ReturnType<typeof f>; // {x: number, y: string}
```

### Parameters

Parameters permet de récupérer les types des paramètres d'une fonction

```ts
type A = (a: number, b: string) => void;

type B = Parameters<A>; // [number, string]
```
