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


**Patrones De Diseño Creacionales**
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
 
 
