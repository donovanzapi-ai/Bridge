# Ejercicio: Patrón Bridge en Java

## 🌉 Contexto del Problema

Tenemos diferentes tipos de dispositivos (e.g., `Radio`, `TV`) y diferentes tipos de controles remotos (e.g., `ControlRemotoBasico`, `ControlRemotoAvanzado`). Queremos evitar una explosión de clases como `ControlRemotoBasicoParaRadio`, `ControlRemotoAvanzadoParaTV`, etc., y permitir que ambas jerarquías evolucionen independientemente.

## 🎯 Tu Tarea

Analizar y comprender cómo el patrón Bridge desacopla la abstracción (controles remotos) de la implementación (dispositivos), permitiendo que ambas jerarquías varíen independientemente.

## 📋 Instrucciones Paso a Paso

### Paso 1: Identifica las Dos Jerarquías

**Jerarquía de Implementación (Dispositivos):**
- Interfaz `Dispositivo` con operaciones básicas de dispositivo
- Clases `Radio` y `TV` que implementan la interfaz

**Jerarquía de Abstracción (Controles):**
- Clase abstracta `ControlRemoto` que contiene referencia a un `Dispositivo`
- Clases `ControlRemotoBasico` y `ControlRemotoAvanzado` que extienden `ControlRemoto`

### Paso 2: Implementa la Interfaz Dispositivo

Crea la interfaz que define las operaciones que todos los dispositivos deben implementar:

```java
public interface Dispositivo {
    boolean isEnabled();
    void enable();
    void disable();
    int getVolume();
    void setVolume(int percent);
    int getChannel();
    void setChannel(int channel);
    void printStatus();
}
```
### Paso 3: Implementa las Clases Concretas de Dispositivos

Para Radio:

```java
public class Radio implements Dispositivo {
    private boolean on = false;
    private int volume = 30;
    private int channel = 1;
    
    @Override
    public boolean isEnabled() { return on; }
    
    @Override
    public void enable() { 
        on = true;
        System.out.println("Radio encendida");
    }
    
    @Override
    public void disable() { 
        on = false;
        System.out.println("Radio apagada");
    }
    
    // Implementa los demás métodos: getVolume, setVolume, getChannel, setChannel
    // y printStatus para mostrar el estado actual
}
```

Para TV:

```java
public class TV implements Dispositivo {
    private boolean on = false;
    private int volume = 30;
    private int channel = 1;
    
    @Override
    public boolean isEnabled() { return on; }
    
    @Override
    public void enable() { 
        on = true;
        System.out.println("TV encendida");
    }
    
    @Override
    public void disable() { 
        on = false;
        System.out.println("TV apagada");
    }
    
    // Implementa los demás métodos similares a Radio
}
```

### Paso 4: Crea la Abstracción Base (ControlRemoto)

Implementa la clase abstracta que contiene el "puente" hacia la implementación:

```java
public abstract class ControlRemoto {
    protected Dispositivo dispositivo; // ¡ESTE ES EL PUENTE!

    public ControlRemoto(Dispositivo dispositivo) {
        this.dispositivo = dispositivo;
    }

    public abstract void togglePower();
    public abstract void volumeDown();
    public abstract void volumeUp();
    public abstract void channelDown();
    public abstract void channelUp();
}
```

### Paso 5: Implementa las Abstracciones Refinadas

Control Remoto Básico:

```java
public class ControlRemotoBasico extends ControlRemoto {
    public ControlRemotoBasico(Dispositivo dispositivo) {
        super(dispositivo);
    }

    @Override
    public void togglePower() {
        System.out.println("Control Básico: Botón power presionado");
        if (dispositivo.isEnabled()) {
            dispositivo.disable();
        } else {
            dispositivo.enable();
        }
    }

    @Override
    public void volumeDown() {
        System.out.println("Control Básico: Bajar volumen");
        dispositivo.setVolume(dispositivo.getVolume() - 10);
    }

    @Override
    public void volumeUp() {
        System.out.println("Control Básico: Subir volumen");
        dispositivo.setVolume(dispositivo.getVolume() + 10);
    }

    @Override
    public void channelDown() {
        System.out.println("Control Básico: Canal anterior");
        dispositivo.setChannel(dispositivo.getChannel() - 1);
    }

    @Override
    public void channelUp() {
        System.out.println("Control Básico: Canal siguiente");
        dispositivo.setChannel(dispositivo.getChannel() + 1);
    }
}
```

Control Remoto Avanzado:

```java
public class ControlRemotoAvanzado extends ControlRemoto {
    public ControlRemotoAvanzado(Dispositivo dispositivo) {
        super(dispositivo);
    }

    // Implementa los mismos métodos básicos
    @Override
    public void togglePower() {
        System.out.println("Control Avanzado: Power inteligente");
        if (dispositivo.isEnabled()) {
            dispositivo.disable();
        } else {
            dispositivo.enable();
        }
    }

    // ... implementa volumeDown, volumeUp, channelDown, channelUp

    // Método adicional solo disponible en el control avanzado
    public void mute() {
        System.out.println("Control Avanzado: Silenciar");
        dispositivo.setVolume(0);
    }

    public void record() {
        System.out.println("Control Avanzado: Grabando programa actual");
        // Lógica específica del control avanzado
    }
}
```

### Paso 6: Experimenta con Diferentes Combinaciones

En la clase Main, prueba diferentes combinaciones de controles y dispositivos:

```java
public class Main {
    public static void main(String[] args) {
        // Combinación 1: Control básico con Radio
        System.out.println("=== Control Básico con Radio ===");
        ControlRemoto control1 = new ControlRemotoBasico(new Radio());
        probarControlRemoto(control1);

        // Combinación 2: Control avanzado con TV
        System.out.println("\n=== Control Avanzado con TV ===");
        ControlRemotoAvanzado control2 = new ControlRemotoAvanzado(new TV());
        probarControlRemoto(control2);
        control2.mute(); // Funcionalidad exclusiva del control avanzado

        // Combinación 3: Control básico con TV
        System.out.println("\n=== Control Básico con TV ===");
        ControlRemoto control3 = new ControlRemotoBasico(new TV());
        probarControlRemoto(control3);

        // Combinación 4: Control avanzado con Radio
        System.out.println("\n=== Control Avanzado con Radio ===");
        ControlRemotoAvanzado control4 = new ControlRemotoAvanzado(new Radio());
        probarControlRemoto(control4);
        control4.record();
    }

    public static void probarControlRemoto(ControlRemoto control) {
        control.togglePower(); // Encender
        control.volumeUp();
        control.channelUp();
        control.volumeDown();
        control.togglePower(); // Apagar
    }
}
```

## 🔍 Puntos Clave del Bridge

El "Puente":

La composición protected Dispositivo dispositivo; en la clase ControlRemoto es el puente que conecta las dos jerarquías independientes.

Ventajas del Patrón:

Evita explosión de clases: Sin Bridge necesitaríamos:

ControlRemotoBasicoRadio
ControlRemotoBasicoTV
ControlRemotoAvanzadoRadio
ControlRemotoAvanzadoTV
Extensibilidad independiente: Podemos agregar nuevos dispositivos O nuevos controles sin afectar la otra jerarquía
### 🤔 Preguntas de Análisis

* Explosión de Clases: ¿Cuántas clases necesitaríamos si tuviéramos 3 tipos de controles y 4 tipos de dispositivos sin el patrón Bridge? ¿Y con el patrón Bridge?
- Sin agregar el patron tendriamos que hacer más clases, si hicieramos un dispositivo serían 3 más y si sumara solo 1 control más sumaria 4.
Si fuera el otro caso, con el patrón, se tendria que separa las clases en las jerarquias que s aplicaron en el ejercicio, si agregara un dispositivo solo seria una nueva clase.
* Acoplamiento: ¿Qué pasaría si la clase ControlRemotoBasico heredara directamente de Radio o TV en lugar de usar composición?
Si asi fuera el control quedaria exclusivo para cada dispositivo y no podria reutilizarse facilmente en codigo, por ello la herencia seria algo complejo de utilizar o presentar, no podria cambiar de objeto.
* Extensibilidad: Si quiero agregar un nuevo dispositivo (ej: Projector), ¿qué cambios necesito hacer? ¿Necesito modificar las clases de controles existentes?
Tendria primero que crear el dispositivo y con sus respectivos metodos, que puedan funcionar con el, y no tendrian tema con las otros controles porque no afectan su funcionamiento.
* Principios SOLID: ¿Qué principios SOLID se aplican en este patrón? (Especialmente Principio de Inversión de Dependencias y * Principio Abierto/Cerrado)
El principio de Inversion de Dependencias ya que tanto los controles de alto nivel como los dispositivos concretos dependen de las abstracciones, no de implementaciones entre si.
El Principio Abierto/ Cerrado el sistema esta abierto a la extension, se puedne añadir más, y cerrado a la modifcacion, no se tiene que reescribir de nuevo las clases.
El principio de Responsabilidad Unica ya que separa la logica de control/ interfaz de usuario de la logica especifica. 
* Flexibilidad: ¿Cómo nos beneficiamos al poder cambiar el dispositivo de un control en tiempo de ejecución?
Reutilizar el mismo objeto control ayuda a manejar distintsos dispositivos.
Adaptarse a cambios de estado de manera limpia sin tener desacoplamiento y sin cadenoas de condicionales largas.
### 🧪 Ejercicios de Extensión

* Agrega un nuevo dispositivo: Implementa la clase Projector y prueba con los controles existentes.
* Agrega un nuevo control: Crea ControlRemotoVoz que responda a comandos de voz.
* Funcionalidad cruzada: ¿Qué pasaría si algunos controles avanzados solo funcionaran con ciertos dispositivos? ¿Cómo manejarías esta restricción?