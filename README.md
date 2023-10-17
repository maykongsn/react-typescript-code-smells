# React e TypeScript Code Smells

> Identificar e analisar os _code smells_ mais comuns no desenvolvimento com React e TypeScript

---

## Sumário

[Code smells](#code-smells) <br>
  - [TypeScript smells](#typescript-smells)
    - [Any Type](#any-type)
    - [Many Non-Null Assertions](#many-non-null-assertions)
    - [Missing Union Type Abstraction](#missing-union-type-abstraction)
    - [Enum Implicit Values](#enum-implicit-values)
  - [React e TypeScript](#react-e-typescript)
    - [Multiple Booleans for State](#multiple-booleans-for-state)
    - [Children Props Pitfall](#children-props-pitfall)
---

## Code Smells

### TypeScript smells

#### Any Type
O TypeScript permite a verificação e inferência de tipos para variáveis, objetos, funções,
etc. Ao definir o tipo any para algum dos elementos do código, o desenvolvedor está desativando a checagem de tipos, possibilitando a manipulação dessas entidades sem qualquer verificação. Por exemplo, considere o código abaixo, onde um estado tem o seu tipo definido como ``any``. Isso é problemático porque anula os benefícios que o TypeScript oferece em termos de segurança de tipos e detecção de erros em tempo de compilação. Consequentemente, sem a verificação, é possível que erros em tempo de execução aconteçam.

```tsx
import React, { useState } from 'react';

const MyComponent = () => {
  const [data, setData] = useState<any>(null);

  // ...
};

export default MyComponent;
```

#### Many Non-Null Assertions
É comum que os desenvolvedores utilizem o operador não-nulo para informar que uma propriedade não será ``null`` ou ``undefined`` em momentos que a verificação de tipos não consegue deduzir isso. No exemplo a seguir, a interface ``DataProps`` infere que ``data`` pode ser do tipo ``null``. Por esse motivo, o desenvolvedor utiliza o operador não-nulo, para garantir ao TypeScript que ``data`` não será ``null``. Por consequência, erros em tempo de execução podem ocorrer pela falta de checagem de tipo e a possibilidade de receber ``null``.

```tsx
function Component({ data }: { data: { prop1: string; prop2: number } | null }) {
  return(
      <div>
        <p>{data!.prop1}</p>
        <p>{data!.prop2}</p>
      </div>
  )
}
```

#### Missing Union Type Abstraction
Os ``type aliases`` do TypeScript são similares às interfaces, no entanto, eles aceitam ``union types``, algo que as interfaces não permitem. Além disso, facilitam a manutenção, reusabilidade e legibilidade do código. O exemplo a seguir demonstra um componente em que as ``props`` são do tipo ``string``, ``number`` ou ``boolean``. Nesse caso, é recomendável utilizar ``type aliases`` para evitar a repetição de ``union types``.

```tsx
function Component({ prop }: {
  prop: string | number | boolean;
}) {
  return (
    // ...
  );
}
```

#### Enum Implicit Values
Os ``enums`` são uma forma de definir constantes e facilitar a manutenção e legibilidade de um código. No entanto, no TypeScript, é comum que os desenvolvedores usem ``enums`` sem definir um valor explícitos para eles. Sendo assim, o TypeScript define valores numéricos para eles seguindo a ordem. Neste exemplo, as constantes ``Pending``, ``Processing`` e ``Completed`` estão sem valores explícitos. Com isso, caso o desenvolvedor adicione uma nova constante como ``Failed``, poderá alterar a ordem e consequentemente os valores numéricos do ``enum``.

```tsx
enum PaymentStatus {
  Pending,
  Processing,
  Completed,
}
```

#### Multiple Booleans for State
É comum que os desenvolvedores usem muitos ``states`` do tipo ``boolean`` para definir o estado do componente. Isso aumenta a complexidade e torna o código difícil de manter à medida que o número de ``states`` e tipagem aumenta. No exemplo mostrado, o componente possui alguns ``useState`` do tipo ``boolean``, que podem ser substituídos por alternativas para a melhor tipagem.

```tsx
function Component() {
    const [isActive, setIsActive] = useState(false);
    const [isLoading, setIsLoading] = useState(false);
    const [isVisible, setIsVisible] = useState(false);
    // ...
}
```

Para refatorar esse smell, podemos criar um objeto de estado que representa todos os possíveis estados do componente. Cada estado será uma classe que encapsula o comportamento associado a esse estado.

```tsx
const [state, setState] = useState({
  isLoading: false,
  isError: false,
  isModalOpen: false,
  // ...
});
```

#### Children Props Pitfall
Se o componente utiliza children props, os desenvolvedores devem optar pela tipagem correta, para conseguir passar os elementos JSX. Então, os desenvolvedores devem utilizar os tipos providos por ```@types/react``` ou algum outro tipo primitivo, funções, etc. Por isso, não é uma boa prática definir ```any``` ou tipos que podem restringir os elementos JSX e que afetam a legibilidade como: ```undefined```, ```null```, ```unknown``` e ```never```.

```tsx
type ComponentProps = {
  children: undefined;
};

interface AnotherComponentProps {
  children: never;
}
```

Para corrigir esse smell, podemos definir o tipo ```ReactNode``` para o children.

```tsx
import { ReactNode } from "react";

type ComponentProps = {
  children: ReactNode;
};

interface AnotherComponentProps {
  children: ReactNode;
}
```