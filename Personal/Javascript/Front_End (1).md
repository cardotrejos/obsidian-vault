# JavaScript Questions

## Event delegation
- Agregar el __event listener__ en el componente padre para no tener que agregarlo en cada uno de los hijos
- Esto se aprovecha de que el DOM propaga los eventos HACIA ARRIBA (event bubbling up the DOM)

## How `this` works in JavaScript
- Si la función es un método ->`this` hace referencia al objeto que ejecuta la funcion
	- Si estamos dentro de un objeto, y ese objeto tiene un metodo, entonces `this` apunta al objeto mismo
	- Al ejecutar el metodo `play` , `this` toma el valor del objeto que lo llama. Nos devuelve el mismo objeto `video`

```js
const video = {
	title: 'a',
	play() {
		console.log(this)
	}
}

video.play();
```

- Si no es asi, `this` hace referencia al objeto globa `window` o `global` para navegadores y node 
	- `playVideo` hara un console log del objeto `window`
```js
function playVideo() {
	console.log(this)
}
```

- Si usamos un constructor, con la palabra `new`, esto genera un nuevo objeto vacio `{}` por lo que `this` apuntara a este nuevo objeto
	- Esto creara un nuevo objeto y le asignara el valor `title` que le pasemos
```js
function Video(title){
	this.title = title
	console.log(this)
}
const video = new Video('daniel'); // {}
// {
//    "title": "daniel"
//  }
```

- Tricky question
	- Este codigo funciona bien, devolvera cada valor del tag.
	- Que pasa si usamos `this` dentro del callback function
		- El callback function sigue siendo una funcion anonima.
		- Su `this` apuntaria al global
```js
const video = {
	title: 'a',
	tags: ['a', 'b', 'c'],
	showTags() {
		this.tags.forEach(function(tag) {
			console.log(tag)
		})
	}
}
```

```js
const video = {
	title: 'a',
	tags: ['a', 'b', 'c'],
	showTags() {
		console.log(this)

		this.tags.forEach(function(tag) {
			console.log(this, tag)
			// This mostraria el objeto global
		})
	}
}
```

- Para "corregir" esto, `forEach` tiene un segundo argumento que hace referencia al `this` que se usara para correr. 
	- Pero tambien existen 3 funciones famosas para lograr esto
	- `apply`, `call`, `bind`

```js
const video = {
	title: 'a',
	tags: ['a', 'b', 'c'],
	showTags() {
		this.tags.forEach(function(tag) {
			console.log(this, tag)
		}, this) // usando el segundo argumento de forEach `thisArg`
	}
}
```

- Cuando usamos arrow functions
	- Toman el contexto de donde se definio la funcion.
		- Enclosing object execution context
	- En el ejemplo, tomaria el contexto de donde se definio jeff, es decir, el objeto global
```js
const jeff = {
	name: 'Daniel',
	whos: function() {
		console.log(this) // jeff
	}

	butWhoAmI: () => console.log(this) // global object
}
```

- `bind` -> Crea una nueva funcion con el `this` que le pases como argumento
- `call` y `apply` -> Se usan para invocar la funcion con un `this` especifico.
	- La diferencia es que call recibe parametros extra separados con `,` y apply los recibe como un array 

```js
const persona = {
  nombre: 'John',
  saludar: function() {
    console.log(`Hola, soy ${this.nombre}!`);
  }
};

const saludarPersona = persona.saludar.bind(persona);
saludarPersona(); // Salida: "Hola, soy John!"

// call & apply
function saludar(genero, edad) {
  console.log(`Hola, soy ${this.nombre} y tengo ${edad} años. Soy ${genero}.`);
}

const persona = {
  nombre: 'John'
};

saludar.call(persona, 'masculino', 30); // Salida: "Hola, soy John y tengo 30 años. Soy masculino."

saludar.apply(persona, ['femenino', 25]); // Salida: "Hola, soy Jane y tengo 25 años. Soy femenino."

```

- Chaining methods!!
	- Para poder encadenar metodos uno detras de otro, lo unico que tenemos que hacer es devolver el objeto `return this` 
```js
function Horse(name) {
	this.name = name;
	this.run = function () {
		console.log('run')
		return this
	}
}

const myHorse = new Horse('daniel')
myHorse.run().run().run();
```

## How prototypal inheritance
- NO es herencia, sino conectar los prototipos(metodos/propiedades) del padre al hijo para compartir y utilizar las propiedades
- Si el hijo no tiene una propiedad `x` , js busca la propiedad en el prototipo del objeto (es decir, el objeto "padre") y sigue asi hasta que ya no tenga mas prototipos para buscar o hasta que encuentre la propiedad

## Thread & Call Stack (Encontrar una forma de explicarlo)
- JavaScript es single thread y cada thread tiene un __call stack__ y un __memory__ 
- __Call Stack__ 
	- Stack de funciones a ser ejecutadas
	- Se encarga de manejar el __execution contexts__
	- Es una estructura de datos __LIFO__ -> Last In First Out
	- ![[Pasted image 20230522175011.png]]
	- El ultimo en entrar siempre sera el primero en salir
- Buen video sobre como funciona el call stack con un ejemplo sencillo https://www.youtube.com/watch?v=-G9c4CMMUKc&list=PLillGF-Rfqbars4vKNtpcWVDUpVOVTlgB
- El va ejecutando y las funciones linea por linea y función que termine la remueve de la pila sino, sigue poniendo encima

## Garbage collector
- Main concept es __reachability__
- Si un __root__ (una variable que no se puede eliminar, como la funcion que se esta ejecutando, o el objeto global) puede acceder a otro objeto, se considera alcanzable y no se eliminara
- JS usa un algoritmo llamado __mark and sweep__ 
	- __Mark__: JS recorre todos los roots
		- Recorre exhaustivamente todos los objetos en memoria y se marca cada uno que es alcanzable desde el root
		- Los objetos marcados se consideran en uso
	- __Sweep__: Recorre toda la memoria de nuevo y elimina todos los objetos que no fueron marcados

> [!NOTE]- Ejemplo 
> ![[Pasted image 20230523071140.png]]

## Event Loop
- JS es single thread, y no puede permitirse bloquear la ejecucion con codigo asincrono
- Cuando encuentra algo asincrono lo delega a algun componente externo (libuv en nodejs o el browser engine)
- Cuando se completa la operacion asincrona se agrega al __event queue__ 
	- __Event queue__ es FIFO -> First In First Out
- Cuando el stack esta vacío, inmediatamente se agrega el evento al __call stack__ y se ejecuta

## AMD vs CommonJS
Son estándares para la implementación de módulos en JS
- __CommonJS__ 
	- Usado en servidores
	- Usa `require` para importar módulos
	- `module.exports` o `exports` para exportar módulos

```js
// archivo math.js
var add = function (x, y) {
  return x + y;
};

module.exports = add;

// archivo app.js
var add = require('./math.js');

console.log(add(1, 2)); // outputs: 3
```

- __Asynchronous Module Definition__ 
	- Async - [link](https://www.linkedin.com/feed/update/urn:li:activity:7066744961857134592?utm_source=share&utm_medium=member_desktop)
	- Diseñado para asincronia
	- Es basicamente como un callback function

## What's the difference between a variable that is: `null`, `undefined` or undeclared? How would you go about checking for any of these states?

## What is a closure, and how/why would you use one?

Scope - Son las variables a las que tiene acceso una funcion?
[Understanding Variables, Scope, and Hoisting in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-variables-scope-hoisting-in-javascript)