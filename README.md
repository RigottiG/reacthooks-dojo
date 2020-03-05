# React Hooks

## O que é um Hook
>"Hooking, do inglês enganchar, é um conceito que permite modificar o comportamento de um programa. É a chance que um código te dá para alterar o comportamento original de alguma coisa sem alterar seu código."

*Para mais informações:*
[O que são Hooks ?](https://felipeelia.com.br/o-que-sao-hooks/)

## Por que os React Hooks surgiram
Os Hooks vieram para ajudar a resolver uma variedade de problemas que encontramos ao utilizar o React e, por problemas, estamos falando principalmente sobre reutilizar os `states` e **lógicas** entre os componentes.

![State meme](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQaa-ALShh41jIsgBtjak-sSoRWNzGXkfo24AIqnVUca8LVA053)

## Por que devo utilizar os React Hooks

O maior motivo para usar os *hooks* é que eles são **totalmente opcionais** :scream: :scream: :scream:.

Eles possuem total retro compatibilidade com as classes, i.e permitem que você use o `state` e os ciclo de vidas da mesma maneira feita com as classes  ~é aqui que as coisas ficam legais~ mas sem o uso de classes.

O Javascript sempre esteve de certa forma mais próximo do **funcional** do que da **orientação a objetos**, isso pela dificuldade em compreender alguns conceitos do Javascript, exemplo, entender como o `this` funciona. Porém, Hooks adotam funções sem perder o espírito prático de React.
OS Hooks nos dão acesso as variáveis e funções de forma direta e não é necessario aprender programação funcional ou reativa para isso.

# Hand in the pasta ~hands on~
![Hand pasta](https://www.profissaoatitude.com.br/files//Uploads/UploadBass/6700878_ACAO.png)

### Principais Hooks
- **useState**

O primeiro hooks que vamos ver é o **useState**, como o nome diz, ele """cria""" um state para nós, antigamente com as classes, precisavamos declarar os states da seguinte maneira.

```javascript
class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      contador: 0
    }
  }

  render() {
    return (
      <>
        <p>Você clicou {this.state.count} vezes</p>
        <button onClick={() => this.setState({ count: this.state.contador + 1 })}>
          CLICA NEU
        </button>
      </>
    )
  }
}
```

Agora usando o hook **useState** não precisamos mais de um componente classe para criar um estado, faremos de uma maneira desaclopada, que permite mais flexibilidade e mais simples de testar.

Primeiro vamos começar importando o hook useState que a partir da versão 16.8 do React já esta dentro da lib oficial.

```javascript
import React, { useState } from 'react'
```

Como citado antes, ao invés de **classe**, vamos utilizar **funções**, trasformando nossa classe anterior em uma função com hooks teremos algo assim.

```javascript
function App() {

  const [contador, setContador] = useState(0);

  return (
    <>
      <p>Você clicou {contador} vezes</p>
      <button onClick={() => setContador(contador + 1)}>Aperte-me</button>
    </>
  )
}
```
Da mesma maneira que haviamos feito anteriormente com o state contador, aqui criamos um state `contador` e como não possuimos o `setState` como nas classes, no Hook **useState** criamos uma função para setar o nosso state `contador` quando quisermos `setContador`

- **useEffect**

É aqui que a brincadeira fica boa, a partir do momento que relacionamos Hooks com os ciclo de vida do React, que podemos enxergar a real vantagem de utilizar essa nova abordagem.

O React com classes possuia métodos como `componentDidMount` e `componentDidUpdate`, que continham uma mistura de lógicas que não se relacionam, já com os Hooks toda essa lógica é centralizada em um único método `useEffect`.

> Se você está familiarizado com os métodos do ciclo de vida do React, você pode pensar no Hook useEffect como componentDidMount, componentDidUpdate, e componentWillUnmount combinados.

**componentDidMount**

```javascript
class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      users: []
    }
  }

  componentDidMount() {
    fetch(`api.bacanuda`).then(res => this.setState({...users, res}))
  }

  render() {
    return (
        <h1>Quando eu iniciar vou fazer uma busca para uma api bacanuda!!</h1>
    )
  }
}
```

Com o hook useEffect teriamos algo assim.

```javascript
function App() {

  const [users, setUsers] = useState({});

  useEffect({
    fetch(`api.bacanuda`).then(res => setUsers(...users, res))
  }, [])

  return (
    <>
      <p>Você clicou {contador} vezes</p>
      <button onClick={() => setContador(contador + 1)}>Aperte-me</button>
    </>
  )
}
```

o `[]` como segundo parametro da função `useEffect` define o state que o efeito precisa observar para alterar, como queremos que ele roda somente na montagem do componente, não é necessario que passemos nenhum state ali.

Agora que você entendeu isso, ficou facil relacionar com o `componentDidUpdate`, basta passarmos o state que queremos observar no array e todas as vezes que esse state alterar, o efeito vai executar.

**Mas e o componentWillUnmount**

Para o `componentWillUnmount` basta o efeito ter um **return**, exemplo.

```javascript

function resetarContador() {
  setContador(0)
}

useEffect(() => {
    setContador(10)

    return () => { resetarContador() }
  }, [] );
```