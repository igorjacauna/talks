---
highlighter: shiki
drawings:
  persist: false
css: unocss
layout: image
image: assets/images/code-this-not-that.png
---

# Antipatterns React

Padrões para não seguir

---
layout: cover
background: https://media.giphy.com/media/kaZdqgKC2Dg0XoRzgD/giphy.gif
---

# Componentes grandes!

---

<Donts>
  <template #dont>

```tsx
const MyBigComponent = () => {
  return (
    <div>
      <nav>
        <ul>
          <li>
            <div>
              <icon name="cool-icon" />
              <span>Home</span>
            </div>
          </li>
          <li>
            <div>
              <icon name="cool-icon" />
              <span>Home</span>
            </div>
          </li>
          <li>
            <div>
              <icon name="cool-icon" />
              <span>Home</span>
            </div>
          </li>
          <li>
            <div>
              <icon name="cool-icon" />
              <span>Home</span>
            </div>
          </li>
          <li>
            <div>
              <icon name="cool-icon" />
              <span>Home</span>
            </div>
          </li>
          <li>
            <div>
              <span>Home</span>
            </div>
          </li>
        </ul>
      </nav>
    </div>
  )
}
```

  </template>
  <template #doit>

```tsx{none|all|4-6|all}
  const MyBigComponent = () => {
    return (
      <div>
        <NavBar />
        <SectionOne />
        <SectionTwo />
      </div>
    )
  }
```

  </template>
</Donts>

---
layout: cover
background: https://media.giphy.com/media/X8HbeXDF7nzaM/giphy.gif
---

# Aninhamento

---

<Donts>
  <template #dont>

```tsx{all|1,6-8|all}
function Parent () {
  const [count, setCount] = useState(0);

  const handleClick = () => setCount(previous => previous + 1);

  const Child = () => {
    return <button onClick={handleClick}>+</button>
  }

  return (
    <div>
      <Child />
    </div>
  )
}
```

  </template>
  <template #doit>
    
```tsx{none|all|0-4,5,12|all}
const Child = ({ onClick }) => {
  return <button onClick={onClick}>+</button>
}

function Parent () {
  const [count, setCount] = useState(0);

  const handleClick = () => setCount(previous => previous + 1);

  return (
    <div>
      <Child onClick={handleClick} />
    </div>
  )
}
```

  </template>
</Donts>

---
layout: cover
background: https://tenor.com/pt-BR/view/tired-office-sleepy-exhausted-gif-19486265.gif
---

# Muito trabalho

---

<Donts>
<template #dont>

```tsx{all|5|all}
function WorkHard() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(0);

  const total = expensiveCalculation(count);

  return (
    <div>
      Count: {count}
      Total: {total}
      <button onClick={() => setOther(o => o + 1)}>
        Change other
      </button>
    </div>
  )
}
```

</template>
<template #doit>

```tsx{none|all|5-8|all}
function WorkHard() {
  const [count, setCount] = useState(0);
  const [other, setOther] = useState(0);

  const total = useMemo(
    () => expensiveCalculation(count),
    [count]
  );

  return (
    <div>
      Count: {count}
      Total: {total}
      <button onClick={() => setOther(o => o + 1)}>
        Change other
      </button>
    </div>
  )
}
```

</template>
</Donts>

---
layout: cover
background: https://tenor.com/pt-BR/view/locking-door-fail-sliding-door-useless-pointless-meaningless-gif-13573515.gif
---

# DIV desnecessária

---

<Donts>
<template #dont>

```tsx{all|2-5|all}
function Frag() {
  return (
    <nav></nav>
    <article></article>
  )
}
```

```tsx{none|all|3,6|all}
function Frag() {
  return (
    <div>
      <nav></nav>
      <article></article>
    </div>
  )
}
```

</template>
<template #doit>

```tsx{none|all|3,6|all}
function Frag() {
  return (
    <>
      <nav></nav>
      <article></article>
    </>
  )
}
```

</template>
</Donts>

<!--
Sem os Fragments podem haver problemas de acessibilidade com muitas divs aninhadas
E complica CSS
 -->

---
layout: cover
background: https://tenor.com/pt-BR/view/work-files-filing-cabinet-file-it-in-the-back-gif-10490790.gif
---

# Organização

---

<Donts>
<template #dont>

##### ComponenteA.tsx

```tsx
export function NavBar() { ... }
export function Icon() { ... }
```

<v-click>

### Estrutura

```
├── NavBar.ts
├── NavBar.style.tsx
├── index.ts
├── Icon.tsx
├── Icon.style.ts
```

</v-click>

</template>

<template #doit>

<v-click>

##### NavBar.tsx

```tsx
export default function NavBar() { ... }
```

##### Icon.tsx

```tsx
export default function Icon() { ... }
```

</v-click>

<v-click>

#### Estrutura

```
├── NavBar
|   ├── index.ts
│   ├── NavBar.tsx
├── Icon
|   ├── index.ts
│   ├── Icon.tsx
│   └── Icon.style.ts
```

</v-click>

</template>
</Donts>

---

# Exportação

```
├── NavBar
|   ├── index.ts
│   ├── NavBar.tsx
├── Icon
|   ├── index.ts
│   ├── Icon.tsx
│   └── Icon.style.ts
```

<v-click>
<Donts>
<template #dont>

##### index.tsx

```ts
export default function NavBar() { ... }
```

</template>

<template #doit>

##### index.ts

```ts
export { default } from './NavBar';
```

##### NavBar.tsx

```ts
export default function NavBar() { ... }

```

</template>
</Donts>
</v-click>

<v-click>

```ts
import NavBar from './NavBar'; // 👍🏻

import NavBar from './NavBar/NavBar'; // 💩
```

</v-click>

<!-- 
É importante não ter o componente definido em index.ts senão, quando houver múltiplos
arquivos abertos não vai ser fácil saber quem é quem ou as importações vão acabar tendo
'index' no final, se for preciso exportar outra coisa além do componente
 -->

---
layout: cover
background: https://tenor.com/pt-BR/view/teamwork-red-men-red-men-redskins-gif-19844561.gif
---

# Prop Drilling

---

<Donts>
<template #dont>

```tsx{all|4|8|12|all}
function PropDrilling() {
  const [count] = useState(0)

  return <Child count={count} />
}

function Child({ count }) {
  return <GrandChild count={count} />
}

function GrandChild({ count }) {
  return <dit>{{ count }}</div>
}
```

</template>

<template #doit>

```tsx{none|all|5-7|6,11-13,15-18|all}
function PropDrilling() {
  const [count] = useState(0)

  return (
    <CountContext.Provider value={count}>
      <Child />
    </CountContext.Provider>
  )
}

function Child() {
  return <GrandChild />
}

function GrandChild() {
  const count = useContext(CountContext)
  return <dit>{{ count }}</div>
}
```

</template>
</Donts>

<!--

-->

---
layout: cover
background: https://tenor.com/pt-BR/view/setting-up-this-is-happening-arrange-chairs-seats-gif-16252081.gif
---

# Organizando eventos

---

<Donts>
<template #dont>

```tsx
function Componente() {
  const handle = (e: any, v: number) => {
    console.log(e, v)
  }

  return (
    <>
      <input onChange={(e) => handleIt(e, 1)} />
      <input onChange={(e) => handleIt(e, 2)} />
      <input onChange={(e) => handleIt(e, 3)} />
    </>
  )
}
```

</template>
<template #doit>

```tsx{none|all|2-6,10-12|all}
function Componente() {
  const handle = (v: number) => {
    return (e: any) => {
      console.log(e, v)
    }
  }

  return (
    <>
      <input onChange={handleIt(1)} />
      <input onChange={handleIt(2)} />
      <input onChange={handleIt(3)} />
    </>
  )
}
```

</template>
</Donts>

<!--
Curry Functions -São funções que retornam outra função
-->

---
layout: cover
background: https://tenor.com/pt-BR/view/mord-mit-aussicht-murder-with-a-view-caroline-peters-sophie-haas-meike-droste-gif-26045410.gif
---

# Estado amarrado

---

<Donts>
<template #dont>

```tsx
function Feature() {
  const [state, setState] = useState({
    count: 0,
    x: 20,
    y: 50,
    title: 'Hello',
    ts: Date.now()
  })
  
  const updateCount = () => {
    setState(previous => {
      ...previous,
      count: previous.count + 1
    })
  }
}
```

</template>

<template #doit>

```ts{none|all|1-8|9-12|all}
function useMetadata() {
  const [count, setCount] = useState(0)
  const [coords, setCoords] = useState({x: 0, y: 0})
  const [title, setTitle] = useState('Hello')
  const [ts, setTs] = useState(Date.now())

  return { count, coords, title, ts, setCount, ... }
}

function Feature() {
  const { count, setCount } = useMetadata()
  ...
}
```

</template>
</Donts>

---
layout: image
image: https://tenor.com/pt-BR/view/the-end-gif-9054410.gif
---
