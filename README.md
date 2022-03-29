# React - Componentes

### Funcionales y de Clase

Hay dos maneras de crear componentes en react. Una es básicamente idéntica a una función en Javascript, por ej:

```jsx
function Saludo(props) {
  return <h1>Hola, {props.nombre}</h1>;
}
```

Esto es una función normal, que debe respetar el inicio del nombre siempre en Mayúsculas, para que React reconozca que estamos creando un componente funcional, y por otro lado opcionalmente recibir props (properties) para que pueda ser configurado o reutilizado a nuestra conveniencia. Por último, siempre debe retornar JSX, que es una sintaxis amigable muy parecida a HTML, pero de fondo React lo convierte realmente en HTML en última instancia.

***

La otra manera, gradualmente en desuso, es crear un componente de clase, de la siguiente manera:

```jsx
class Saludo extends React.Component {
  render() {
    return <h1>Hola, {this.props.nombre}</h1>;
  }
}
```
***

Podemos recalcar que originalmente solo los componentes de clase gozaban de la habilidad para contener un estado, eso puede traducirse como datos que al cambiar, hacen que la aplicación vuelva a crear ese componente pero actualizándolo de maneras que nosotros podemos definir.
Con la inclusión de Hooks, los componentes funcionales pasaron de ser componentes `stateless` a componentes `stateful`. En simples palabras, componentes capaces de contener estado propio.

***

### Hooks

Los hooks son una adición a React para que los componentes funcionales, anteriormente `stateless`, puedan contener estado y gozar de la reactividad que ofrece React.

Veremos que el estado en un componente de clase se podía definir de esta manera:

```jsx
class Reloj extends React.Component {
  constructor(props) {
    super(props);
    this.state = { fecha: new Date() };
  }

  render() {
    return (
      <div>
        <h1>Reloj</h1>
        <h2>La hora es {this.state.fecha.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Básicamente, agregamos en el constructor de la clase nuestro `state`, definiéndolo en el objeto `this` de toda la clase para que sea accesible dentro de la misma.

Tengamos en cuenta que React nos provee un método para cambiar el estado, así no tenemos que cambiar el objeto directamente generando que React pierda el rastreo de que el estado cambió e imposiblitando que un componente se vuelva a renderizar y actualizar correctamente.

```jsx
// No hay que actualizar el objeto state directamente
this.state.saludo = 'Hola';
```

```jsx
// Utilizamos siempre setState() para cambiar el estado interno
this.setState({ saludo: 'Hola' });
```

## Ciclo de vida

Para poder explicarle a React cómo queremos que el componente cambie ante ciertos actos, por ejemplo, una interacción del usuario o un cambio de un timer en este caso, tenemos a disposición los métodos básicos `componentDidMount()` y `componentWillUnmount()`.

`componentDidMount()` es un buen lugar para depositar acciones que queramos que se ejecuten cuando el componente ya se muestra en la pantalla:

```jsx
  componentDidMount() {
      console.log('El componente ya está visible en la pantalla')
  }
```

`componentWillUnmount()` por otro lado, se utilizad generalmente como una limpieza para cuando el componente ya no está visible:

```jsx
  componentWillUnmount() {
      console.log('Este es un buen lugar para limpiar timers...')
  }
```

## High Order Components

Los componentes de alto nivel son una técnica en React para reusar la lógica en un componente. Cabe aclarar que no son parte de la API de React, es solamente un patrón que emerge de la naturaleza compositva de React.

Básicamente un HOC es una función que acepta un componente y retorna un nuevo componente.

```jsx
const ComponenteMejorado = funcionHOC(ComponenteParaMejorar);
```

Un ejemplo simple, podemos crear un HOC que agrege un prop a un componente, con la ventaja de que el componente original no es modificado:

```jsx
function AgenteSimple(nombre) {
    return <h1>{nombre}</h1>
}

function withPropiedadEspecial(Componente) {
  // And return a new anonymous component
  return class extends React.Component {
    render() {
      return <Componente especial={true} {...this.props} />
    }
  }
}

function AgenteEspecial = withPropiedadEspecial(AgenteSimple)
```