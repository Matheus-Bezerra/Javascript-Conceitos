# Javascript (Conceitos e Fundamentos)

Neste repositório deixarei algumas explicações do funcionamento do javascript
<br><br>

<h2>Escopo</h2>

Escopo é a área do código em que é possível acessar uma varíavel durante o tempo de execução. Temos dois tipos de escopo:
- Escopo Global: Criado pelo runtime JS (SO para a nuvem que executa o javascript) e pode ser acessado e modificado em qualquer local do seu código
- Escopo local: Blocos onde as variáveis geradas ali só podem serem acessadas ali dentro (função, classe, estrutura de controle e repetição, módulo...)

```javascript
let user = {name: "Matheus"};

function scopo() {
    console.log(user.name) // acessível
    
    let outroUsuario = {name: "Gabriel"};
}

console.log(outroUsuario) // Reference error, escopo não acessível, somente se usasse var que ai acaba vazando o escopo
```

<h2>Garbage collector</h2>

É o mecanismo responsável por gerenciar automaticamente a memória, liberando espaços de objetos e variáveis que não estão sendo mais utilizadas pelo programa para que não acumule memória, evitando vazamento de memória.
```javascript
let usuario1 = {name: "Matheus"} // aloca memória para o Objeto
let usuario2 = {name: "Flávia"};
usuario2 = null; // O objeto anterior não tem mais referência, ou seja é excluido automaticamente
```

<h2>Closures</h2>

É uma função que "lembra" do ambiente onde foi criada, mesmo depois que esse ambiente deixou de existir. Isso significa que ela pode acessar variáveis definidas no escopo externo. O javascript lembra da variável de uma closure enquanto a função depende dela ainda pode ser acessada em algum lugar do código.<br>
Assim que a closure não for mais referenciada (ou seja, nenhuma parte do código pode chamá-la), o JavaScript libera a memória usando o garbage collector.

```javascript
function criarContador() {
    let contador = 0;

    return function () {
        contador++;
        return contador;
    }
}

const meuContador = criarContador();
console.log(meuContador()); // 1
console.log(meuContador()); // 2
console.log(meuContador()); // 3
```

<h2>Callback</h2>

É um padrão que uma função recebe outra (callback) como argumento e com isso executa em um momento pré-acordado

```javascript
function criarContador() {
    let soma = 0

    return function() {
        let teste = [1,2,3];
        teste.forEach((element, index) => {
            soma += element;
        })
        return soma;
    }
}

criarContador() // 6
criarContador() // 12
// Observe que acontece o Closure nesse código também
```

<h2>Call Stack</h2>

Mecanismo que organiza o funcionamento quando são chamadas muitas funções e qual estão sendo executadas no momento, aqui vai algumas observações:
- Quando o script chama a função, ela é adicionada na pilha de funções de forma empilhada, onde vai ser executada uma de cada vez.
- Se houver uma função dentro de uma função, ela vai ser colocada em primeiro na empilhadeira.
- Quando a função termina a execução, o interpretador retira a função da empilhadeira.
- Caso ocupar mais espaço do que foi reservado para a pilha, vai acontecer um erro: "Stack overflow" (estouro de pilha).

<h2>Valores por referência</h2>

Variáveis atribuídas a valores não primitvos(objeto, array, classes, funções...) em Javascript recebem uma referência a esse valor. A referência aponta para o local da memória onde o valor está armazenado. Exemplo:
```javascript
let array1 = [1,2,3,4];
let array2 = array1;
array2.push(5,6)
console.log(array1); // [1,2,3,4,5,6]
console.log(array2); // [1,2,3,4,5,6]

// Para corrigir isso, tem varios jeitos, vou apresentar 3:
// 1 - Spread Operator (...);
let array2 = [...array1];
// 2 - Array.slice();
let array2 = array.slice();
// 3 - Array.from()
let array2 = Array.from(array1)
```

<h2>Immediately Invoked Function Expressions e Módulos</h2>

São funções invocadas imediatamente, que são executadas asso, que são definidas. Usadas principalmente para evitar de poluir o escopo global.
```javascript
(function() {
  const usuario = {name: "Matheus"};
  console.log(usuario.nome) // Saída -> 'Matheus'
})()
```

<h2>Engines de Javascript</h2>

Um mecanismo que interpreta e executa o código Javascript. Implementação de baixo nível que transforma o código Javascript em um formato que pode ser entendido e executado pela máquina. <br>
<h3>Exemplos de engines populares</h3>

- V8 (Google)
- Chakra (Microsoft)
- SpiderMonkey (Mozilla)

<h2>Classes e factories</h2>

Classes são uma forma de definir estruturas e comportamentos para objetos. Permite retornar métodos e propriedades de forma clara e legível. <br>
Factories são funções que retornam objetos (pode ser tipo propriedades e métodos), alternativa mais flexível e simples às classes.
```javascript
class Carro {
  constructor(marca, modelo) {
    this.marca = marca;
    this.modelo = modelo

    informacao() {
      return `Marca: ${this.marca}, Modelo: ${this.modelo}`
    }
  }
}

const carro = new Carro("BMW", "320I");
console.log(carro.informacao()) 

// Usando factories (funções fábricas)
function criarCarro(marca, modelo) {
  return {
    marca: marca,
    modelo: modelo,
    informacao() {
      return `Marca: ${this.marca}, Modelo: ${this.modelo}`
    }
  }
}
console.log(criarCarro("BMW", "320I").informacao())
```
 
<h2>Collections</h2>

Collections são estrutura de dados que armazenam múltiplos valores. Maneiras mais poderosas e eficientes de trabalhar com dados do que os array tradicionais. Principais são: <br>

1 - Map: Coleção de pares chave-valor, onde tanto chave quanto valor podem ser de qualquer tipo. Tipo a chave pode ser um booleano ou um tipo booleano.
```javascript
let mapa = new Map();
mapa.set("nome", "Matheus")
mapa.set(1, "um")
mapa.set(true, "Verdadeiro")
console.log(mapa.get("nome")) // Saída: Matheus
console.log(mapa.has(1)) // Saída: true
```

2 - Set: Coleção de valore únicos que não permite valores duplicados, bom para garantir que não vai existir nenhum duplicado.
```javascript
let conjunto = new Set();
conjunto.add(1);
conjunto.add(2);
conjunto.add(3);
```

<h2>Generators</h2>

São funções especiais que permitem pausar sua execução e retornar de ondem pararam, sem bloquear a execução de outros códigos.  Elas são declaradas com * após o function (para declarar que é uma função geradora) e usam a palavra chave yield para pausar a execução
```javascript
function* contador() {
  yield 1;
  yield 2;
  yield 3;
}

const gerador = contador();
console.log(gerador.next()) // {value: 1, done: false}
console.log(gerador.next()) // {value: 2, done: false}
console.log(gerador.next()) // {value: 3, done: true}
```

<h2>Promises</h2>

São objetos usados para lidar com operações assíncronas de clara e bem estruturada. Pode estar em três estados:

- Pending (pendente): Operação ainda está em execução e o resultado não disponível
- Fullfilled (resolvida): Operação quando é concluída com sucesso e um valor retornado
- Rejected (rejeitada): Operação que falhou e um erro foi retornado

```javascript
const promise = new Promise((resolve, reject)) => {
  const sucesso = true;

  if(sucesso) {
    resolve("Operação com sucesso")
  } else {
    reject("Houve um erro")
  }
}
```
Para lidar com resultados das Promise, podemos usar alguns métodos:
- .then(): Chamado quando a Promessa é resolva
- .catch(): Chamado quando a promessa é rejeitada
- .finally(): Executando quando a Promessa foi resolvida ou rejeitada

<h2>async/await</h2>

Outra forma de fazer um trecho do seu código ser de forma assíncrona de forma mais legível e mais estruturada
- Async é usada para declarar uma função assícrona. A sua função sempre vai retornar uma Promise <br>
- Awaité usado dentro de funções para esperar a resolução da Promise

```javascript
// Com Promises
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));

// Com async/await
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
fetchData();
```
