Texto: # Implementación avanzada en el edge: adoptando los polyfills de Node.js (1)
# Obtén una implementación sencilla en el edge con Vulcan y los polyfills de Node.js (2)
# Implementa en el edge de forma sencilla con Vulcan y los polyfills de Node.js (3)

Como desarrolladores, estamos familiarizados con el mundo acelerado y en constante cambio de los frameworks web. A veces, puede ser confuso ya que destacarse en el uso de un framework específico requiere una comprensión profunda que va más allá de saber solo el lenguaje de programación. En este sentido, configurar el proyecto para que se ejecute sin problemas en diferentes entornos es uno de los más grandes obstáculos que se debe enfrentar.

A menudo surgen preguntas como "¿funcionará en la nube?" y "¿en qué plataforma debería funcionar?". Pero, ¿y si pudieras implementar tu proyecto en el edge de la red, aprovechando la baja latencia, la seguridad y la disponibilidad sin preocuparte por un entorno específico? Este artículo explora la adaptación de proyectos para ejecutarlos en el edge e introduce una solución que simplifica este proceso.

## Adaptando proyectos para ejecutarse en el edge

Para garantizar una implementación sin problemas, un proyecto debe estructurarse de manera que evite la fricción con la plataforma que lo va a hospedar. No es raro encontrarse con situaciones en las que los ajustes requeridos para ejecutar una aplicación en una plataforma tienen poco en común con los requisitos de otra. Esto puede ocasionar el  llamado vendor lock-in, donde cambiar de plataforma de hospedaje se vuelve costoso en términos de tiempo y dinero, lo que obliga a los clientes a quedarse con el proveedor de servicios actual.

En Azion, valoramos el poder del software de código abierto y nuestro objetivo es potenciar el desarrollo web moderno. Nuestra plataforma permite la inicialización y despliegue de proyectos en varios frameworks web, incluyendo:

- Next.js
- React
- Vue
- Astro
- Angular
- Hexo
- Vite
- Y el propio JavaScript

Azion proporciona su propio [Edge Runtime]() diseñado para ofrecer una experiencia óptima para ejecutar aplicaciones en el edge. Este tiempo de ejecución alimenta a las [Azion Edge Functions]() y abre un mundo de posibilidades tanto para los desarrolladores como para las empresas. Además, hemos desarrollado [Vulcan]() para cerrar las brechas y permitir que los frameworks web se ejecuten de forma nativa en el edge. Vulcan simplifica la integración de *polyfills* para edge computing, revolucionando el proceso de creación de Workers, especialmente para la plataforma Azion.

> Los *polyfills*, son fragmentos de código utilizados en JavaScript para proporcionar funcionalidades modernas a los entornos que no las soportan de forma nativa. Los polyfills llenan esas brechas, lo que proporciona un comportamiento consistente en diferentes navegadores. Por ejemplo, consideremos un tiempo de ejecución que carece de soporte para una API específica de Node.js, de la cual depende un proyecto. Durante el proceso de creación o build, Vulcan reconoce la firma de esta API y la reemplaza con una API relativa, lo que elimina la necesidad de adaptar el proyecto manualmente.

Vulcan destaca con la configuración de un protocolo intuitivo y simplificado para la creación de *presets*. Esta característica mejora la personalización y permite a los desarrolladores adaptar sus aplicaciones a los requisitos únicos del proyecto, brindando la flexibilidad necesaria para optimizarlas de manera efectiva.

## Configurando un proyecto

La [CLI de Azion]() es una herramienta excepcional que mejora la experiencia del desarrollador. Con la CLI instalada en tu entorno, solo tendrás que ejecutar el siguiente comando para inicializar un proyecto:

```bash
azion
```

Este comando inicia una jornada interactiva donde puedes elegir el template deseado.

Una vez que hayas seleccionado el template, cada framework presentará un conjunto específico de pasos. Elige ejecutar el proyecto localmente e instala las dependencias requeridas.

### Adoptando los polyfills

Vulcan hace posible aprovechar los *polyfills*. Vamos a ver cómo configurar un proyecto para utilizar esta función.

**Ejemplo**: supongamos que quieres inicializar un proyecto de JavaScript que utiliza la API Node.js Buffer. Para hacerlo, necesitas informar a Vulcan que el proyecto implementa polyfills.

Para ello, Vulcan lee un archivo de configuración llamado `vulcan.config.js`. Crea este archivo y agrega las siguientes propiedades:

```js
module.exports = {
  entry: 'main.js',
  builder: 'webpack',
  useNodePolyfills: true,
};
```

- **entry**: representa el punto de entrada principal para tu aplicación donde comienza el proceso de creación. No es aplicable para soluciones Jamstack.
- **builder**: define la herramienta de creación a utilizar, ya sea `esbuild` o `webpack`.
- **useNodePolyfills**: especifica si se deben usar los polyfills de Node.js.

Después de realizar estos ajustes, puedes importar las APIs necesarias en tu proyecto. Por ejemplo, para importar Node.js Buffer:

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

  // La función devuelve un nuevo objeto de Respuesta con la string "Testing buffer polyfills"
  return new Response("Testing polyfills");
}
```

Para ejecutar el proyecto localmente, utiliza el siguiente comando:

```bash 
azion dev
```

**Salida**:

![Salida de Azion Dev](azion-dev-output.png)

Ahora, puedes acceder al proyecto localmente.

> Nota: asegúrate de estar en el directorio raíz de tu proyecto para que los comandos se ejecuten correctamente.

El comando `azion dev` inicia el **proceso de creación**, que es manejado de manera fluida por Vulcan y devuelve el puerto para acceder al proyecto.

El proyecto de muestra utilizado en este ejemplo se puede encontrar en el repositorio [Azion Samples](https://github.com/aziontech/azion-samples/tree/dev/samples/polyfills/buffer) en GitHub.

---

## Edge Runtime, Vulcan y CLI de Azion

![cli - vulcan - runtime ](cli-vulcan-runtime.png)

### Proceso explicado 

1. El usuario inicia el proceso ejecutando `azion` o `azion init` a través de CLI de Azion y selecciona el template deseado.
2. La CLI de Azion llama a Vulcan, que se encarga de iniciar el proyecto.
3. Una vez iniciado, Vulcan devuelve el proyecto al usuario, quien decide si implementar el proyecto o ejecutarlo localmente.
4. Si el usuario opta por ejecutar el proyecto localmente, Vulcan activa el proceso de creación y genera el worker.
5. Si el usuario decide implementar el proyecto, ejecuta el comando `azion deploy`. Esto da inicio de nuevo al proceso de creación; luego, la CLI de Azion crea una aplicación en el edge y la implementa en la red de edge distribuida de Azion.
6. Tras una implementación exitosa, la CLI de Azion proporciona el dominio de la aplicación. Después de un breve período de espera, la aplicación está disponible.

### Responsabilidades 

**CLI de Azion**: es la interfaz de línea de comandos que sirve como principal punto de interacción entre el usuario y el sistema. Administra todo el proceso de implementación de la aplicación, garantizando un flujo de trabajo fluido y eficiente.

**Vulcan**: es el motor que impulsa la inicialización, creación y adaptación del proyecto. Adapta inteligentemente el proyecto basándose en el template seleccionado, garantizando que la aplicación esté configurada de manera óptima para el uso previsto.

**Edge Runtime de Azion**: es el entorno de ejecución que hospeda la aplicación y gestiona su ejecución. La aplicación se distribuye a través de la red global de edge nodes de Azion, garantizando que siempre esté cerca de los usuarios, lo que maximiza su velocidad y eficiencia.

---

Al utilizar la plataforma de Azion, los desarrolladores pueden disfrutar de una mejor experiencia de implementación para los framewoks web en el edge. Ahora puedes adaptar tus proyectos para que se ejecuten sin problemas, mientras te centras en la funcionalidad principal sin preocuparte por el entorno específico. Con el Edge Runtime de Azion y Vulcan, los desarrolladores obtienen el poder para optimizar efectivamente sus aplicaciones, aprovechando la baja latencia, seguridad y disponibilidad en el edge de la red.
---
