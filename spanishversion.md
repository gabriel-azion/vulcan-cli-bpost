Texto: # Implementación avanzada en Edge: Abrazando los Polyfills de Node.js (1)
# Desbloquea una implementación sin esfuerzo en Edge con Vulcan y los Polyfills de Node.js (2)
# Desbloquea una implementación sin esfuerzo en Edge con Vulcan y los Polyfills de Node.js (3)

Como desarrolladores, todos estamos familiarizados con el mundo acelerado y en constante cambio de los marcos de trabajo web. A veces, puede ser confuso ya que sobresalir en el uso de un marco específico requiere una comprensión más profunda que va más allá de saber solo el lenguaje de programación. Uno de los obstáculos significativos que se enfrentan es configurar el proyecto para que se ejecute sin problemas en diferentes entornos.

A menudo surgen preguntas como "¿Funcionará en la nube?" y "¿En qué plataforma debería funcionar?" Pero, ¿y si pudieras implementar tu proyecto en el borde de la red, aprovechando la baja latencia, la seguridad y la disponibilidad sin preocuparte por el entorno específico? Este blog explora el concepto de adaptar proyectos para ejecutarse en Edge e introduce una solución que simplifica el proceso.

## Adaptando Proyectos para Ejecutarse en Edge

Para asegurar una implementación sin problemas, un proyecto debe estructurarse de manera que evite la fricción con la plataforma anfitriona. No es raro encontrarse con situaciones en las que los ajustes requeridos para ejecutar una aplicación en una plataforma tienen poco en común con los necesarios para una plataforma diferente. Esto puede llevar al bloqueo del proveedor, donde cambiar de anfitriones se vuelve costoso en términos de tiempo y dinero, lo que obliga a los clientes a quedarse con el proveedor de servicios actual.

En Azion, valoramos el poder del software de código abierto y nuestro objetivo es potenciar el desarrollo web moderno. Nuestra plataforma permite la inicialización y despliegue de proyectos en varios frameworks web, incluyendo:

- Next.js
- React
- Vue
- Astro
- Angular
- Hexo
- Vite
- JavaScript en sí mismo

Azion proporciona su propio [Edge Runtime]() diseñado para ofrecer una experiencia óptima para ejecutar aplicaciones en Edge. Este tiempo de ejecución alimenta [Azion Edge Functions]() y abre un mundo de posibilidades tanto para los desarrolladores como para las empresas. Además, hemos desarrollado [Vulcan]() para cerrar las brechas y permitir que los frameworks web se ejecuten de forma nativa en Edge. Vulcan simplifica la integración de *polyfills* para Edge Computing, revolucionando el proceso de creación de Workers, especialmente para la plataforma Azion.

> Si tienes curiosidad sobre qué son los *polyfills*, son fragmentos de código utilizados en JavaScript para proporcionar funcionalidad moderna a los entornos que no la soportan de forma nativa. Los polyfills llenan los vacíos, asegurando un comportamiento consistente en diferentes navegadores. Por ejemplo, consideremos un tiempo de ejecución que carece de soporte para una API específica de Node.js, de la cual depende un proyecto. Durante el proceso de construcción, Vulcan reconoce la firma de esta API y la reemplaza con una API relativa, eliminando la necesidad de que el proyecto se adapte manualmente.

Vulcan destaca en la configuración de un protocolo intuitivo y simplificado para la creación de *presets*. Esta característica mejora la personalización y permite a los desarrolladores adaptar sus aplicaciones a los requisitos únicos del proyecto, brindando la flexibilidad necesaria para optimizarlas de manera efectiva.

## Configurando un Proyecto

La [CLI de Azion]() es una herramienta excepcional que mejora enormemente la experiencia del desarrollador. Con la CLI instalada en tu entorno, inicializar un proyecto es tan simple como ejecutar el siguiente comando:

```bash
azion
```
Este comando inicia una jornada interactiva donde puede elegir la plantilla deseada.

![Listado de Plantillas Disponibles](templates-list.png)

Una vez que hayas seleccionado la plantilla, cada framework presentará un conjunto específico de pasos. Elige ejecutar el proyecto localmente e instala las dependencias requeridas.

### Aprovechando los Polyfills

Vulcan hace posible aprovechar los *polyfills*. Vamos a ver más de cerca cómo configurar un proyecto para utilizar esta función.

**Ejemplo**: Supongamos que quieres inicializar un proyecto de JavaScript que utiliza la API Buffer de Node.js. Para lograr esto, necesitas informar a Vulcan que el proyecto implementa polyfills.

Vulcan lee un archivo de configuración llamado `vulcan.config.js`. Crea este archivo e incluye las siguientes propiedades:

```js
module.exports = {
  entry: 'main.js',
  builder: 'webpack',
  useNodePolyfills: true,
};
```

- **entry**: Representa el punto de entrada principal para tu aplicación donde comienza el proceso de construcción. No aplicable para soluciones Jamstack.
- **builder**: Define la herramienta de construcción a utilizar, ya sea `esbuild` o `webpack`.
- **useNodePolyfills**: Especifica si se deben aplicar los polyfills de Node.js.

Después de aplicar estos ajustes, puedes importar las APIs necesarias en tu proyecto. Por ejemplo, consideremos importar el Buffer de Node.js:

**Dentro de main.js**:

```js
// Importa la clase Buffer del módulo 'buffer' en Node.js
import { Buffer } from 'node:buffer';

// Define una función llamada 'myWorker' que toma un evento como argumento
export default function myWorker(event) {
  // Crea una nueva instancia de Buffer 'buf1' a partir de la cadena "x"
  var buf1 = Buffer.from("x");
  // Crea una nueva instancia de Buffer 'buf2' a partir de la cadena "x"
  var buf2 = Buffer.from("x");
  // Compara 'buf1' y 'buf2' utilizando el método Buffer.compare,
  // que devuelva un número indicando la igualdad de los buffers
  var a = Buffer.compare(buf1, buf2);

  // El resultado será 0 ya que ambos buffers son iguales
  console.log(a);

  // Ahora, intercambiemos los valores de 'buf1' y 'buf2'
  buf1 = Buffer.from("y");
  buf2 = Buffer.from("x");
  // Compara 'buf1' y 'buf2' de nuevo
  a = Buffer.compare(buf1, buf2);

  // Aquí, el resultado será 1
  console.log(a);

  // La función devuelve un nuevo objeto de Respuesta con la cadena "Probando polyfills"
  return new Response("Probando polyfills");
}
```

Para ejecutar el proyecto localmente, utiliza el siguiente comando:

```bash 
azion dev
```

**Salida**:

![Salida de Azion Dev](azion-dev-output.png)

Ahora, puedes acceder al proyecto localmente.

> Nota: Asegúrate de estar en el directorio raíz de tu proyecto para que los comandos se ejecuten correctamente.

El comando `azion dev` inicia el **proceso de construcción**, que es manejado de manera fluida por Vulcan y devuelve el puerto para acceder al proyecto.

El proyecto de muestra utilizado en este ejemplo se puede encontrar en el repositorio [Azion Samples](https://github.com/aziontech/azion-samples/tree/dev/samples/polyfills/buffer) en GitHub.

---

## Tiempo de Ejecución de Edge, Vulcan y CLI de Azion

![cli - vulcan - runtime ](cli-vulcan-runtime.png)

### Proceso explicado 

1. El usuario inicia el proceso ejecutando `azion` o `azion init` a través de CLI de Azion, y selecciona la plantilla deseada.
2. La CLI de Azion invoca a Vulcan, que se encarga de iniciar el proyecto.
3. Una vez iniciado, Vulcan devuelve el proyecto al usuario, quien decide si desplegar el proyecto o ejecutarlo localmente.
4. Si el usuario opta por ejecutar el proyecto localmente, Vulcan activa el proceso de construcción y genera el worker.
5. Si el usuario decide implementar el proyecto, ejecuta el comando `azion deploy`. Esto da inicio de nuevo al proceso de construcción, tras lo cual la CLI de Azion crea una aplicación en Edge y la implementa en la red Edge distribuida de Azion.
6. Tras una implementación exitosa, la CLI de Azion proporciona el dominio de la aplicación. Después de un breve período de espera, la aplicación se vuelve accesible.

### Responsabilidades 

**CLI de Azion**: la interfaz de línea de comandos que sirve como principal punto de interacción entre el usuario y el sistema. Administra todo el proceso de implementación de la aplicación, garantizando un flujo de trabajo suave y eficiente.

**Vulcan**: el motor que impulsa la inicialización, construcción y adaptación del proyecto. Adapta inteligentemente el proyecto basándose en la plantilla seleccionada, garantizando que la aplicación esté configurada de manera óptima para su uso previsto.

**Tiempo de Ejecución de Edge de Azion**: el entorno de ejecución que aloja la aplicación y gestiona su ejecución. La aplicación se distribuye a través de la red global de nodos Edge de Azion, asegurando que siempre esté cerca de los usuarios, maximizando así la velocidad y eficiencia.

---

Al utilizar la plataforma de Azion, los desarrolladores pueden ahora disfrutar de una mejor experiencia de implementación para los marcos de trabajo web en Edge. Adapta tus proyectos para que se ejecuten sin problemas, mientras te centras en la funcionalidad principal en lugar de preocuparte por el entorno específico. Con el Tiempo de Ejecución del Edge de Azion y la herramienta Vulcan, los desarrolladores obtienen el poder para optimizar efectivamente sus aplicaciones, aprovechando la baja latencia, seguridad y disponibilidad en el borde de la red.
		---

		- Traduce este texto al español latinoamericano.
		- No cambies el contexto
		- El formato de salida debe ser en markdown
