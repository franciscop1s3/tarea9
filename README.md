# tarea9
Algunas arquitecturas y biblioteca EventBus

# Model View Presenter

El MVP (Model View Presenter) es un patrón derivado del conocido MVC (Model View Controller), que de un tiempo a esta parte está cobrando gran importancia en el desarrollo de aplicaciones Android. Cada vez hay más gente hablando del tema, pero sin embargo muy poca información fiable y estructurada. Es por eso que quería aprovechar este blog para incitar el debate y que todos aportemos nuestros conocimientos para aplicarlo de la mejor manera posible a nuestros proyectos.

## ¿Qué es el MVP?
El patrón MVP permite separar la capa de presentación de la lógica de la misma, de tal forma que todo lo relacionado con cómo funciona la interfaz queda separado del cómo representarlo en pantalla. Idealmente el patrón MVP permitiría conseguir que una misma lógica pudiera tener vistas totalmente diferentes e intercambiables.
Lo primero a tener claro es que el MVP no es un patrón de arquitectura de aplicaciones, sólo se encarga de la capa de presentación. En cualquier caso siempre es mejor usarlo para la arquitectura que no usarlo en absoluto.

## ¿Por qué usar MVP?
En Android tenemos un problema derivado del hecho de que las actividades en Android están íntimamente acopladas tanto con la interfaz como con las mecánicas de acceso a datos. Hasta tal punto que existen clases como el CursorAdapter, que mezclan los adaptadores, que son parte de la vista, con los cursores, algo que debería estar relegado a lo más profundo de la capa de acceso a datos.
Para que una aplicación sea fácilmente extensible y mantenible necesita tener bien separadas sus capas. ¿Qué hacemos si mañana en vez de tirar de base de datos necesitamos hacerlo de un servicio en la red? Tendríamos que rehacer toda la vista.
El MVP independiza la vista de la forma de conseguir esos datos. Nos divide la aplicación en, al menos, tres capas distintas, pudiendo además testear cada una de ellas de forma independiente.

## ¿Cómo implementar MVP en Android?
Pues aquí es donde todo comienza a ser más difuso. Hay muchas variaciones de MVP y cada uno puede ajustar la idea del patrón a sus necesidades y a la forma en que se sienta más cómodo. El patrón varía sobre todo en función de la cantidad de responsabilidades que deleguemos en el presentador.

¿Es la vista la que tiene que activar o desactivar una barra de progreso, o se debe encargar el presentador? ¿Y quién decide qué botones deben pintarse en la Action Bar? Ahí es donde comienzan las decisiones difíciles. Yo os mostraré cómo suelo trabajar, pero quiero que este artículo sea más un foro para el debate que unas guías estrictas de cómo aplica el MVP, porque a día de hoy no existe una forma estandarizada de implementarlo.

### El presentador
El presenter se encarga de actuar de intermediario entre la vista y el modelo. Recupera los datos del modelo y se los devuelve a la vista formateados. Pero a diferencia del MVC típico, también decide qué ocurre cuando se interactúa con la vista.

### La vista
La vista, habitualmente implementada por una Activity (aunque puede ser un fragment, una View… según como se estructure la App), contendrá una referencia al presenter. Lo ideal sería inyectárselo a la actividad mediante Dagger, pero en el caso de no usarlo, será la vista la encargada de crear el objeto presentador. Lo único que hará la vista será llamar a un método del presenter cada vez que se realice una acción sobre la interfaz, normalmente el pulsado de un botón, un elemento de una lista, etc.

### El modelo
En una aplicación con una buena arquitectura por capas, este modelo no sería más que la puerta de enlace a la capa de dominio o de lógica de negocio. Si estuviéramos utilizando la clean architecture de Uncle Bob, seguramente el modelo sería un interactor que implemente algún caso de uso. Pero este es otro tema que me gustaría tratar en futuros artículos. Por ahora es suficiente con verlo como el proveedor de los datos que queremos mostrar en la vista.

### Un ejemplo
Como es un poco extenso de explicar, he creado un Github con un ejemplo consistente en una pantalla de login que valida los datos y que permite acceder a una home con un listado de datos que se recuperan desde el modelo. En este artículo no explicaré nada de código porque es bastante simple, pero si veis que no queda claro puedo crear otro artículo explicándolo en detalle.


# La Arquitectura Clean


Surge con la problemática que en la mayoría de los desarrollos en los cuales existe un framework se encuentra más código que lo representa, en lugar del problema que está resolviendo, es decir la idea es centrarse en lo que tiene que hacer la aplicación como tal y es por ello que dice que debemos de pensar en él framework como el mecanismo de paso de mensajes hacia nuestro software, el cual se debe mirar como una herramienta o plugin de lo que queremos realizar.

El autor dice en varias de sus charlas que más que una arquitectura sólo son una serie de condiciones que se deben cumplir para que una arquitectura de considere ”clean”, solo son una serie de reglas que lo único que van a hacer es dividir el software en capas.

![alt text](https://erikcaffrey.github.io/content/images/2016/1/clean_archi.png)

Antes de comenzar a describir los componentes importantes del esquema debes tener en cuenta que no es obligatorio ni mucho menos mandatorio usar solo 4 círculos ya que de acuerdo a Uncle Bob solo son esquemáticos y quizás tu problema necesita de más para poder ser resuelto.

Lo único que nos dice es que hay que respetar la Dependency Rule: “El código fuente sólo puede apuntar hacia adentro“.

## Entidades
Son aquellos objetos que van a representar los actores importantes de la lógica de negocio de nuestra aplicación.

## Use Casos
Están muy relacionados con las entidades y son los encargados de implementar lógica de negocio de nuestra aplicación, ya que van a orientar todas aquellas interacciones del flujo de datos y entidades dejando al framework fuera de todo esto (en nuestro caso el sdk android), también se conocen como interactores.

(En ingeniería de software un caso de uso se define como: “Una secuencia de interacciones que se encargan de modelar alguna acción que quiero que mi software realice”)

## Adaptadores de Interfaz
Convierten los datos en el formato más conveniente para los casos de uso y entidades. (De manera recurrente aqui encontraras controladores y presentadores.)

## Frameworks and Drivers
Aquí es donde residen los detalles y todo ese conjunto de plataformas externas y herramientas como puede ser la UI, Web, DB , Devices, etc. Generalmente solo se deben comunicar con el siguiente círculo a su interior.

## Android Clean Architecture
![alt text](https://erikcaffrey.github.io/content/images/2016/1/android_archi.png)

Este esquema es una representación de cómo se aplica el Clean Architecture en una aplicación android ya que gracias a la gran participación de muchos desarrolladores de la comunidad android alrededor del mundo se ha ido mejorando en estos últimos meses ya que se le han agregado dos componentes importantes el primero un patrón en la capa de presentación que puede ser MVP, MVVM, MVC o el que prefieras y el segundo se ha agregado el Repository Pattern en la capa de datos para abstraer el origen de datos y destacar que estos patrones no están dentro de la arquitectura que Uncle Bob describe, es trabajo de una comunidad con el único objetivo de mejorar el desarrollo de nuestras aplicaciones android y si estoy seguro que te encontraras diferentes implementaciones porque realmente no hay un camino a seguir pero particularmente es la que más me gusta y es la que se ha adaptado a mis problemas.


## Presentation Layer
Es la capa en donde ocurre todo lo relacionado a cómo funcionan la vistas normalmente activities y fragmentos los cuales contienen lógica pero únicamente enfocada en cómo funciona la vistas es decir el que debo mostrarle al usuario y cuando debe hacerlo. Para lograr un mejor manejo de vistas en esta capa se suelen usar patrones de UI (en el ejemplo que he escrito encontrarás MVP).

Yo he escrito un post acerca del MVP y otro sobre MVVM si solo has utilizado MVC dale una mirada a estos posts por que te ayudarán a separar tus vistas de la lógica de negocio de una forma elegante.

## Domain Layer
Toda la lógica de negocio ocurre en esta capa aquí solo lo enfocado a que tiene que hacer nuestra aplicación. Se encuentran entidades y casos de uso regularmente es solo código java (puede ser kotlin o scala es indiferente) y no hay ninguna dependencia android.

## Data Layer
Es la capa en donde se obtienen todos los datos que necesita nuestra aplicación para funcionar y los datos pueden ser de proveídos por una base de datos, de la red o de la memoria o de donde nos imaginemos particularmente en android hacemos uso de Repository Pattern un patrón que permite abstraer el origen de datos en donde no va importar de donde vengan los datos lo importante es que seran obtenidos de algún lugar y podremos utilizarlos para hacer las acciones que tengamos que hacer.

Algo que valdría la pena mencionar es que cada capa tiene su propio modelo de datos ya que será más fácil de testar y modificar para lo que estaremos usamos mappers para estar cambiando objetos de datos a un objeto de dominio y de dominio a un objeto de nuestra vista no quiero indagar mucho aquí pero cuando veas el código te darás cuenta de lo útil que es.

## Benefios de usar arquitectura clean
Existe una cantidad muy grande de diversas arquitecturas algunas en las cuales Uncle Bob se ha basado son Hexagonal Architecture, Onion Architecture, Screaming Architecture y lo importante es que cada una de ellas varía en sus detalles de implementación pero todas son similares y tienen el mismo objetivo separar en capas nuestro software con la finalidad de darle flexibilidad.

Las principales aportaciones que esta arquitectura seguirá motivando son:

Independiente de frameworks
Testable
Independendiente de nuestra UI
Independendiente de nuestra DB
Independendiente de cualquier herramienta externa
