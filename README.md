# Como implementar arquitectura hexagonal en el frontend (Javascript/Typescript)

Este repositorio es una traducción directa de del artículo de juanm4, autor original de la información. El fin de este proyecto es la divulgación de contenido informativo, no busco desmerecer ni copiar ningún contenido. Pásatelo bien, y continúa aprendiendo 😉🖐️.

**Tabla de contenido**

## Introducción 👋

Existen múltiples definiciones para el término arquitectura, según el contexto y la rama de desarrollo de la que provenga. Por estos motivos es complicado llegar a un consenso y a una definición única que sea válida para todos los casos. Entonces, según el desarrollo de software frontend, y desde un punto de vista profesional, la definición podría ser la siguiente:

**Los desarrolladores llaman arquitectura al conjunto de patrones de desarrollo que nos permiten definir pautas a seguir en nuestro software en cuanto a límites y restricciones. Es la guía que debemos seguir para ordenar nuestro código y lograr que las distintas partes de la aplicación se comuniquen entre sí.**

Existe un amplio abanico de opciones a la hora de elegir una arquitectura u otra. Cada uno tendrá sus propias ventajas y desventajas. Incluso una vez que elegimos cuál se adapta mejor a nuestro caso, no necesariamente tiene que implementarse de la misma forma en diferentes proyectos.

Sin embargo, aunque la cantidad de opciones es casi infinita, la mayoría mantiene en común sus atributos de calidad, tales como: escalabilidad, responsabilidad única, bajo acoplamiento, alta cohesión, etc.

Entonces, de manera general, es crucial comprender los conceptos y la razón por la que se ha elegido una solución u otra.

Uno de los patrones más utilizados para diseñar arquitectura de software es la Arquitectura Hexagonal, también conocida como Puertos y Adaptadores.

El objetivo de este patrón es dividir nuestra aplicación en diferentes capas, permitiendo que evolucione de forma aislada y haciendo que cada entidad sea responsable de una única funcionalidad.

## ¿Por qué esta arquitectura se llama hexagonal? 🤔

La idea de representar esta arquitectura con un hexágono se debe a la facilidad de asociar el concepto teórico con el concepto visual. Dentro de este hexágono es donde se encuentra nuestro código base. Esta parte se llama dominio.

Cada lado de este hexágono representa una interacción con un servicio externo, por ejemplo: servicios http, base de datos, renderizado...

![arquitectura_hexagonal](arquitectura_hexagonal.png)

La comunicación entre el dominio y el resto de actores se realiza en la capa de infraestructura. En esta capa implementamos un código específico para cada una de estas tecnologías.

Una de las preguntas más recurrentes entre los profesionales que ven por primera vez esta arquitectura es: ¿Por qué un hexágono? Bueno, el uso de un hexágono es sólo una representación teórica. La cantidad de servicios que podríamos agregar es infinita y podemos integrar tantos como necesitemos.

## Mismo concepto diferentes nombres 😒

El patrón de arquitectura hexagonal también se denomina puertos y adaptadores. Este nombre surge de una separación dentro de una capa de **infraestructura**, donde tendremos dos subcapas:

- **Puerto**: Es la interfaz que nuestro código debe implementar para poder abstraerse de la tecnología. Aquí definimos las firmas de métodos que existirán. 

- **Adaptador:** Es la implementación de la propia interfaz. Aquí tendremos nuestro código específico para consumir una tecnología concreta. Es importante saber que esta implementación NO debe estar en nuestra aplicación, más allá de la declaración, ya que su uso se realizará a través del puerto.

Así, nuestro dominio realizará llamadas a la subcapa que corresponde al puerto, quedando desacoplado de la tecnología, mientras que el puerto, a su vez, consumirá el adaptador.

El concepto de Puertos y Adaptadores está muy ligado a la programación orientada a objetos y al uso de interfaces, y tal vez, la implementación de este patrón en la programación funcional pueda ser diferente al concepto inicial. De hecho, han surgido muchos patrones que iteran sobre esto, como la **Arquitectura Onion** o la **Arquitectura Limpia (Clean architeture)**. Al final el objetivo es el mismo: dividir nuestra aplicación en capas, separando dominio e infraestructura.

## ¿Cómo afecta la mantenibilidad? 🧐

El hecho de tener nuestro código separado en capas, donde cada una de ellas tiene una única responsabilidad, ayuda a que cada capa evolucione de diferentes maneras, sin impactar a las demás.

Además, con esta segmentación conseguimos una alta cohesión, donde cada capa tendrá una responsabilidad única y bien definida dentro del contexto de nuestro software.

## ¿Cómo afecta la interfaz? 😮

Actualmente existen una serie de falencias en el uso de metodologías a la hora de crear aplicaciones. Hoy en día, contamos con una increíble cantidad de herramientas que nos permiten desarrollar aplicaciones muy rápidamente y, al mismo tiempo, hemos dejado en un segundo plano el análisis y la implementación de arquitecturas conocidas y probadas.

Sin embargo, aunque estas arquitecturas puedan parecer del pasado, donde los lenguajes no evolucionaron tan rápido, estas arquitecturas han sido mostradas y adaptadas para brindarnos la escalabilidad que necesitamos para desarrollar aplicaciones reales.

## Contexto histórico 😴

Hace dos décadas las aplicaciones de escritorio eran la principal herramienta a desarrollar. En ellos toda nuestra aplicación estaba instalada en la máquina, a través de librerías, y había un alto acoplamiento entre vista y comportamiento. Luego, quisimos escalar nuestras aplicaciones para obtener un software más fácil de mantener, con bases de datos centralizadas. Muchos de ellos fueron migrados a un servidor. Con esto, nuestras aplicaciones de escritorio quedaron reducidas a aplicaciones "tontas", que no requerían acceso, persistencia ni muchos datos. Finalmente, si la aplicación necesitaba algunos datos, tenía la responsabilidad de realizar estas llamadas a los servidores externos a través de servicios de red. Es aquí cuando empezamos a distinguir entre "frontend" y "backend".

Durante los siguientes años tuvimos el boom web. Muchas aplicaciones de escritorio se adaptaron a los navegadores, donde las limitaciones eran mayores solo con HTML. Posteriormente JAVASCRIPT empezó a darle más posibilidades al navegador.

## Presente 😌

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

Primero pensamos en qué entidades entran en juego, sabiendo que recuperaremos los datos a través de un servicio de terceros (lo veremos más adelante).
- Producto
- Carrito de compras

También sabemos que estas entidades deben estar disponibles para el usuario, para que pueda interactuar con ellas. El usuario podría hacer lo siguiente:
- Ver una lista de productos
- Añadir productos al carrito de compras
- Eliminar productos del carrito de compras

Ahora imagine que tenemos las siguientes reglas comerciales:
- Un carrito de compras no puede tener más de 5 productos
- El mismo producto no puede estar en el carrito dos o más veces
- El precio máximo del carrito debe ser 100€

## Estructura de directorios 🗂️

Aquí podemos ver un ejemplo de cómo organizar los directorios, tanto para la aplicación "React" como para la aplicación "Vue".

![[directorios.png]]

Déjame explicarte un poco más qué representa cada carpeta.

- **domain (Dominio)**
	- **models (Modelos):**  Aquí tenemos los modelos que necesitaremos, tanto tipos como interfaces de cada modelo.
	- **repositories (Repositorios):** Todo tipo e interfaces relacionadas con los repositorios (un repositorio se encarga de traer datos de un servicio web, o una base de datos, o un archivo...).
	- **services (Servicios):** Un servicio se encarga de interactuar con nuestros modelos y realizar acciones sobre ellos. Por ejemplo para conseguir los productos o añadir un producto al carrito.
- **infrastructure (Infraestructura)**
	- **http**: Aquí se almacenan cosas relacionadas con nuestro cliente, en este caso un cliente http.
		- **dto:** Todos los dto que recibimos de un repositorio.
	- **instances (Instancias):** Aquí hemos creado instancias concretas para nuestro cliente y repositorios. Puede verse como el punto de entrada de su sistema. Quizás este no sea el mejor lugar para esta carpeta, la hemos creado de esta manera para utilizar datos falsos ya que no tenemos un servicio web.
	- **repositories (Repositorios):** Aquí definimos los repositorios que necesitamos para obtener productos.
	- **views (Vistas):** Esta carpeta almacena todo lo relacionado con nuestras vistas.
		- **react-ui:** Proyecto React que interactúa con nuestros modelos y servicios.
		- **reactnative-ui:** Proyecto React Native que interactúa con nuestros modelos y servicios.
		- **vue-ui**: Proyecto Vue que interactúa con nuestros modelos y servicios.
- **mocks:** Aquí tenemos datos simulados que nuestro cliente utilizará para proporcionar datos concretos.
- **test:** Todas las pruebas unitarias para los casos de uso.

Hemos creado dos directorios: dominio e infraestructura. Todos los componentes visuales se asignan dentro de la infraestructura (recuerde que las vistas y representaciones no pertenecen a nuestro dominio).

### Domain 🛡️

Ahora vamos a definir los modelos de dominio (Producto y Carrito) con las respectivas interfaces requeridas.

```ts
// src/domain/models/Product.ts

export type Product = {
    id: string;
    title: string;
    price: number;
};
```

```ts
// src/domain/models/Carts.ts

import { Product } from './Product';

// Esta interfaz define qué operaciones podemos realizar en un carrito.
export interface ICart {
    createCart: () => Cart;
    addProductToCart: (cart: Cart, product: Product) => Cart;
    removeProductFromCart: (cart: Cart, product: Product) => Cart;
}

export type Cart = {
    id: string;
    products: Product[];
};
```

Ahora definimos una funcionalidad que nos permite agregar y quitar un "Producto", teniendo en cuenta los requisitos y reglas del negocio.

Dependiendo del patrón de diseño que utilicemos, esta implementación puede ser ligeramente diferente. Para este caso utilizamos una opción sencilla, un módulo de servicio que maneja los datos.

```ts
// src/domain/services/Cart.service.ts

import { Cart, ICart } from '../models/Cart';
import { Product } from '../models/Product';

const createCart = (): Cart => {
    return { id: Date.now().toString(), products: [] };
};

const hasProduct = (cart: Cart, product: Product): boolean => {
    return !!cart.products.find(item => item.id === product.id);
};

const isCartFull = (cart: Cart): boolean => {
    return cart.products.length >= 5;
};

const isCartLimitPriceExceeded = (cart: Cart, product: Product, limit: number): boolean => {
    let totalPriceCart = 0;
    cart.products.forEach(item => {
        totalPriceCart += item.price;
    });
    totalPriceCart += product.price;

    return totalPriceCart > limit;
};

const addProductToCart = (cart: Cart, product: Product): Cart => {
    if (!hasProduct(cart, product) && !isCartFull(cart) && !isCartLimitPriceExceeded(cart, product, 100))
        cart.products = [...cart.products, product];
    return { ...cart };
};

const removeProductFromCart = (cart: Cart, product: Product): Cart => {
    const productsWithRemovedItem: Product[] = [];
    cart.products.forEach(item => {
        if (item.id !== product.id) productsWithRemovedItem.push(item);
    });
    cart.products = [...productsWithRemovedItem];
    return { ...cart };
};

// Este servicio debe implementar las operaciones definidas para la interfaz Cart.
export const cartService: ICart = {
    createCart,
    addProductToCart,
    removeProductFromCart
};
```

### Acceso a datos 📰

Además, necesitamos obtener una lista de productos. En la mayoría de los casos estos datos se obtienen de servicios http, pero también podríamos usar graphql o cualquier otra biblioteca. Además, dentro de http podríamos usar fetch, axios, xhr...

En cualquier caso, esto es parte de la capa de infraestructura y este objeto será consumido por una entidad de repositorio.

Primero, definimos la estructura de los datos devueltos por la API. Este tipo de datos se denomina "Objeto de transferencia de datos (DTO)":

```ts
// src/infrastructure/http/dto/ProductDTO.ts

export interface ProductDTO {
    id: string;
    title: string;
    description: string;
    price: number;
}
```

Además, hemos declarado qué métodos necesitamos implementar para http. Así, más adelante podremos utilizar nuestro cliente favorito (fetch, axios...) implementando esta interfaz:

```ts
// src/domain/repositories/Http.ts

export interface Http {
    get: <T>(path: string, params?: Record<string, any>, config?: any) => Promise<T | any>;
    post: <T>(path: string, params?: Record<string, any>, config?: any) => Promise<T | any>;
    put: <T>(path: string, params?: Record<string, any>, config?: any) => Promise<T | any>;
    delete: <T>(path: string, params?: any, config?: any) => Promise<T | any>;
}
```

Nuestro cliente, que será una instancia que implemente la interfaz `Http`, será inyectado como una dependencia a nuestro repositorio. Así, en cualquier momento, podremos cambiar de cliente al instante. Esta técnica se llama inyección de dependencia y, aunque no es muy común en JavaScript, es muy poderosa y está ahí para usarse. Typecript nos lo pone muy fácil.

Para este ejemplo, hemos creado dos clientes (envoltorios), uno que usa axios y el otro devolverá datos simulados.

#### Cliente para axios

```ts
// src/infrastructure/instances/httpAxios.ts

import axios from 'axios';
import { Http } from '../../domain/repositories/Http';

const headers = {
    'Content-Type': 'application/json'
};

export const httpAxios: Http = {
    get: async <T>(path: string, params?: Record<string, any>, config?: any) => {
        const response = await axios.get(path, { ...config, params: params, headers });
        return response.data as T;
    },
    post: async <T>(path: string, params?: Record<string, any>, config?: any) => {
        const response = await axios.post(path, { ...params }, { ...config, headers });
        return response.data as T;
    },
    put: async <T>(path: string, params?: Record<string, any>, config?: any) => {
        const response = await axios.put(path, { ...params }, { ...config, headers });
        return response.data as T;
    },
    delete: async <T>(path: string, params?: any, config?: any) => {
        const response = await axios.delete(path, { ...config, params: params, headers });
        return response.data as T;
    }
};
```

#### Cliente para datos falsos

```ts
// src/infrastructure/instances/httpAxios.ts

import { Http } from '../../domain/repositories/Http';
import { productListMock } from '../../mocks/products';

export const httpFake: Http = {
    get: async <T>(path: string, params?: Record<string, any>, config?: any) => {
        const response = await productListMock;
        return response;
    },
    post: async <T>(path: string, params?: Record<string, any>, config?: any) => {
        const response = await productListMock;
        return response;
    },
    put: async <T>(path: string, params?: Record<string, any>, config?: any) => {},
    delete: async <T>(path: string, params?: any, config?: any) => {}
};
```

De esta manera, cuando quieras cambiar el cliente y usar, por ejemplo, fetch en lugar de axios, puedes crear un nuevo contenedor http que implemente la interfaz para http, pero usando la biblioteca fetch. ¡Fácil!

Para finalizar esta parte, necesitamos una última cosa. Tenemos que crear un repositorio para productos dentro de la infraestructura. Este repositorio maneja la solicitud y la transformación de los datos de respuesta a nuestro modelo de dominio.

```ts
// src/infrastructure/repositories/productRepository.ts

import { Product } from '../../domain/models/Product';
import { ProductRepository } from '../../domain/repositories/ProductRepository';
import { Http } from '../../domain/repositories/Http';
import { ProductDTO } from '../../infrastructure/http/dto/ProductDTO';

export const productRepository = (client: Http): ProductRepository => ({
    getProducts: async () => {
        const products = await client.get<ProductDTO>('');
        return products.map((productDto): Product => ({ id: productDto.id, title: productDto.title, price: productDto.price }));
    },

    getProductsById: async id => {
        const products = await client.get<ProductDTO>('', { id });
        return products.map((productDto): Product => ({ id: productDto.id, title: productDto.title, price: productDto.price }));
    }
});
```

Como puede ver, `productRepository` es una función que recibe un cliente como parámetro (solo aquí está la inyección de dependencia).

Para mayor comodidad, hemos creado un repositorio falso que implementa la interfaz `ProductRepository`.

**Recuerde que el código de producción no debe contener ninguna referencia a datos falsos o simulados. Lo estamos utilizando para hacer que este proyecto sea funcional. Por este motivo, en producción debes olvidar la carpeta de `instancias`.**

### Vistas 📱 💻

La vista y capa para acceder a los datos están en infraestructura. Sin embargo, no deben comunicarse directamente. Vamos a crear un nuevo servicio para consumir nuestro repositorio, por lo que estos datos estarán disponibles para el resto de nuestra aplicación.

```ts
// src/domain/services/ProductService.ts

import { ProductRepository } from '../repositories/ProductRepository';

// Aquí podemos cambiar el repositorio por uno que implemente la interfaz IProductRepository
//const repository: IProductRepository = productRepository;

export const productService = (repository: ProductRepository): ProductRepository => ({
    getProducts: () => {
        return repository.getProducts();
    },
    getProductsById: id => {
        return repository.getProductsById(id);
    }
});
```

Al igual que con nuestro cliente `Http`, utilizamos la inyección de dependencia en nuestro servicio, que recibe un repositorio como parámetro. De esta forma podremos cambiar de repositorio en cualquier momento. (Quizás en el futuro tengamos que obtener los productos de una base de datos local en lugar de un servicio de descanso).

---

Bueno, ahora hemos definido cómo obtener los datos y la funcionalidad que necesitamos para agregar/eliminar elementos en nuestro carrito.

Independientemente de si usas react o vue, ten en cuenta que el código escrito hasta ahora es común para ambas aplicaciones, por lo que la llamada a nuestros métodos desde nuestro componente será la misma.

Dentro de la carpeta de vistas hemos creado tantos proyectos como hemos necesitado. En nuestro caso tenemos react, vue y react native.

Lo primero que vamos a hacer es definir el estado inicial y las funciones para manejar el estado del carrito.

#### React

```tsx
// src/infrastructure/views/react-ui/src/App.tsx

import React, { useState } from 'react';
import { ProductList } from './views/ProductList';
import { Cart } from '@domain/models/Cart';
import { Product } from '@domain/models/Product';
import { cartService } from '@domain/services/CartService';

const App = () => {
    const [cart, setCart] = useState<Cart>(cartService.createCart());

    const handleAddToCart = (product: Product) => {
        setCart(cartService.addProductToCart(cart, product));
    };

    const handleRemoveToCart = (product: Product) => {
        setCart(cartService.removeProductFromCart(cart, product));
    };

    const renderCartProducts = (): JSX.Element[] => {
        const cartProducts: JSX.Element[] = [];
        let totalCart = 0;

        cart.products.forEach(product => {
            totalCart += product.price;
            cartProducts.push(
                <div key={product.id}>
                    <label>{product.title} </label>
                    <span>({product.price} €) </span>
                    <button onClick={() => handleRemoveToCart(product)}>remove</button>
                    <br />
                </div>
            );
        });

        cartProducts.push(
            <div key={'total'}>
                <br />
                <label>
                    <b>Total:</b>
                </label>
                <span>{totalCart} €</span>
                <br />
            </div>
        );
        return cartProducts;
    };

    return (
        <div>
            <h1>Shopping cart</h1>
            <h2>Products in the cart</h2>
            {renderCartProducts()}
            <ProductList onSelectProduct={handleAddToCart} />
        </div>
    );
};

export default App;
```

#### Vue

```vue
<!--
// src/infrastructure/views/vue-ui/src/App.vue
-->
<template>
    <div id="app">
        <h1>Shopping cart</h1>
        <h2>Products in the cart</h2>
        <div v-for="product in cart.products" :key="product.id">
            <label>{{ product.title }} </label>
            <span>({{ product.price }} €) </span>
            <button @click="handleRemoveProductFromCart(product)">remove</button>
            <br />
        </div>
        <div>
            <br />
            <label>
            <b>Total:</b>
            </label>
            <span>{{ getTotalCart() }} €</span>
            <br />
        </div>
        <ProductList @onSelectProduct="handleAddProductToCart" />
    </div>
</template>

<script lang="ts">
import { Product } from '@/domain/models/Product';
import ProductList from '@/infrastructure/views/ProductList.vue';
import { cartService } from '@/domain/services/Cart.service';
import { Cart } from '@/domain/models/Cart';
type DataProps = {
    cart: Cart;
};
export default {
    components: {
        ProductList
    },
    data(): DataProps {
        return {
            cart: cartService.createCart()
        };
    },
    mounted() {
        this.cart = cartService.createCart();
    },
    methods: {
        handleAddProductToCart(product: Product) {
            this.cart = cartService.addProductToCart(this.cart, product);
        },
        handleRemoveProductFromCart(product: Product) {
            this.cart = cartService.removeProductFromCart(this.cart, product);
        },
        getTotalCart() {
            let totalCart = 0;
            this.cart.products.forEach(product => {
                totalCart += product.price;
            });
            return totalCart;
        }
    }
};
</script>
```

#### React native

```tsx
// src/infrastructure/views/reactnative-ui/App.tsx

import React, { useState } from 'react';
import { SafeAreaView, StyleSheet, ScrollView, View, Text, StatusBar, Button } from 'react-native';

import { Colors } from 'react-native/Libraries/NewAppScreen';
import { Cart } from '@domain/models/Cart';
import { cartService } from '@domain/services/CartService';
import { Product } from '@domain/models/Product';
import { ProductList } from '@/components/ProductList';

const App = () => {
    const [cart, setCart] = useState<Cart>(cartService.createCart());

    const handleAddToCart = (product: Product) => {
        setCart(cartService.addProductToCart(cart, product));
    };

    const handleRemoveToCart = (product: Product) => {
        setCart(cartService.removeProductFromCart(cart, product));
    };

    const renderCartProducts = (): JSX.Element[] => {
        const cartProducts: JSX.Element[] = [];
        let totalCart = 0;

        cart.products.forEach(product => {
            totalCart += product.price;
            cartProducts.push(
                <View style={styles.productInCart} key={product.id}>
                    <Text>{product.title} </Text>
                    <Text>({product.price} €) </Text>
                    <Button color={'red'} onPress={() => handleRemoveToCart(product)} title={'remove'} />
                </View>
            );
        });

        cartProducts.push(
            <View style={styles.productInCart} key={'total'}>
                <Text>Total:</Text>
                <Text> {totalCart} €</Text>
            </View>
        );
        return cartProducts;
    };

    return (
        <>
            <StatusBar barStyle='default' />
            <SafeAreaView>
                <ScrollView contentInsetAdjustmentBehavior='automatic' style={styles.scrollView}>
                    <Text style={styles.titlePage}>Shopping cart</Text>
                    <Text style={styles.title}>Products in the car</Text>
                    {renderCartProducts()}
                    <ProductList onSelectProduct={handleAddToCart} />
                </ScrollView>
            </SafeAreaView>
        </>
    );
};

const styles = StyleSheet.create({
    scrollView: {
        backgroundColor: Colors.lighter
    },
    titlePage: {
        fontWeight: 'bold',
        margin: 5,
        fontSize: 20
    },
    title: {
        fontWeight: 'bold',
        margin: 5
    },
    productInCart: {
        flexDirection: 'row',
        flex: 1,
        alignContent: 'center',
        alignItems: 'center',
        margin: 10
    }
});

export default App;
```

Vamos a mostrar la lista de productos.

#### React

```tsx
// src/infrastructure/views/react-ui/src/views/ProductList.tsx

import React, { useCallback } from 'react';
import { Product } from '@domain/models/Product';
import { productService } from '@domain/services/ProductService';
import { productRepositoryFake } from '@infrastructure/instances/productRepositoryFake';

interface ProductListProps {
    onSelectProduct: (product: Product) => void;
}

export const ProductList: React.FC<ProductListProps> = ({ onSelectProduct }) => {
    const [products, setProducts] = React.useState<Product[]>([]);

    const getProducts = useCallback(async () => {
        try {
            const responseProducts = await productService(productRepositoryFake).getProducts();
            setProducts(responseProducts);
        } catch (exception) {
            console.error(exception);
        }
    }, []);

    React.useEffect(() => {
        getProducts();
    }, []);

    const handleSelectProduct = (product: Product) => {
        onSelectProduct(product);
    };

    return (
        <div>
            <h2>List of products</h2>
            <ul>
                {products.map(product => (
                    <li key={product.id}>
                        <button
                            onClick={() => {
                                handleSelectProduct(product);
                            }}
                        >
                            {product.title}
                        </button>
                    </li>
                ))}
            </ul>
        </div>
    );
};
```

#### Vue

```vue
<!--
// src/infrastructure/views/vue-ui/src/views/ProductList.vue
-->
<template>
    <div>
        <h2>List of products</h2>
        <ul>
            <li v-for="product in products" :key="product.id">
                <button @click="handleSelectProduct(product)">{{ product.title }}</button>
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
import { productService } from '@domain/services/ProductService';
import { Product } from '@domain/models/Product';
import { productRepositoryFake } from '@infrastructure/instances/productRepositoryFake';
type DataProps = {
    products: Product[];
};
export default {
    name: 'ProductList',
    data(): DataProps {
        return {
            products: []
        };
    },
    mounted() {
        productService(productRepositoryFake)
            .getProducts()
            .then(response => (this.products = response));
    },
    methods: {
        handleSelectProduct(product: Product) {
          this.$emit('onSelectProduct', product);
        }
    }
};
</script>
```

#### React native

```tsx
// src/infrastructure/views/reactnative-ui/src/components/ProductList.tsx

import React, { useCallback, useState } from 'react';
import { StyleSheet, View, Text, Button } from 'react-native';

import { Product } from '@domain/models/Product';
import { productService } from '@domain/services/ProductService';
import { productRepositoryFake } from '@infrastructure/instances/productRepositoryFake';

interface ProductListProps {
    onSelectProduct: (product: Product) => void;
}

export const ProductList: React.FC<ProductListProps> = ({ onSelectProduct }) => {
    const [products, setProducts] = useState<Product[]>([]);

    const getProducts = useCallback(async () => {
        try {
            const responseProducts = await productService(productRepositoryFake).getProducts();
            setProducts(responseProducts);
        } catch (exception) {
            console.error(exception);
        }
    }, []);

    React.useEffect(() => {
        getProducts();
    }, []);

    const handleSelectProduct = (product: Product) => {
        onSelectProduct(product);
    };

    return (
        <View>
            <Text style={styles.title}>List of products</Text>
            <View>
                {products.map(product => (
                    <View style={styles.buttonProduct} key={product.id}>
                        <Button
                            onPress={() => {
                                handleSelectProduct(product);
                            }}
                            title={product.title}
                        >
                            <Text>{product.title}</Text>
                        </Button>
                    </View>
                ))}
            </View>
        </View>
    );
};

const styles = StyleSheet.create({
    title: {
        fontWeight: 'bold',
        margin: 5
    },
    buttonProduct: {
        margin: 5
    }
});
```

Es interesante la gestión del estado de los elementos del carrito, pero es algo relacionado con la visualización de datos y esta gestión pertenece a la tecnología que estemos usando (React o Vue).

Además, si prestamos atención al código anterior, podemos ver que todo nuestro código está desacoplado. Nuestra capa de dominio puede ser utilizada por reaccionar, vue y reaccionar nativo.

Además, aquí podemos ver la inyección de dependencia en acción. Usamos el servicio del producto, que recibe un repositorio `ProductRepository` específico que, a su vez, recibe un cliente `Http` específico.

### Bibliotecas de terceros 🛠️

