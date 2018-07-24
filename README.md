# Patrones de Diseño para Humanos

Los patrones de diseño son **soluciones a problemas recurrentes**. No son paquetes , ni clases ni mucho menos librerias , asi que no pasa ninguna
magia por detras.

Los patrones son pasos a dar para resolver problemas comunes en applicaciones web , de esta manera buscaremos soluciones sostenibles y 
escalables de tal manera que  crearemos ciertas convenciones a la hora de crear codigo, asi cuando otro desarrollador lea nuestro
codigo pueda enterderlo mucho mas rapido!.

:warning: **# CUIDADO!**
---

+  Los patrones de diseño no son una armadura para todos nuestros problemas.
+  No trates de forzarlos o tendras en vez de solucionar un problema comun tendras mas problemas.

> El codigo que usaremos para implementar los PD sera java , pero que esto sea un impedimento , puedes implementarlos en cualquier lenguaje
que desees.




**Los Patrones Se Dividen En 3 Tipos**
---
+ Creacionales
+ Estructurales
+ Comportamiento


**Patrones Creacionales**
---

Lo que dice wikipedia:

>Corresponden a patrones de diseño de software que solucionan problemas de creación de instancias. 
Nos ayudan a encapsular y abstraer dicha creación.

En termino humano:

> Los diseños bajo esta categoria , muestras maneras de crear Objetos. tal vez esten pensando que solo es necesario un **new** para rear un objeto
pero de vez en cuando esta forma de creacion de objetos no se adapta muy bien a los requerimientos del sistema por ello ahora veremos
como estos patrones nos enseñaran a crear objetos de una manera muy practica.


## Patrones bajo La categoria de Creacionales son:

+ Simple Factory
+ Factory Method
+ Abstrac Factory
+ Builder 
+ Prototype 
+ Singleton


:house: Simple Factory
---

**Ejemplo del mundo real:**

> Piensa que estas construyendo un casa y necesitas puertas. seria una perdida de tiempo que cadavez que necesites una puerta ponerte
tu ropa de carpintero y comenzar a hacerla en tu casa. en ve de conseguirla medante la fabrica.

**En termino humano:**

> El patron Simple Factory retorna una instancia para el **cliente**(clase que llama a nuestra clase) sin exponer ninguna logica de creacion.


Programemos un Ejemplo:
 
```java
interface Puerta {

  public int obtenerAltura();
  public int obtenerAnchura();
  
 }
 
 public class PuertaMadera implements Puerta{
 
    private int ancho;
    private int alto;
    
    public PuertaMadera(int ancho , int alto){
    this.ancho = ancho;
    this.alto = alto;
    }
    
    @Override
    public int ObtenerAltura(){
    return alto;
    }
    
    @Override
    public int ObtenerAnchura(){
    return ancho;
    }

```

Ahora crearemos el Simple Factory el cual nos devolvera una instancia de una clase asi que no tenemos que hacer new
desde nuestra clase cliente.


```java
public class HazLaPuertaPorMi{
 
   public static Door HazPuerta(int ancho , int alto){//Simple Factory
      return new PuertaMadera(ancho,alto);
    }

 
 public class Client{//Cliente
 
    public static void main(String[] args){
   
        //Necesito un objeto de Puerta de mandera pero para hacer i coigo mas legible no quiero que aqui haya ninguna logica de creacion
        //usaremos el SIMPLE FACTORY creado anteriormente
        
        HazLaPuertaPorMi.HazPuerta(1,2);
        
        //Ya no tenemosque hacer esto siguiente
       //Door puerta = new PuertaMadera( 8 , 7);
        
   
      }
   
   }
   ```

**Cuando Usarlo?**
      
      
  Lo usaremos cuando la creacion de un objeto envuelve mas logica que solo la fsimple instanciacion. como por ejemplo filtrar los valores 
  que estan entrando etc...
  
  
  :factory: Factory Method
  ---
  
  Ejemplo del mundo real:
  
  >Imagina a un director de recursos humanos , seria imposible para una sola persona poder entrevistar paracada una de las pisiciones.
  basado en los empleos correspondientes se tiene que delegar los pasos de entrevista a diferentes personas.

  Para Humanos:
  
  >Se trata de definir una Interfaz para la creacion de objetos pero dejar que las implementaciones de esta clase creen los objetos.
  veamos un ejemplo.


Programemos un Ejemplo:


```java
interface CuantaBancaria{
    pubic String accountType();
}


public class CuentaDeAhorros extends CuentaBancaria{
  
  @Override
  public String accountType(){
      return "Cuenta de Ahorros";
    }
  }
  
 public class CuentaCorriente extend CuentaBancaria{
  
   @Override
   public String accountType(){
      return "Cuenta Corriente";
    }
  }
```

Todo muy normal hasta ahora tenemos Una interfaz de tipo Cuenta de Banco al cual tiene dos implementaciones Cuenta de ahorro y Cuenta 
Corriente.

Ahorra crearemos el Factory method.para ello vamos a crear una interfaz que provea un metodo en comun a sus subclases. luego
crearemos una subclase con el Factory method que devolvera una instancia de tipo o **CuentaDeAhorro** o **CuentaCorriente**.

```java

interface DevuelveInstancias{// Interfaz que da la abstraccion a las subclases.
  public CuentaDeBanco getAccount();
 }


public class FactoryMethod extends DevuelveInstancias{//Subclases que decidiran cual clase Instancializar

   private static String Ahorros = "Ahorro";
   private static String Corriente = "Corriente";

  public static CuentaDeBanco getAccount(String AccountType){
  
      if(AccountType.equalsIgnoreCase(Ahorros)){
          return new CuentaDeAhorros();
        
        }else if(AcountType.equalsIgnoreCase(Corriente)){
          return new CuentaCorriente();
        }else{
        
        return null;   
    }

```

```java
  
  public class Clinte{
  
    public static void main(String[] args){
      
      Account cuenta = FactoryMethod.getAccount(Ahorro);// usamos el Factory method para conseguir una instancia de tipo Ahorro
      
      Account cuenta2 = FactoryMethod.getAccount(Corriente);// usamos ahora el mismo metodo Factory para obtener otro tipo de instancia.
    
    }

```

**Cuando Usarlo?**

Lo usaremos cuando hay algun proceso generico en una clase pero la subclase debe obtenerse dinamicamente en tiempo de ejecucion.
Ejemplo: necesitamos procesar las CuentasDeBanco que resivamos para guardarla en la base de datos , no podemos indicar que
la subclase que entrara sera de ahorro o corriente ya que eso lo sabremos en tiempo de ejecucion. asi que usaremos este patron
y el cliente indicara cual de las subclases usara.



:hammer: Abstract Factory
---

Ejemplo de la vida real:

>Volviendo al ejemplo de Simple Factory , recurrimos a la fabrica(tienda) para obtener la puerta.Tal vez dependiendo de la puerta
que obtengamos necesitemos a alguien que sepa bregar con ese tipo de puerta: Un carpintero para la madera , Un herrero para la de hierro.



Para Humanos:

> El patron Abstract Factory nos permite mediante el uso de los **Factory methods** obtener objetos que estan relacionados entre si.(
carpintero para la puerta de madera. Herrero para las de hierro).


Programemos un Ejemplo:

```java
  interface Puerta{
    public String TipoPuerta();
  }
  
  
  public class PuertaDeMadera extends Puerta{
      @Override
      public String TipoPuerta(){
        return "puerta de madera";
      }
  }
  
    
  public class PuertaDeHierro extends Puerta{
  
    @Override
    public String TipoPuerta(){
        return "puerta de hierro"
      }
   }
```
  
  Como necesitamos un experto que sepa montar puertas crearemos la interfaz ExperoEnPuertas ,
  y como necesitaremos que el sepa montar una de madera o quizas
  de hierro crearemos implemetaciones que cumplan esos requisitos.
  
```java
  interface ExpertosEnPuertas{
      public String Descripcion();
  }
  
  
  
  public class ExpertoEnMadera extends ExpertosEnPuertas{
      @Override
      public String Descripcion(){
        return "Experto en Puertas De Madera";
      }
   }
   
   
   public class ExpertoEnHierro extends ExpertosEnPuertas{
      @Override
      public String Descripcion(){
        return "Experto en Puertas De Hierro";
      }
   }
```


Bien! , Ahora crearemos nuestra Fabrica de Factorias (Abstract factory) que nos hara familia de objetos. Puerta de madera on un carpintero, Una puerta de hierro con un herrero.


```java

  interface AbstractFactory{
      public Puerta devuelvePuerta();
     
      public ExpertoenPuertas devuelveExpertoCorrespondiente();
  }
  
  
  public class FactoriaDeMadera extends AbstractFactory{
     
     @Override
     public Puerta devuelvePuerta(){// Me da una puerta de madera
       return new PuertaDeMadera();
     }
     
     @Override
     public ExpertoenPuertas devuelveExpertoCorrespondiente(){// y tambien me da el experto en montar puertas especialmente de Madera
       return new ExpertoEnMadera();
     }
   
   
   public class FactoriaDeHierro extends AbstractFactory{
     
     @Override
     public Puerta devuelvePuerta(){
       return new PuertaEnHierro();
     }
     
     @Override
     public ExpertoenPuertas devuelveExpertoCorrespondiente(){
       return new ExpertoeEnHierro();
     }
     
    }
  
  
 ```
  Ahora crearemos una clase que se encargara de llamar a los AbstractFactories dependiendo de si necesitamos puerta de madera con carpintero  o puerta de hierro con el herradero. normalmente a esta clase se le llama FactoryProducer porque produce Factorias.
 
 ```java
 public class FactoryProducer{
    private static String MaderaFactory = "FactoriaMadera";
    private static String HierroFactory = "FactoriaHierro;
    
   public static AbstractFactory obtenObjetos(String Factoria){
      if(Factoria.equelasIgnoreCase(MaderaFctory)){
         return new FactoriaDeMadera();
       }else if(Factoria.equalsIgnoreCase(HierroFactory)){
         return new FactoriaDeHierro();
       }else{
        return null;
     }
  
  }
 ```
 Creemos nuestra clase Cliente:
 
 ```java
  public class Cliente{
  
   public static void main(String[] args){
   
     /*Digamos que estamos haciendo nuestra casa y pensamos que en la sala principal se necesita una puerta de madera 
     Llamaremos a la tienda(AbstractFactory) por una puerta de madera y alguien que sepa montar tal tipo de puertas:*/
     
     FactoriaDeMadera madera = FactoryProducer.obtenObjetos("FactoriaHierro");
     
     
     madera.devuelvePuerta();//nos devuelven la puerta del tipo que pedimos, Madera.
     madera.devuelveExpertoCorrespondiente();// Y de igual modo nos devuelven un experto en montar puertas del tipo Madera
     
     FactoriaDeHierro hierro = FactoryProducer.obternObjetos("FactoriaDeMadera");
     
     hierro.devuelvePuerta();//Ahora compramos una puerta de tipo madera para la galeria
     hierro.devuelveExpertoCorrespondiente();//Obviamente necesitaremos a alguien que sepa montar este tipo de puertas.
 ```
 
 **Cuando Usarlo?**
 
 Lo usaremos cuando necesitamos familia de objetos osea objetos que esten relacionados entre si, y como estamos usando el patron Factory
 Method no estamos usando ninguna logica en la clase Cliente.
 
 
 :construction_worker: Builder
 ---
 
 Ejemplo de la vida real:
 
 >Imagina que vas al McDonals y pides alguna hamburguesa , el personal sin hacerte ninguna pregunta te devuelve tu hamburguesa.
  esto seria el Simple Factory. Pero que tal si te preguntan que tipo de salsa quieres , carne de res o pollo? . En este caso hay
  ciertos   pasos a dar en la creacion de tu hamburguesa. Para ello Necesitaremos usar el Builder Pattern.
 
 
 En terminos Humanos:
 
 >Cuando queremos construir un objeto complicado en el que hay que tomar en cuenta varios pasos se usa el Builder Pattern.
 
 
 Programemos un ejemplo:
 
 	
```java

 public class Hamburguesa {
   private String tipoHamburguesa;
   private String tipoSalsa;
   private String tipoCarne;
   private boolean bebida;
   private boolean ketchup;
   
   public Hamburguesa(HamburguesaBuilder hamburguesaBuilder){
   
     this.tipoSalsa = hamburguesaBuilder.tipoSalsa();
     this.tipoCarne = hamburguesaBuilder.tipoCarne();
     this.bebida = hamburguesaBuilder.bebida();
     this.ketchup = hamburguesa.ketchup();
   }
   
   //getters y setters
   
   
   //Ahora crearemos nuestro Builder el encargado de construirnos paso por paso nuestra Hamburguesa
   
  interface Builder{
   public Hamburguesa cocinar();
  }
  
  public class HamburguesaBuilder implements Builder}//concreteBuilder
  
    private String tipoHamburguesa;
    private String tipoSalsa;
    private String tipoCarne;
    private boolean bebida;
    private boolean ketchup;
    
    public HamburguesaBuilder(String tipoHamburguesa){
     this.tipoHamburguesa = tipoHamburguesa;
    }
    
    
    public HamburguesaBuilder tipoSalsa(String tipoSalsa){
     this.tipoSalsa = tipoSalsa;
     return this;
     }
     
     public HamburguesaBuilder tipoCarne(String tipoCarne){
      this.tipoCarne = tipoCarne;
      return this;
     }
     
     public HamburguesaBuilder bebida(boolean bebida){
      this.bebida = bebida;
      return this;
     }
     
     public HamburguesaBuilder ketchup(boolean Ketchup){
      this.ketchup = ketchup;
      return this;
     }
     
     public Hamburguesa cocinar(){
      return new Hamburguesa(this);
     }
     

```
 
Ahora crearemos nuestra clase cliente y veremos como nuestro Builder nos ayuda a crear este objeto hamburguesa.

```java

public class Cliente{
  
  public static void main(String[] args){
   
   Hamburguesa miAlmuerzo = new HamburguesaBuilder("Wooper")
             .tipoSalsa("picante")
             .tipoCarne("pollo")
             .bebida(true)
             .ketchup(false)//No me gusta!
             .cocinar();// Yummy!
             
   //Sino utilizaramos el Builder , la creacion del objeto seria mas complicada y en applicaciones grandes es sinonimo de perdida
   de tiempo, ademas de que es posible que me equivoque en algun parametro del objeto.Ej:
   
   Hamburguesa almuerzo2 = new Hambruguesa("Wooper","picante","true","false
```
 
**Cuando Usarlo?**

Lo usaremos cuando a la hora de crear un objeto se vean implicados varios pasos antes de poder hacerlo. ;D
 


🐑 Prototype
---

Ejemplo de la vida "real"

>Se recuerdan de Dolly? la oveja que fue clonada?... Pues ya vemos la intencion del patron, no?. Clonar!'

En terminos Humanos:

> Clonar a un objeto ya existente.


Programemos un ejemplo , Clonemos una oveja!.

```java

public abstract Oveja implements Cloneable {

  abstract public String tipoOveja();
  
   public Object clone() { 
     Object clone = null; 
    try {         
      clone = super.clone();  
      
   }catch (CloneNotSupportedException e) {     
   e.printStackTrace();      
        }      
   return clone;     
       }  
   }
  
  
  
  public class Dolly implements Oveja{
  
    @Override
    public String tipoOveja(){
      return "Es Dolly";
    }
    
    }
    
```

Ya creamos nuestra clase abstracta que implemente la interfaz Cloneable la responsable de clonar objetos, ahora vamos a crear nuestra
clase cliente y clonemos cuales cientificos!


    
```java

 public class Cliente(
 
    public static void main(String[] args){
    
    Dolly primeraOveja = new Dolly();
    
    //Ahora vamos a clonar a nuestra oveja!
    
    Dolly segundaOveja = primerOveja.clone();
    
    }
}
    
```


**Cuando Usarlo?**

Usaremos el Patron prototype cuando debemos crear varios objetos con parametros iguales o similares Ej: si creamos una biblioteca
tal vez tengamos libros de la misma editorial , autor , idioma.... En esos casso el Patron Prototype es muy Util.


:one: Singleton
---

Ejemplo de la vida real:

>Solo puede haber un solo presidente al mismo tiempo en un pais. El presidente es el patron Singleton.

En terminos Humanos:

> Con este patron nos aseguramos que solo un objeto pueda ser creado. Por ejemplo en una aplicacion empresarial se necesita
una conexion a la base de datos , esta conexion podria ser un Singleton asi que cada vez que necesitemos conectarnos a la base de datos
usemos ese objeto. Si tenemos muchas conexiones a la base de datos(muchso obetos de conexion) nuestra aplicacion ira muy lenta!.


Hay dos tipos de Singletons a uno se le llama "Lazy" y a otro "Eagerly". Hagamos el primero el Early!>

Programemos un ejemplo:

```java

 
 public class LazySingleton{
 
 public static final LazySingleton singleton = new LazySingleton();// Creara un objeto de el mismo dede que se ejecute nuestra app
 //por eso es llamado Lazy Singleton
 
 
 private LazySingleton(){}
 
 public static LazySingleton dameInstancia(){
   return singleton;
  }
 
 }

```


Ahora creemos el Eagerly Singleton.


```java

public class EagerlySingleton {

  private static EagerlySingleton eagerly = null;
  
  private EagerlySingleton(){}
  
  public EagerlySingleton obtenerInstancia(){
    
    if(eagerly == null){
       synchronized(EagerlySingleton.class)//Usamos esto para asegurar que no se peuda crear un objeto en diferentes hilos.
       eagerly = new eagerly();
     }
     return eagerly;
   }
 }
```

**Cuando Usarlo?**

Lo usaremos cuando haya algun objeto que no querramos que se cree mas de una vez en nuestros programas.


# Patrones Estructurales:


Lo que dice wikipedia:

>Son los patrones de diseño software que solucionan problemas de composición (agregación) de clases y objetos


En terminos Humanos:

>Los patrones bajo esta categoria nos ayudaran a hacer componentes de software(elemento de un sistema de software que ofrece un conjunto de servicios, o funcionalidades) de la mejor manera. En fin como nuestras clases y objetos interactuan unos con otros.



## Patrones bajo La categoria de Estructurales son:

+ Adapter  
+ Bridge
+ Composite
+ Decorator
+ Facade
+ Flyweight
+ Proxy

:electric_plug: Adapter
---

Lo que dice wikipedia:

>El patrón adaptador se utiliza para transformar una interfaz en otra, de tal modo que una clase que no pueda utilizar la primera haga uso de ella a través de la segunda.


En terminos humanos:

>Se usa el patron Adapter nos permite envolver un objeto incompatible en un Adapter para hacerlo compatible con otra clase.


Programemos un ejemplo:

Craemos un juego con un cazador que cazara Leones Luego utilizaremos el Patron adapter para que pueda cazar otro tipo de animales.


```java
public class Hunter{
 
  public Hunter(Lion lion){
   lion.roar();
  }
}

```


Ya tenemos a nuestro cazador ahora Creemos alos diferentes tipos de Leones.



```java

interface Leon{
 public void roar();
}

public class LeonAsiatico{

 @Override
 public void roar(){
 ///
 }
 
}

```

Ahora digamos que queremos agregar a un Lobo al juego para que el cazador los caze, como un lobo no es un Leon. Deberemos usar el patron
Adapter para Hacer que este objeto incompatible mediante el adapter puedan trabajar juntos.


```java
public class Lobo{
 
  public void Aullar(){
   //Auh!!!!!1
  }
}
```

Ahora nuestra clase adaptadora

```java
public class Adapter implements Lion{
  private Lobo lobo;
  
  public Adapter(Lobo lobo){
   this.lobo = lobo;
  }
  
  @Override
  public void roar(){
     lobo.Aullar();
  }
}
```


Ahora podremos tener  Lobo en nuestro juego tambien!. Creemos nuestra clase cliente.


```java

public class Cliente{

  public static void main(String[] args){
  
     Lion leon1 = new Lion();
     
     Hunter cazador = new Hunter(leon1);
     
     Lobo lobo = new Lobo();
     
     cazador = new Hunter(new Adapter(lobo));// aqui usamos al clase adaptadora para hacer que el cazador tambien caze lobos :D

```


:bridge_at_night: Bridge

Lo que wikipedia dice:

>El patrón Bridge, también conocido como Handle/Body, es una técnica usada en programación para desacoplar una abstracción de su implementación, de manera que ambas puedan ser modificadas independientemente sin necesidad de alterar por ello la otra.

En Terminos Humanos:
>Permite que la Abstraccion(metodos abstractos) y la implementacion(clases sobreescriben estos metodos y le dan alguna logica) esten desacopladas pudiendo asi cambiar en tiempo de ejecucion la implementacion del mismo.

Programemos un ejemplo:

Imagina que tiene sun Website y quieres permitirles a lo susuarios customizar el tema de las paginas , como lo harias?, crearias una
copia de cada pagina por cada tema? **o crearas un tema separado que sea cargado a la pagina deacuerdo con laspreferencias del usuario?**

![alt text](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png "Logo Title Text 1")


Primero hagamos el mal ejemplo , asi entenderemos mejor porque usamos el patron bridge.

```java
interface Webpage {
 public String pageType();
 public String theme();
}

public class About implements Webpage{

 @Override
 public String pageType(){
   return "About";
 }
 
 @Override
 public String theme(){
  //mucho codigo
  return "Tema por Defecto de About"
 }
 
 
 public class Contact implements Webpage{
 
 @Override
 public String pageType(){
   return "Contact";
 }
 
 @Override
 public String theme(){
  return "Tema por Defecto de Contact";
 }
}

```
Bueno ya creamos la interfaz que da abstraccion y las subclases que conforman nuestra pagina.
Ahora se necesita que los usuarios customizen el theme de cada pagina , por ejemplo Contact puede ser rojo,
verde.. por igual About puede ser de diferentes colores. el problema aqui es que cuando queremos que la cada
pagina tenga un comportamiento diferente la solucion que nos provee la herencia es crear una nueva clase
heredad de nuestras paginas y cambiar la implementacion de theme , pero esta no es la mejor solucion , ya que
por cada implementacion diferente deberemos crear una subclase. 

Para solucionar este problema usaremos el patron Bridge, su lema es : "Desacoplar la abstraccion de su implementacion", 
haciendo uso de la composicion veremos como el patron bridge soluciona el problema.


```java
interface Theme{//Interfaz que sera cargada en tiempo de ejecucion a las paginas segun las preferencias del usuario.
 public String theme();
}

public class RedTheme implements Theme{
 @Overide
 public String theme(){
   return "Red theme";
 }
}

public class Blacktheme implements Theme{
 @Override
 public String theme(){
   return "Black theme";
 }
}
```
Bien!, Ahora crearemos nuestras Paginas

```java
abstract public class Webpage{

 protected Theme theme;
 public Webpage(Theme theme)
   this.theme = theme;
 }
 
 public String theme();
 
}


public class About extends Webpage{
 
 public About(Theme theme){
  super(theme);
 }
 @Override
 public String theme(){
   return super(theme).theme();
  }
 }
 
 public class Contacts extends Webpage{
 
 public Contacts(Theme theme){
  super(theme);
 }
 @Override
 public String theme(){
   return super(theme).theme();
 }
}
```
El problema fue solucionado , nuestras paginas ya aceptan cualquier tipo de interfaz dependiendo de las preferencias
del usuario. Ya no tenemos que crear subclases para crear implementacion ya nuestra implementacion esta desacoplada
de la abstraccion.

**Cuando Usarlo?**

Lo usaremos cuando esperemos cambios en tiempo de ejecucion.





