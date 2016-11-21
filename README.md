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

#Biblioteca EventBus
![alt text](https://cms-assets.tutsplus.com/uploads/users/362/posts/22694/final_image/eve1.png)

## El respeto de la original:
## Descargar el Código:
En el proceso de programación, Cuando pensamos en la notificación de otros componentes determinados cuando sucedió., Los que suelen utilizar el modo de Observación, El observador de modo formal como muy frecuentes, Así que nos ha ayudado en llaves en cuenta de modo de observador, Simplemente algún tipo de modelo de la sucesión puede rápida observación, EventBus del repositorio de código fuente abierto también tiene una función similar en Androide, Puede ayudarnos a alcanzar el modo de vista, Entonces podremos empezar a aprender cómo usarEventBus.
En el siguiente contenido, quiero, en primer lugar, se describe cómo usar EventBus, de principio a fin de lograr aprender luego un simple EventBus demersales, porque sólo aprender cómo usar siempre sentía suficiente certidumbre, en caso de que algún día un bug también empezar, sabemos que después de la realización de la base, se con facilidad.- No digas tonterías, ahora empieza a aprender.

1, Descargar EventBus biblioteca: 
La Dirección de la descarga de EvnetBus: https://github.com/greenrobot/EventBus.git
2, Se puede libs EventBus.jar en su directorio de Ingeniería
3, Un caso de la definición, el caso una vez que se EventBus distribuido es algo que sucede, esta cosa es una observación de los acontecimientos de interés (No necesitamos que el heredero de cualquier clase)
4, El observador de la definición, el observador de registro a EventBus
5, Por EventBus distribuir dicho incidente, los observadores de algo que pasó
6, Después de la utilización de los EventBus registrada en contra de Observación. 

Conoce a un amigo de Observadores de modo seguro para el proceso muy familiar, pero, de hecho, a modo de observación básica es la misma.Pero también hay diferencias.En el modo de observador, todos los observadores de la necesidad de lograr una interfaz, que hay un método unificado como:
public void onUpdate(), 
Y cuando un incidente, los métodos de Observación de los onUpdate llamadas de un objeto a la observación de que algo pasó, pero no en EventBus en la necesidad de que, en la realización de EventBus es así: 
El observador en EventBus normalmente en función de cuatro tipos de suscripciones (que suceda algo llamado método)
1, onEvent
2, onEventMainThread
3, onEventBackground
4, onEventAsync
Estos cuatro tipos de suscripción de funciones son de uso enevento comienzo, su función es ligeramente diferente, en la introducción de antes de la introducción de dos conceptos diferentes: 
En caso de que los observadores EventBus.post pasó a través de la función de ejecución, este proceso es el llamado caso de Liberación, los observadores han dicho que la recepción llamado se caso, es de aplicación a través de la función de la suscripción de los siguientes. 


onEvent:Si se utiliza como función enevento suscripción, entonces el caso en qué hilo enevento será liberado, en este hilo en la operación, es decir, la liberación de los acontecimientos y recibir el caso con hilo en un hilo.Cuando se utiliza este método, en forma de ejecución de la operación no enevento en tiempo de ejecución de la operación, si el tiempo de retraso a causa de la distribución de los acontecimientos de fácil. 
onEventMainThread:Si el uso de onEventMainThread como la suscripción de la función, Entonces no importa en qué caso es liberado en el hilo, OnEventMainThread realizar en la industria de la Unión hilo, Va a recibir en la ejecución de un incidente en la interfaz de usuario, Esta en androide es muy útil, Porque en androide sólo en la industria de la Unión hilo con la interfaz de usuario, Así que no es de aplicación en el tiempo de la operación en el método de onEvnetMainThread. 
onEvnetBackground:Si el uso de onEventBackgrond como la suscripción de la función, entonces si el caso es en la industria de la Unión hilo liberado, entonces onEventBackground estará en funcionamiento en hilo, si el caso es el hilo en la liberación, la función tan onEventBackground directamente en el hilo de la ejecución. 
onEventAsync: Utilice esta función como la suscripción de la función, independientemente de que los acontecimientos en el hilo de la publicación, se crea un nuevo niño hilo en la ejecuciónonEventAsync.

A continuación a través de un ejemplo de la utilización de este par de suscripción para ver la función.
1, La definición de caso: 
```javascript
public class AsyncEvent
{
  private static final String TAG = "AsyncEvent";
  public String msg;
  public AsyncEvent(String msg)
  {
    this.msg=msg;
  }
}
```

Otros acontecimientos que no me han informado, voy a dar un código descargado. 
2, La creación de observadores (también llamado abonado), y la aplicación de métodos de suscripción
```javascript
public class MainActivity extends Activity
{

  @Override
  protected void onCreate(Bundle savedInstanceState)
  {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    //El registro de EventBus
    EventBus.getDefault().register(this);
  }

  // click--------------------------------------start----------------------
 
  
  public void methodPost(View view)
  {
    Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
    EventBus.getDefault().post(new PostEvent("PostEvent"));
  }
  
  public void methodMain(View view)
  {
    Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
    EventBus.getDefault().post(new MainEvent("MainEvent"));
  }
  
  public void methodBack(View view)
  {
    Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
    EventBus.getDefault().post(new BackEvent("BackEvent"));
  }
  
  public void methodAsync(View view)
  {
    Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
    EventBus.getDefault().post(new AsyncEvent("AsyncEvent"));
  }
  
  public void methodSubPost(View view)
  {
    new Thread()
    {
      public void run() {
        Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
        EventBus.getDefault().post(new PostEvent("PostEvent"));
      };
    }.start();
   
  }
  
  public void methodSubMain(View view)
  {
    new Thread()
    {
      public void run() {
        Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
        EventBus.getDefault().post(new MainEvent("MainEvent"));
      };
    }.start();
    
  }
  
  public void methodSubBack(View view)
  {
    new Thread()
    {
      public void run() {
        Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
        EventBus.getDefault().post(new BackEvent("BackEvent"));
      };
    }.start();
   
  }
  
  public void methodSubAsync(View view)
  {
    new Thread()
    {
      public void run() {
        Log.d("yzy", "PostThread-->"+Thread.currentThread().getId());
        EventBus.getDefault().post(new AsyncEvent("AsyncEvent"));
      };
    }.start();
   
  }
  
  
  //click--------------------end------------------------------
  
  
  //Event-------------------------start-------------------------------
  /**
   * El uso de enevento para recibir el caso, entonces la recepción y distribución de los acontecimientos en el caso de un hilo
   * @param event
   */
  public void onEvent(PostEvent event)
  {
    Log.d("yzy", "OnEvent-->"+Thread.currentThread().getId());
  }
  
  /**
   * El uso de onEventMainThread para recibir el caso, entonces el caso de distribución, independientemente de en qué hilos, recibir siempre la ejecución en caso de hilo, 
   * Para la aplicación de este androide es muy importante
   * @param event
   */
  public void onEventMainThread(MainEvent event)
  {
    Log.d("yzy", "onEventMainThread-->"+Thread.currentThread().getId());
  }
  
  /**
   * El uso de onEventBackgroundThread para recibir el caso, si el caso de distribución en hilo, entonces recibe el caso directamente en el mismo hilo
   * Corre, si la distribución de los acontecimientos en la industria de la Unión de hilo, entonces puede empezar un hilo de recibir el caso
   * @param event
   */
  public void onEventBackgroundThread(BackEvent event)
  {
    Log.d("yzy", "onEventBackgroundThread-->"+Thread.currentThread().getId());
  }
  /**
   * El uso de onEventAsync recibir los acontecimientos, tanto en caso de distribución (UI o hilo) Qué hilos de ejecución, además de recibir y realizar en un hilo
   * @param event
   */
  public void onEventAsync(AsyncEvent event)
  {
    Log.d("yzy", "onEventAsync-->"+Thread.currentThread().getId());
  }
  //Event------------------------------end-------------------------------------

  
  @Override
  protected void onDestroy()
  {
    //Cancelar la inscripción EventBus
    EventBus.getDefault().unregister(this);
    super.onDestroy();
  }
}
```

Nota: a EvnetBus registrada en el uso de métodos de EventBus.register abonado, cancelar el uso de métodos de EvnetBus.unregister abonado, algo que el método de notificación de los abonados, EventBus.post uso específico de la llamada, puede ver el ejemplo anterior. En el ejemplo anterior he creado 4 en el caso de hilo, respectivamente, y en el post en cuatro incidentes y puesto en hilo en cuatro incidentes, y luego cuatro hilos de suscripción, respectivamente, en función de impresiónid.

EventBusEl uso de aquí, es muy simple, pero si sobre la base de este principio de la comprensión EvnetBus, entonces podremos EventBus uso muy relajado. 
Desde la entrada EvnetBus empezó a ver.: EventBus.register

```javascript
public void register(Object subscriber) {
        register(subscriber, DEFAULT_METHOD_NAME, false, 0);
    }
```

De hecho, es el nombre de la función de registro de llamadas, cuatro de los parámetros de su significado, respectivamente,: 
subscriber: Es un registro de los abonados, 
MethodName: es el nombre de la función de la suscripción de los abonados de forma predeterminada, es, de hecho,"onEvent"
Adhesiva: indica si es coherente y, en general, de forma predeterminada es falso, a menos que el método de la llamada registerSticky
priority: Dijo que la prioridad de los acontecimientos, de forma predeterminada., 
Entonces podremos ver la función concreta de qué

```javascript
private synchronized void register(Object subscriber, String methodName, boolean sticky, int priority) {
        List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriber.getClass(),
                methodName);
        for (SubscriberMethod subscriberMethod : subscriberMethods) {
            subscribe(subscriber, subscriberMethod, sticky, priority);
        }
    }

```

A través de unfindSubscriberMethodsLa solución que se ha encontrado todos los métodos de suscripciones en un abonado, devuelve un *List<SubscriberMethod>*, 进入到findSubscriberMethodsMira cómo lograr

```javascript
List<SubscriberMethod> findSubscriberMethods(Class<?> subscriberClass, String eventMethodName) {
		//A través de los abonados a la clase + + quot;. & quot; & quot; onEvent" crear una clave
        String key = subscriberClass.getName() + '.' + eventMethodName;
        List<SubscriberMethod> subscriberMethods;
        synchronized (methodCache) {
			//Determinar si la caché de volver directamente de la caché
            subscriberMethods = methodCache.get(key);
        }
		//La primera vez subscriberMethods es nulo.
        if (subscriberMethods != null) {
            return subscriberMethods;
        }
        subscriberMethods = new ArrayList<SubscriberMethod>();
        Class<?> clazz = subscriberClass;
        HashSet<String> eventTypesFound = new HashSet<String>();
        StringBuilder methodKeyBuilder = new StringBuilder();
        while (clazz != null) {
            String name = clazz.getName();
			//Sistema de filtro de clase
            if (name.startsWith("java.") || name.startsWith("javax.") || name.startsWith("android.")) {
                // Skip system classes, this just degrades performance
                break;
            }

            // Starting with EventBus 2.2 we enforced methods to be public (might change with annotations again)
			//Por reflejo, a todos los métodos de adquisición de los abonados
            Method[] methods = clazz.getMethods();
            for (Method method : methods) {
                String methodName = method.getName();
				//Sólo busca el método enevento comienzo
                if (methodName.startsWith(eventMethodName)) {
                    int modifiers = method.getModifiers();
					//El juicio de los abonados si es público, y si los modificadores, parece que sólo es público, y no puede ser estático modificación final, etc.
                    if ((modifiers & Modifier.PUBLIC) != 0 && (modifiers & MODIFIERS_IGNORE) == 0) {
						//Los parámetros de la función de una suscripción
                        Class<?>[] parameterTypes = method.getParameterTypes();
						//Mira el número de parámetros de sólo 1
                        if (parameterTypes.length == 1) {
							//La parte de atrás de obtener enevento
                            String modifierString = methodName.substring(eventMethodName.length());
                            ThreadMode threadMode;
                            if (modifierString.length() == 0) {
								//La suscripción de la función de onEvnet
								//Registro de hilo de modelo para PostThread, de importancia es la liberación de los acontecimientos y la recepción de un hilo en caso de ejecución con detalle, puede consultar mi suscripción a cuatro puntos diferentes funciones de análisis
                                threadMode = ThreadMode.PostThread;
                            } else if (modifierString.equals("MainThread")) {
								//OnEventMainThread correspondiente
                                threadMode = ThreadMode.MainThread;
                            } else if (modifierString.equals("BackgroundThread")) {
								//OnEventBackgrondThread correspondiente
                                threadMode = ThreadMode.BackgroundThread;
                            } else if (modifierString.equals("Async")) {
								//OnEventAsync correspondiente
                                threadMode = ThreadMode.Async;
                            } else {
                                if (skipMethodVerificationForClasses.containsKey(clazz)) {
                                    continue;
                                } else {
                                    throw new EventBusException("Illegal onEvent method, check for typos: " + method);
                                }
                            }
							//Para obtener el tipo de parámetro, de hecho, es el tipo de la recepción de los acontecimientos
                            Class<?> eventType = parameterTypes[0];
                            methodKeyBuilder.setLength(0);
                            methodKeyBuilder.append(methodName);
                            methodKeyBuilder.append('>').append(eventType.getName());
                            String methodKey = methodKeyBuilder.toString();
                            if (eventTypesFound.add(methodKey)) {
                                // Only add if not already found in a sub class
								//El método de la suscripción de un paquete de objeto, que contiene el objeto, threadMode objeto, eventType objetos
                                subscriberMethods.add(new SubscriberMethod(method, threadMode, eventType));
                            }
                        }
                    } else if (!skipMethodVerificationForClasses.containsKey(clazz)) {
                        Log.d(EventBus.TAG, "Skipping method (not public, static or abstract): " + clazz + "."
                                + methodName);
                    }
                }
            }
			//Ver también la función de la suscripción de atravesar el padre de clase
            clazz = clazz.getSuperclass();
        }
		//Finalmente, la adición de caché, el segundo uso directo de la caché de encima
        if (subscriberMethods.isEmpty()) {
            throw new EventBusException("Subscriber " + subscriberClass + " has no public methods called "
                    + eventMethodName);
        } else {
            synchronized (methodCache) {
                methodCache.put(key, subscriberMethods);
            }
            return subscriberMethods;
        }
    }
```

Para este método de explicar en el comentario dentro, aquí no se describen en repetir, aquí hemos encontrado una forma de suscripción de todos los abonados
Estamos de vuelta en el método de registro: 

```javascript
for (SubscriberMethod subscriberMethod : subscriberMethods) {
            subscribe(subscriber, subscriberMethod, sticky, priority);
        }
```

Para cada método de una suscripción, su método de subscribe el método de llamadas, a ver exactamente qué hiciste.

```javascript
private void subscribe(Object subscriber, SubscriberMethod subscriberMethod, boolean sticky, int priority) {
        subscribed = true;
		//Tipo de método de suscripción a la suscripción de los acontecimientos
        Class<?> eventType = subscriberMethod.eventType;
		//Mediante la suscripción de un tipo de evento, encontrar todas las suscripciones (la suscripción de suscripción), contiene abonado, método de suscripción
        CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
		//Crear una nueva suscripción
        Subscription newSubscription = new Subscription(subscriber, subscriberMethod, priority);
		//La lista de todas las suscripciones a la suscripción de unirse a este nuevo tipo de evento correspondiente
        if (subscriptions == null) {
			//Si la actual lista de suscripción no el caso, entonces crear y unirse a la suscripción
            subscriptions = new CopyOnWriteArrayList<Subscription>();
            subscriptionsByEventType.put(eventType, subscriptions);
        } else {
			//Si hay una lista de suscripción, compruebe si se ha adherido.
            for (Subscription subscription : subscriptions) {
                if (subscription.equals(newSubscription)) {
                    throw new EventBusException("Subscriber " + subscriber.getClass() + " already registered to event "
                            + eventType);
                }
            }
        }

		//De conformidad con la prioridad de suscripción de inserción
        int size = subscriptions.size();
        for (int i = 0; i <= size; i++) {
            if (i == size || newSubscription.priority > subscriptions.get(i).priority) {
                subscriptions.add(i, newSubscription);
                break;
            }
        }
		//El caso de suscripción de unirse a la lista de suscriptores en caso de suscripción
        List<Class<?>> subscribedEvents = typesBySubscriber.get(subscriber);
        if (subscribedEvents == null) {
            subscribedEvents = new ArrayList<Class<?>>();
            typesBySubscriber.put(subscriber, subscribedEvents);
        }
        subscribedEvents.add(eventType);
		//Este es el caso de no debatir sobre Cohesión, temporalmente
        if (sticky) {
            Object stickyEvent;
            synchronized (stickyEvents) {
                stickyEvent = stickyEvents.get(eventType);
            }
            if (stickyEvent != null) {
                postToSubscription(newSubscription, stickyEvent, Looper.getMainLooper() == Looper.myLooper());
            }
        }
    }
```

Bueno, aquí casi todo el proceso general de registro de métodos de análisis, es así, podemos resumir: 
1, El método de la suscripción de los registrados en encontrar a todos. 
2, El método de la suscripción se encuentre a través de EventBus eventType correspondiente en la lista de suscripción, según el método actual de suscripción abonados y crear una nueva suscripción a suscribirse a la lista
3, Encontrar la lista de eventos de suscripción EvnetBus abonado en eventType añadir a la lista de eventos. 

Así que para cualquier otro abonado, podemos encontrar la lista de tipos de eventos de la suscripción de su suscripción, a través de este tipo de incidentes, se pueden encontrar en función de la suscripción de los abonados en. 

Registro terminado el análisis de análisis post, este análisis terminado, EventBus principio casi terminado....

```javascript
public void post(Object event) {
		//En este EventBus sólo una, casi es un ejemplo concreto de cerca, no
        PostingThreadState postingState = currentPostingThreadState.get();
        List<Object> eventQueue = postingState.eventQueue;
		//En el caso de la cola
        eventQueue.add(event);

        if (postingState.isPosting) {
            return;
        } else {
            postingState.isMainThread = Looper.getMainLooper() == Looper.myLooper();
            postingState.isPosting = true;
            if (postingState.canceled) {
                throw new EventBusException("Internal error. Abort state was not reset");
            }
            try {
                while (!eventQueue.isEmpty()) {
					//La distribución de los acontecimientos
                    postSingleEvent(eventQueue.remove(0), postingState);
                }
            } finally {
                postingState.isPosting = false;
                postingState.isMainThread = false;
            }
        }
    }
```

Puesto que no hay nada concreto de la lógica, su función principal es la llamada postSingleEvent cumplir esta función, a ver.

```javascript
private void postSingleEvent(Object event, PostingThreadState postingState) throws Error {
        Class<? extends Object> eventClass = event.getClass();
		//Encontrar a los acontecimientos eventClass correspondiente con eventos contiene eventos superclase correspondiente y la interfaz
        List<Class<?>> eventTypes = findEventTypes(eventClass);
        boolean subscriptionFound = false;
        int countTypes = eventTypes.size();
        for (int h = 0; h <countTypes; h++) {
            Class<?> clazz = eventTypes.get(h);
            CopyOnWriteArrayList<Subscription> subscriptions;
            synchronized (this) {
				//Encontrar abonados correspondiente al caso, esto es a través de la adhesión (Register recuerdas?.
                subscriptions = subscriptionsByEventType.get(clazz);
            }
            if (subscriptions != null && !subscriptions.isEmpty()) {
                for (Subscription subscription : subscriptions) {
                    postingState.event = event;
                    postingState.subscription = subscription;
                    boolean aborted = false;
                    try {
						//El método para cada suscripción
                        postToSubscription(subscription, event, postingState.isMainThread);
                        aborted = postingState.canceled;
                    } finally {
                        postingState.event = null;
                        postingState.subscription = null;
                        postingState.canceled = false;
                    }
                    if (aborted) {
                        break;
                    }
                }
                subscriptionFound = true;
            }
        }
		//Si no la suscripción, que será un caso de NoSubscriberEvent post
        if (!subscriptionFound) {
            Log.d(TAG, "No subscribers registered for event " + eventClass);
            if (eventClass != NoSubscriberEvent.class && eventClass != SubscriberExceptionEvent.class) {
                post(new NoSubscriberEvent(this, event));
            }
        }
    }
```

Este método postToSubscription un método básico, a ver.

```javascript
private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
		//La primera es la suscripción de los parámetros de la introducción, el segundo parámetro es para la distribución de los acontecimientos, el tercer parámetro fundamental: si en el hilo principal
        switch (subscription.subscriberMethod.threadMode) {
		//Qué es esto threadMode entrante, piensa cuidadosamente? Según enevento, onEventMainThread, onEventBackground, onEventAsync decisión?
        case PostThread:
			//La función de suscripción directamente en el hilo
            invokeSubscriber(subscription, event);
            break;
        case MainThread:
            if (isMainThread) {
				//Si directamente en el hilo principal en este campo, tan directamente en función de la suscripción de llamadas
                invokeSubscriber(subscription, event);
            } else {
				//Si no está en el hilo principal, entonces el manejador de ejecución en el hilo principal y, en concreto, no voy siguiendo
                mainThreadPoster.enqueue(subscription, event);
            }
            break;
        case BackgroundThread:
            if (isMainThread) {
				//Si el hilo principal, la creación de un hilo en la piscina en runnable
                backgroundPoster.enqueue(subscription, event);
            } else {
				//Si el hilo, llamadas directas
                invokeSubscriber(subscription, event);
            }
            break;
        case Async:
			//No importa qué hilos, directamente en el hilo de la piscina
            asyncPoster.enqueue(subscription, event);
            break;
        default:
            throw new IllegalStateException("Unknown thread mode: " + subscription.subscriberMethod.threadMode);
        }
    }
```
