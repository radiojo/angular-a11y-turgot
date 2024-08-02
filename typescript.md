# TypeScript (TS)

Points clés :
- Surcouche du JavaScript apportant le support des types
- Le JavaScript est un langage dynamique : on peut accéder à une propriété d'un objet qui n'existe pas sans erreur par exemple, le TypeScript permet d'avoir un code plus sécurisé et maintenable
- Les erreurs sont détectées avec que le code soit exécuté (_static type checking_)
- Du JavaScript est du TypeScript
- Le TypeScript est **transpilé** (transformation d'un langage en un autre) en JavaScript

## Rappels

### Types de variables

#### Les variables avec `let`

Le mot-clé `let` permet de créer une variable mutable, qui peut changer de valeur.

```ts
let variable = 'quelque chose';
```

#### Les constantes avec `const`

Le mot-clé `const` permet de créer une constante, qui ne peut pas changer de valeur une fois définie.

```ts
const variable = 'quelque chose';
variable = 'autre chose'; // Cannot assign to variable because it is a constant.
```

Par contre, les attributs d'un objet stocké dans une constante peuvent être modifiés.

```ts
const variable = {
  unAttribut: 'quelque chose'
};

variable.unAttribut = 'autre chose';
```

## Types primitifs

### Chaînes de caractères

Type `string`

```ts
let maChaine: string = 'Les produits laitiers sont nos amis pour la vie';
```

### Nombres 

Type `number`

```ts
let monNombre: number = 56;
let monAutreNombre: number = 3.56;
```

### Booléens

Type `boolean`

```ts
let monBooleen: boolean = false;
```

### Les tableaux

Type `<type>[]` ou `Array<type>` (`string[]` ou `Array<string>` par exemple)

```ts
let tableauDeChaines: string[] = ['Les', 'produits', 'laitiers'];
let tableauDeNombres: number[] = [1, 2, 3];
let tableauDeBooleens: boolean[] = [true, false, true];
```

### Le type du mal, `any`

Si je veux que ma variable ou ma constante puisse contenir n'importe quel type, je peux utiliser `any`.
Ainsi je peux écrire ce code sans aucune erreur : 

```ts
let monNombre: any = 56;
monNombre.maFonction();
```

- Je peux donc exécuter une fonction sur ma variable `monNombre` qui n'en contient pas.
- Le code échouera à l'exécution mais pas à la transpilation.
- `any` dit au compilateur de ne pas effectuer de vérifications particulières pour la variable

### Le type `undefined`

`undefined` est la valeur d'une variable non initialisée.

```ts
let monNombre: number;
console.log(monNombre); // Error: Variable 'z' is used before being assigned
```

### Le type `null`

`null` signifie "sans valeur".

```ts
let monNombre: number | null = null;
console.log(monNombre); // null
```

### Le type `unknown`

`unknown` signifie que l'on connaît pas le type. `unknown` est différent de `any` puisque TypeScript se chergera de faire des
vérifications.

```ts
function uneFonctionAvecAny(uneVariableAny: any) {
    uneVariableAvecAny.methode(); // OK
}

function uneFonctionAvecUnknown(uneVariableUnknown: unknown) {
    uneVariableUnknown.methode(); // 'uneVariableUnknown' is of type 'unknown'.
}
```

Dans la fonction `uneFonctionAvecUnknown`, je ne peux pas appeler la fonction `methode()` de la variable `uneVariableUnknown` qui est de type `unknown`. 

### Le type `void`

Utilisé pour indiquer qu'une fonction ne retourne pas de valeur.

```ts
function uneFonction() {
   return;
}

function uneFonction() {
    quelqueChose();
}
```

### Le type `never`

```ts
function lanceUneErreur(message: string): never {
  throw new Error(msg);
}
```

La fonction `lanceUneErreur` jette une exception, la fonction est donc arrêtée et donc ne retourne rien.

## Inférence de types

Capacité de TypeScript à déduire le type d'une variable grâce à sa valeur.

```ts
let maVariable = 78; // number
let monAutreVariable = "Les produits laitiers sont nos amis pour la vie" // string
let encoreUneAutreVariable = false // boolean
```

## Les unions et intersections

### Les unions

Permet de créer des variables ou des fonctions qui contiennent ou retournent un type parmi un ensemble de types (représente donc un **ou**).

```ts
let uneVariable: number | string;
uneVariable = 67;         // OK
uneVariable = "Salut !";    // OK
```

- `maVariable` peut contenir un nombre ou une chaîne de caractères.
- Pour créer une intersection, on utilise l'opérateur | (_pipe_)

### Les intersections

```ts
type MonIntersection = { unePropriete: string } & { uneAutrePropriete: string };

let maVariable: MonIntersection = {
    unePropriete: 'Du texte',
    uneAutrePropriete: 'Encore du texte'
}
```

## L'assertion de type

Le mot-clé `as` permet de faire de l'assertion de type, de dire au compilateur TypeScript qu'une valeur doit être considérée comme étant d'un type spécifique.

```ts
let uneVariable: any = "this is a string";
let longueurDeLaChaine: number = (uneVariable as string).length;
```

- La variable `uneVariable` est de type `any`, elle peut donc contenir n'importe quel type de valeur
- La variable `longueurDeLaChaine` est un nombre contenant la longueur de la chaîne stockée dans la variable `uneVariable`
- L'attribut `length` existant seulement sur le type `string`, on utilise `as` pour dire au compilateur que la variable `uneVariable` doit être considérée comme de type `string` permettant ainsi de pouvoir accéder à l'attribut `length`

Je ne peux pas tout faire avec as : je ne peux pas transformer une chèvre en un torchon.

```ts
const uneConstante = "Salut !" as number; // Conversion of type 'string' to type 'number' may be a mistake because neither type sufficiently overlaps with the other. If this was intentional, convert the expression to 'unknown' first.
```

Une chaîne de caractères ne pouvant être considérée comme un nombre, je ne peux pas utiliser `as` dans ce cas.  

Je ne peux utiliser as que dans le cas de types qui se superposent, qui partagent certains attributs en commun.

```ts
type UnType = { unAttribut: string };
type UnAutreType = { unAutreAttribut: string };

const uneConstante = UnType as UnAutreType
```

## Types littéraux

```ts
let variable: 'Salut'
```

## Interfaces

Une interface est un contrat que doit respecter un objet.

```ts
interface Chien {
  nom: string;
  age: number;
}

let monChien: Chien = {
   nom: 'Scooby-Doo',
   age: 34
}
```

### Héritage des interfaces

Une interface peut hériter de d'autres interfaces.

```ts
interface Mammifere {
  race: string;
}

interface Chien extends Mammifere {
    nom: string;
    age: number;
}

let monChien: Chien = {
    nom: 'Scooby-Doo',
    age: 34,
    race: 'Canidé'
}
```

- L'interface `Mammifere` a un attribut `race`. 
- L'interface `Chien` étend l'interface `Mammifere`, reprenant donc ses attributs
- Un objet de type `Chien` doit donc implémenter tous les attributs, ceux de l'interface `Chien` et `Mammifere`

## Classes

### Héritage des classes

## Énumérations