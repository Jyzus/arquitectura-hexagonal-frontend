# Como implementar arquitectura hexagonal en el frontend (Javascript/Typescript)

Este repositorio es una traducción directa de del artículo de juanm4, autor original de la información. El fin de este proyecto es la divulgación de contenido informativo, no busco desmerecer ni copiar ningún contenido. Pásatelo bien, y continúa aprendiendo 😉🖐️.

**Tabla de contenido**

## Introducción 👋
---
Existen múltiples definiciones para el término arquitectura, según el contexto y la rama de desarrollo de la que provenga. Por estos motivos es complicado llegar a un consenso y a una definición única que sea válida para todos los casos. Entonces, según el desarrollo de software frontend, y desde un punto de vista profesional, la definición podría ser la siguiente:

**Los desarrolladores llaman arquitectura al conjunto de patrones de desarrollo que nos permiten definir pautas a seguir en nuestro software en cuanto a límites y restricciones. Es la guía que debemos seguir para ordenar nuestro código y lograr que las distintas partes de la aplicación se comuniquen entre sí.**

Existe un amplio abanico de opciones a la hora de elegir una arquitectura u otra. Cada uno tendrá sus propias ventajas y desventajas. Incluso una vez que elegimos cuál se adapta mejor a nuestro caso, no necesariamente tiene que implementarse de la misma forma en diferentes proyectos.

Sin embargo, aunque la cantidad de opciones es casi infinita, la mayoría mantiene en común sus atributos de calidad, tales como: escalabilidad, responsabilidad única, bajo acoplamiento, alta cohesión, etc.

Entonces, de manera general, es crucial comprender los conceptos y la razón por la que se ha elegido una solución u otra.

Uno de los patrones más utilizados para diseñar arquitectura de software es la Arquitectura Hexagonal, también conocida como Puertos y Adaptadores.

El objetivo de este patrón es dividir nuestra aplicación en diferentes capas, permitiendo que evolucione de forma aislada y haciendo que cada entidad sea responsable de una única funcionalidad.

## ¿Por qué esta arquitectura se llama hexagonal? 🤔
---
La idea de representar esta arquitectura con un hexágono se debe a la facilidad de asociar el concepto teórico con el concepto visual. Dentro de este hexágono es donde se encuentra nuestro código base. Esta parte se llama dominio.

Cada lado de este hexágono representa una interacción con un servicio externo, por ejemplo: servicios http, base de datos, renderizado...

![arquitectura_hexagonal](arquitectura_hexagonal.png)

La comunicación entre el dominio y el resto de actores se realiza en la capa de infraestructura. En esta capa implementamos un código específico para cada una de estas tecnologías.

Una de las preguntas más recurrentes entre los profesionales que ven por primera vez esta arquitectura es: ¿Por qué un hexágono? Bueno, el uso de un hexágono es sólo una representación teórica. La cantidad de servicios que podríamos agregar es infinita y podemos integrar tantos como necesitemos.

## Mismo concepto diferentes nombres 😒
---
El patrón de arquitectura hexagonal también se denomina puertos y adaptadores. Este nombre surge de una separación dentro de una capa de **infraestructura**, donde tendremos dos subcapas:

- **Puerto**: Es la interfaz que nuestro código debe implementar para poder abstraerse de la tecnología. Aquí definimos las firmas de métodos que existirán. 

- **Adaptador:** Es la implementación de la propia interfaz. Aquí tendremos nuestro código específico para consumir una tecnología concreta. Es importante saber que esta implementación NO debe estar en nuestra aplicación, más allá de la declaración, ya que su uso se realizará a través del puerto.

Así, nuestro dominio realizará llamadas a la subcapa que corresponde al puerto, quedando desacoplado de la tecnología, mientras que el puerto, a su vez, consumirá el adaptador.

El concepto de Puertos y Adaptadores está muy ligado a la programación orientada a objetos y al uso de interfaces, y tal vez, la implementación de este patrón en la programación funcional pueda ser diferente al concepto inicial. De hecho, han surgido muchos patrones que iteran sobre esto, como la **Arquitectura Onion** o la **Arquitectura Limpia (Clean architeture)**. Al final el objetivo es el mismo: dividir nuestra aplicación en capas, separando dominio e infraestructura.

## ¿Cómo afecta la mantenibilidad? 🧐
---
El hecho de tener nuestro código separado en capas, donde cada una de ellas tiene una única responsabilidad, ayuda a que cada capa evolucione de diferentes maneras, sin impactar a las demás.

Además, con esta segmentación conseguimos una alta cohesión, donde cada capa tendrá una responsabilidad única y bien definida dentro del contexto de nuestro software.

## ¿Cómo afecta la interfaz? 😮
---
Actualmente existen una serie de falencias en el uso de metodologías a la hora de crear aplicaciones. Hoy en día, contamos con una increíble cantidad de herramientas que nos permiten desarrollar aplicaciones muy rápidamente y, al mismo tiempo, hemos dejado en un segundo plano el análisis y la implementación de arquitecturas conocidas y probadas.

Sin embargo, aunque estas arquitecturas puedan parecer del pasado, donde los lenguajes no evolucionaron tan rápido, estas arquitecturas han sido mostradas y adaptadas para brindarnos la escalabilidad que necesitamos para desarrollar aplicaciones reales.

## Contexto histórico 😴
---
Hace dos décadas las aplicaciones de escritorio eran la principal herramienta a desarrollar. En ellos toda nuestra aplicación estaba instalada en la máquina, a través de librerías, y había un alto acoplamiento entre vista y comportamiento. Luego, quisimos escalar nuestras aplicaciones para obtener un software más fácil de mantener, con bases de datos centralizadas. Muchos de ellos fueron migrados a un servidor. Con esto, nuestras aplicaciones de escritorio quedaron reducidas a aplicaciones "tontas", que no requerían acceso, persistencia ni muchos datos. Finalmente, si la aplicación necesitaba algunos datos, tenía la responsabilidad de realizar estas llamadas a los servidores externos a través de servicios de red. Es aquí cuando empezamos a distinguir entre "frontend" y "backend".

Durante los siguientes años tuvimos el boom web. Muchas aplicaciones de escritorio se adaptaron a los navegadores, donde las limitaciones eran mayores solo con HTML. Posteriormente JAVASCRIPT empezó a darle más posibilidades al navegador.

## Presente 😌
---
Las vistas siempre se habían limitado únicamente a la representación de datos y nunca habían necesitado funcionalidades superiores, hasta ahora. Con necesidades comunes, las aplicaciones frontend tienen más requisitos que hace años. Por poner algunos ejemplos: gestión del estado, seguridad, asincronía, animaciones, integración con servicios de terceros…

Por todas estas razones, debemos empezar a aplicar patrones en estas aplicaciones.

## Consecuencias 🥳

Como hemos dicho, el propósito del frontend es principalmente visualizar datos. A pesar de esta percepción, NO es el dominio de nuestra aplicación, sino que pertenece a las capas externas de la arquitectura del implemento.

Los casos de uso de la aplicación pertenecen al dominio y no deberían saber cómo se deben visualizar los datos.

Por ejemplo, supongamos que está desarrollando un carrito de compras. Un caso de uso podría ser: "Un carrito de compras no puede tener más de 10 productos". Otro caso de uso: “Un carrito de compras no puede tener el mismo producto dos o más veces”. Podemos ver los casos de uso como requisitos en nuestra aplicación.

Las solicitudes de datos al backend pertenecen a la capa de **infraestructura**, y es algo que nuestra aplicación no necesita saber, incluso si administramos el backend (son aplicaciones diferentes y tienen diferentes necesidades de arquitectura. En algún momento, el esquema de datos de El backend podría cambiar y no queremos que nuestra aplicación se vea afectada por eso.

Otra parte que pertenece a la **infraestructura** es la gestión de datos locales, como datos de sesión, cookies o bases de datos locales. Por supuesto, tenemos que ocuparnos de esto, pero no es parte de nuestro dominio.

## ¿Qué hacemos con los marcos/bibliotecas frontend? 😎

Hoy en día existe una enorme cantidad de librerías para renderizado: Angular, React, Vue…; pero debemos entender cuál es su propósito, por lo que no deben ingresar al dominio, sino que deben ser manejados dentro de la infraestructura.

Todas estas herramientas tienden a evolucionar rápidamente y nuestro código no quiere verse afectado por ellas. Ahora bien, ¿Cómo podemos lograr eso? Bueno, una de las estrategias más comunes es envolver estas bibliotecas en funcionalidades creadas para este propósito. Esta estrategia se conoce como "Wrapping" (Envase) y con ella podemos aislar nuestro código de los efectos secundarios que puedan tener estas bibliotecas.

El "Wrapping" es una buena práctica, pero cuando lo utilicemos debemos hacerlo a través de un adaptador para reducir el acoplamiento. Además, esta técnica también tiene desventajas. Por ejemplo, si abusamos de eso, es posible que estemos sobre-diseñando nuestro producto, aumentando el tiempo de mantenimiento. Entonces, tenemos que identificar en qué casos vale la pena y en cuáles no.

Podemos afirmar que la comunicación entre vista (infraestructura) y dominio es unidireccional, es decir, son un punto de entrada para el usuario, pero nunca serán consumidos por el dominio.

Una vez entendido esto, debemos asumir que las herramientas altamente acopladas a estas bibliotecas frontend, como Redux o Vuex, deben administrarse en la capa de infraestructura.

## Ejemplo de Arquitectura Hexagonal 🚀

Ahora ha llegado el momento del espectáculo, intentemos poner en práctica toda esta teoría a través de un ejemplo. Escribamos un código.

Imaginemos que tenemos que diseñar un carrito de compras, y tenemos que hacerlo en "reactjs", "vuejs" y "React Native".