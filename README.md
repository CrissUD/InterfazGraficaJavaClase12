# Interfaz Gráfica en Java

Curso propuesto por el grupo de trabajo Semana de Ingenio y Diseño (**SID**) de la Universidad Distrital Francisco Jose de Caldas.

## Monitor

**Cristian Felipe Patiño Cáceres** - Estudiante de Ingeniería de Sistemas de la Universidad Distrital Francisco Jose de Caldas

# Clase 12

## Objetivos

* Reconocer la forma para realizar animaciónes de movimiento dentro de nuestro proyecto para adicionar interactividad a las interfaces de usuario o mostrar información adicional que el usuario requiera.
* Identificar algunos factores importantes que intervienen cuando se realizan animaciónes como los objetos y eventos que intervienen.
* Explicar algunas particularidades clave a la hora de usar animaciones que son importantes para el buen funcionamiento.
* Explorar varios enfoques para realizar una misma animación pero en distintas formas.

# Antes de Comenzar

Para continuar con el ejercicio deberá actualizar la carpeta **resources/img** ya que se han agregado nuevas imágenes. Estas las puede descargar en este mismo repositorio entrando a la carpeta **Clase12** seguido de **resources/img**.

Recordando un poco nuestro recorrido en la clase anterior revisamos el uso de tablas para gestionar información, entre otras cosas se mostró **Las principales partes para la creación de una tabla**, **Los métodos de gestión de información dentro de tablas**, **Personalización de tablas**.

<div align='center'>
    <img  src='https://i.imgur.com/K8JYhqm.png'>
    <p>Resultado de la clase anterior con el uso de tablas.</p>
</div>

# Uso de animaciones

Esta sesión tratará el tema de animaciones de movimiento sobre nuestras interfaces y para la explicación e implementación de estas se verán 4 items:
* **Animaciones para Interactividad.**
    * **Particularidades con las animaciones.**
* **Animaciones para muestra de información.**
    * **Enfoques de animación.**

# Animaciones para Interactividad

En muchos casos nos interesa crear animaciones para darle un toque extra de interactividad a nuestras interfaces de usuario, las animaciones pueden capturar la atención de nuestros usuario y hacen de la visibilidad una experiencia bastante agradable. Para crear animaciones dentro de nuestro proyecto vamos a enfocarnos una vez mas en nuestro **Login** y vamos a darle uso a los tres botones que tiene nuestro login en su parte izquierda:

<div align='center'>
    <img  src='https://i.imgur.com/O01VUii.png'>
    <p>Botones de opción dentro de nuestro Login</p>
</div>

Lo que queremos es que una vez se oprima alguno de estos botones se muestre una nueva imagen en el Login:

<div align='center'>
    <img  src='https://i.imgur.com/fGHHfg3.png'>
    <p>Imagen dentro del login que quiere ser cambiada</p>
</div>

Pero para no hacer que esta desaparezca de golpe una vez se muestre la siguiente imágen podemos utilizar **Animaciones**, de esta forma podemos hacer que se mueva a la izquierda la imagén actual y asi la nueva imagen pueda verse en pantalla. Para realizar esto antes debemos hacer algunos preparativos.

### **Preparativos para realizar la animación**.

Como vamos a añadir otras dos imágenes (para la segunda y tercera opción) vamos a crear los objetos **ImageIcon** de estas dentro de nuestra clase **LoginTemplate**. También vamos a añadir una nueva imagen para los botones de las opciones la cual representará cual de las 3 opciones esta siendo seleccionada.

* **Declaración:**
```javascript
private ImageIcon iPunto2, iSvg2, iSvg3;
```

* **Ejemplificación**:
```javascript
// Dentro del método crearObjetosDecoradores
iSvg2 = new ImageIcon("Clase12/resources/img/imagen2.png");
iSvg3 = new ImageIcon("Clase12/resources/img/imagen3.png");
iPunto2 = new ImageIcon("Clase12/resources/img/punto2.png");
```

Para que las nuevas imágenes puedan ser vistas vamos a declarar por ahora los **Labels** correspondientes que contendrán estas imágenes:

* **Declaración:**
```javascript
private JLabel lSvg2, lSvg3;
```

Como queremos que haya un movimiento de las tres imágenes una vez se oprima alguno de los tres botones, debemos hallar una forma optimizada de mover las 3 imágenes al tiempo, ya que tendría mas esfuerzo  y código codificar el movimiento de cada una de las imágenes cada vez que se oprima alguno de los 3 botónes. Para esto vamos a crear un panel que contendrá estas imágenes y asi realizaremos el movimiento hacia el panel haciendo el efecto de que se moverán las 3 imágenes al tiempo.

**Declaración:**
```javascript
private JPanel pSvg;
```

**Construcción y adición:**
```javascript
// Dentro del método crearJPanels
pSvg = sObjGraficos.construirJPanel(
    100, 100, 1700, 345, new Color(0, 0, 0, 0), null
);
pIzquierda.add(pSvg);
```

Note algo importante, el color de fondo que le hemos configurado es un color **rgba o transparente** ya que tiene 4 argumentos, todos en 0, esto quiere decir que el color sera "negro" pero totalmente transparente, ya que el ultimo argumento **Alfa** esta en 0 (transparencia total), caso contrario es un color solido el cual debería tener 255 en ese argumento. También podemos notar que el panel no se esta agregando directamente a la ventana sino al panel **pIzquierda**. Además hay que recalcar su tamaño, ya que su ancho es bastante grande, esto para contener las 3 imágenes, sin embargo solo será visible una parte de este en pantalla.

Allí vamos a dejar nuestros Labels que contienen las imágenes **Svg** así que eso realizaremos:

```javascript
// Dentro del método crearJLabels

//LABEL SVG-----------------------------------------------------------------------------
iDimAux = new ImageIcon(
    iSvg.getImage().getScaledInstance(500, 345, Image.SCALE_AREA_AVERAGING)
);
lSvg= sObjGraficos.construirJLabel(null, 0, 0, 500, 345, iDimAux, null, null, null, "c");
pSvg.add(lSvg);

//LABEL SVG 2-----------------------------------------------------------------------------
iDimAux = new ImageIcon(
    iSvg2.getImage().getScaledInstance(500, 345, Image.SCALE_AREA_AVERAGING)
);
lSvg2= sObjGraficos.construirJLabel(null, 600, 0, 500, 345, iDimAux, null, null, null, "c");
pSvg.add(lSvg2);

//LABEL SVG 3-----------------------------------------------------------------------------
iDimAux = new ImageIcon(
    iSvg3.getImage().getScaledInstance(500, 345, Image.SCALE_AREA_AVERAGING)
);
lSvg3= sObjGraficos.construirJLabel(null, 1200, 0, 500, 345, iDimAux, null, null, null, "c");
pSvg.add(lSvg3);
```

Para el Label **lSvg** se agrego ahora al nuevo panel **pSvg** y ya no directamente al panel **pDerecha**, también se cambio las coordenadas de posición, los otros dos label se agregaron al mismo panel.

Vamos ahora a cambiar la imagen del primer botón por la imágen que indica que esta opción esta seleccionada ya que por defecto la primera opción ya esta seleccionada (La primera imagen ya se esta mostrando). Para los demás botones dejaremos la imagen que tenían originalmente pero le agregaremos la escucha de eventos **ActionListener** ya que esta solo la tenía el primer botón.

```javascript
// Dentro del método crearJButtons
iDimAux = new ImageIcon(
    iPunto2.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
);

//BOTÓN OPCIÓN 1-----------------------------------------------------------------------------
bOpcion1 = sObjGraficos.construirJButton(
    null, 10, 220, 30, 20, sRecursos.getCMano(), iDimAux, null,
    null, null, null, "c", false
);
bOpcion1.addActionListener(loginComponent);
pIzquierda.add(bOpcion1);

iDimAux = new ImageIcon(
    iPunto.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
);

//BOTÓN OPCIÓN 2-----------------------------------------------------------------------------
bOpcion2 = sObjGraficos.construirJButton(
    null, 10, 250, 30, 20, sRecursos.getCMano(), iDimAux, null,
    null, null, null, "c", false
);
bOpcion2.addActionListener(loginComponent);
pIzquierda.add(bOpcion2);

//BOTÓN OPCIÓN 3-----------------------------------------------------------------------------
bOpcion3 = sObjGraficos.construirJButton(
    null, 10, 280, 30, 20, sRecursos.getCMano(), iDimAux, null,
    null, null, null, "c", false
);
bOpcion3.addActionListener(loginComponent);
pIzquierda.add(bOpcion3);
```

Nuestra aplicación se ve de la siguiente manera:

<div align='center'>
    <img  src='https://i.imgur.com/Eug0GZr.png'>
    <p>Login de usuario con las modificaciones realizadas hasta el momento</p>
</div>

Por ahora el único cambio visible esta en el primer botón de opciones ya que resalta en naranja. Las otras imágenes ya están añadidas al panel **pSvg** pero como comentamos solo se esta viendo una parte de este panel.

Ahora vamos a crear algunos métodos **get**.

Como vamos a manipular la posición del panel **pSvg** vamos a necesitar obtenerlo desde la clase **loginComponent**:

```javascript
public JPanel getPSvg(){
    return this.pSvg;
}
```

Como vamos a cambiar la imagen dentro de cada botón una vez se seleccione y poner la imagen original en los botones que no están seleccionados vamos a necesitar estas dos imágenes en la clase **Component**, para que no ocurran problemas con las dimensiones de estas las vamos a retornar rescaldas de una vez:

```javascript
public ImageIcon getIPunto(){
    iDimAux = new ImageIcon(
        iPunto.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
    );
    return iDimAux;
}

public ImageIcon getIPunto2(){
    iDimAux = new ImageIcon(
        iPunto2.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
    );
    return iDimAux;
}
```

Ahora vamos a devolver los 3 botones de las opciones, ya habíamos creado el **get** del primero, para no realizar un método para cada uno y por motivos de la codificación posterior vamos a modificar el método que ya teníamos de tal forma que reciba como parámetro un entero que representara cual botón requiere y mediante if vamos a retornar el que necesitemos:

```javascript
public JButton getBOpcion(int boton){
    if(boton == 1)
        return this.bOpcion1;
    if(boton == 2)
        return this.bOpcion2;
    if(boton == 3)
        return this.bOpcion3;
    return null;
}
```

Por ultimo vamos a prevenir un futuro conflicto que tiene que ver con **el eje Z** y es que si recordamos un poco el panel que contiene las imágenes se agrego dentro del método **crearJPanels** y por otro lado los botónes de opciones se agregaron dentro del método **crearJButtons**. La creación de los paneles se llama dentro del constructor antes de la creación de los botones, esto inicialmente no crea ningún conflicto ya que el panel **pSvg** esta en la posición **100px** de nuestro panel **pIzquierda** mientras que los botones **bOpcion** están en la posición **10px** del mismo, pero una vez se mueva el panel **pSvg** hacia la izquierda para crear la animación este va a situarse por encima de los botones haciendo que las imágenes los tapen mientras pasan sobre ellos y ademas no se podrán seleccionar mas ya que estarán detrás del panel. Para arreglar esta situación vamos a sacar la agregación de este ultimo panel (**pSvg**) del método de crearJPanels y lo vamos añadir dentro del constructor justo después de la creación de los botones pero justo antes de la creación de los labels:

<div align='center'>
    <img  src='https://i.imgur.com/wS010oh.png'>
    <p>Resolución de conflictos en el eje Z</p>
</div>

### **Creando la animación.**

Vamos a concentrarnos en la clase **LoginComponent** lo primero que vamos a hacer es declarar tes atributos que nos serán útiles para realizar la animación:
```javascript
private int estado = 1, estadoAnterior = 0, direccion = -1;
```

* **estado:** El estado nos va a indicar cual es el botón que actualmente ha sido seleccionado y asi podremos cambiar la imagen por el aspecto naranja al botón que se selecciono y ademas que imágen debe mostrar, por defecto esta en 1 ya que el primer boton esta seleccionado y la primera imagen se esta mostrando una vez inicia la aplicación.
* **estadoAnterior:** El estado anterior nos va a indicar cual era el anterior botón que se selecciono previamente y asi poder dejarlo en su estado original.
* **Dirección:** Esta nos sera util cuando queramos ver la imagen del medio (la segunda opción) ya que esta puede ser vista desde la primera imágen donde se tendrá que correr el panel a la izquierda (de forma negativa) o desde la tercera imagen donde se tendrá que correr el panel a la derecha (de forma positiva). Como el estado inicial esta en la primera opción, la dirección por defecto va con -1 para que el panel se corra a la izquierda.

Ahora vamos a crear el objeto encargado de gestionar la animación, este es de tipo **Timer** y en el momento de su ejemplificación este debe recibir dos parámetros:
* **Delay:** Que es un numero entero y representa el tiempo en milisegundos donde creara un retraso en el movimiento y de esa forma se pueda apreciar la animación, sin este el objeto se movería de un lado al otro de golpe.
* **ActionListener:** Un Objeto Timer debe recibir un Objeto tipo ActionListener o en su defecto una clase que implemente esta interfaz, esto ya que la animación se realiza exclusivamente dentro de eventos **ActionListener**.

* **Declaración:**
```javascript
private Timer timer;
```

* **Ejemplificación:**
```javascript
// Dentro del constructor
this.timer=new Timer(1, this);
```

Como delay vamos a dejar 1, este es el numero que mas se suele dejar y que deja ver la animación de forma fluida, lo siguiente que nos pide es un objeto **ActionListener** como nuestra clase **LoginComponent** implementa esta interfaz insertamos esta clase por medio el **this**.

***Nota:** Por precaución es mejor realizar la ejemplificación de este objeto antes de la inyección con la clase template, esto para que el timer este listo antes de la creación de la interfaz y pueda funcionar correctamente. También se debe aclarar que en Java existen varios tipos de Objetos "Timer" pero el que estamos usando es específicamente de la librería Swing.*

```javascript
import javax.swing.Timer;
```

Como el Timer esta estrictamente relacionado con los eventos **ActionListener** vamos a tener que hacer algunos cambios, ya que la acción de entrar, registrarse o cerrar no tienen nada que ver con la animación y si se dejan dentro del **ActionListener** creara conflictos asi que vamos a pasar estas acciones al **MouseListener** específicamente al evento **MouseClicked**.


<div align='center'>
    <img  src='https://i.imgur.com/krR5uFM.png'>
    <p>Traslado de métodos de botones hacia MouseListener</p>
</div>

Para su buen funcionamiento recuerde agregar la escucha de eventos **MouseListener** al botón **bCerrar** y quitar la escucha de los eventos **ActionListener** a los botones **bCerrar, bEntrar y bRegistrarse**.

* **bCerrar.addMouseListener(loginComponent);**
* ~~bCerrar.addActionListener(loginComponent);~~
* ~~bEntrar.addActionListener(loginComponent);~~
* ~~bRegistrarse.addActionListener(loginComponent);~~


Ahora nuestro método **ActionPerformed** esta con una sola condición: 

```javascript
if (e.getSource() == loginTemplate.getBOpcion1())
    JOptionPane.showMessageDialog(null, "Boton Opcion", "Advertencia", 1);
```

Pero este nos esta sacando error, primero por que hemos cambiado el nombre del método para obtener los botones de opción, ahora se llama **getBOpcion** y ademas este debe exigir que se envié como argumento un numero que indica cual de los 3 botones queremos.
* **1:** Devolverá el botón **bOpcion1**.
* **2:** Devolverá el botón **bOpcion2**.
* **3:** Devolverá el botón **bOpcion3**.

Vamos a arreglar esto, ademas vamos a agregar los if correspondientes a los otros botones y vamos a dejar vaciá cada opción.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if (e.getSource() == loginTemplate.getBOpcion(1))

    if (e.getSource() == loginTemplate.getBOpcion(2))

    if (e.getSource() == loginTemplate.getBOpcion(3))
}
```

En cada opción vamos a cambiar el estado para indicar que botón ha sido seleccionado.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if (e.getSource() == loginTemplate.getBOpcion(1)){
        this.estado = 1;
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3))
        this.estado = 3;
}
```

Antes de cambiar el estado debemos guardar cual era el estado anterior, esto para volver al estado inicial al botón que antes estaba seleccionado.

```javascript
 @Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    if (e.getSource() == loginTemplate.getBOpcion(1))
        this.estado = 1;
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3))
        this.estado = 3;
}
```

También podemos cambiar el estado del botón antes seleccionado para que vuelva a su estado original antes de cambiar el nuevo estado:

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1))
        this.estado = 1;
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3))
        this.estado = 3;
}
```

Una vez se cambio el estado del nuevo botón vamos a cambiar el icono al botón que se acabo de seleccionar: 

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1))
        this.estado = 1;
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3))
        this.estado = 3;
    loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
}
```
Finalmente para iniciar la animación vamos a indicarle a nuestro **Timer** que empiece la animación esto con el método **start()**.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1))
        this.estado = 1;
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3))
        this.estado = 3;
    loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
    timer.start();
}
```

Ahora vamos a crear el método que se encargará de darle movimiento a las imágenes, la llamaremos **moverImagenes**:

```javascript
private void moverImagenes(){
}
```

Lo primero que haremos es un **Switch / Case** que se realizara por medio del estado seleccionado:

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:

            break;
        case 2:
            
            break;
        case 3:
            
            break;
    }
}
```

La animación consiste en mover horizontalmente el panel que contiene las imágenes, en este caso **pSgv**, lo primero que vamos a codificar es el **Punto de freno** el cual va a parar la animación:

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else

            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else

            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else

            break;
    }
}
```

Vamos a explicar un poco lo que realizamos:
* Para el primer caso nuestra referencia será la posición inicial del Panel, este empieza en la posición **100px** con respecto al eje X. Entonces este sera nuestro punto de freno ya que cuando se oprima la primera opción el panel debería estar posicionado como estaba originalmente.
* Para el segundo caso queremos que el panel se mueva hacia la izquierda si empezamos desde la primera opción o hacia la derecha si empezamos desde la tercera opción, hasta el punto en que la segunda imagen pueda ser vista, como la posición inicial del panel es **100px** para que se vea la segunda imágen sea visible, el panel deberá tomar valores negativos en el eje X respecto a toda la ventana, cada imágen tiene un ancho de **500px** y cada una de estas están separadas por **100px** asi que calculamos que debe parar una vez haya recorrido un total de **600px** a la izquierda o derecha, es decir en la posición **-500px (100px - 600px o -1100px + 600px)**.
* Para el tercer caso queremos que el panel se mueva a la izquierda y asi pueda ser mostrada la tercera imagen, nuevamente vamos a recorrer un tramo de **600px**, en este caso desde la perspectiva de la posición inicial, debe ser el doble que la anterior recorriendo **1200px** desde la primera imagen asi que el punto de freno estará cuando el panel se ubique en la posición **-1100px (100px - 1200px o -500px - 600px)**.

Ahora en el caso contrario, cuando no vamos a estar en el **punto de freno** vamos a cambiar la posición del panel por lo que vamos a obtener el panel a traves de la clase compañera **loginTemplate** y le vamos a indicar que vamos a modificar su locación con el método **setLocation**. 

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation();
            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation();
            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation();
            break;
    }
}
```

Para los tres casos el movimiento sera unicamente horizontal por lo que el eje Y no se alterará, así que vamos a dejar esa configuración en los tres casos igual y dejaremos el espació para el eje X que si cambiara para cada caso, para obtener su posición en el eje Y basta con llamar al método **getY**:

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
    }
}
```

Ahora vamos a configurar la animación para cada caso. 

* En el primero (Cuando se oprima el botón **bOpcion1**) suponemos que el Panel ya se ha movido previamente a la izquierda para mostrar alguna de las imágenes restantes, así que para volver a mostrar la primer imágen debemos volver a correr el panel hacia la derecha en este caso sumándole un 1. Para esto debemos obtener primero la posición actual en el eje X del panel con el método **getX** y se le sumara un 1:

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() + 1, loginTemplate.getPSvg().getY()
                );
            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
    }
}
```

* En el tercer caso (Cuando se oprima el botón **bOpcion3**) suponemos que el Panel debe moverse hacia la izquierda para mostrar la tercera imágen sin importar si se ha mostrado la primera o segunda imágen, en este caso para correr el panel a la izquierda le restamos un 1 a la posición actual. Para esto debemos obtener primero la posición actual en el eje X del panel con el método **getX** y se le restará un 1:

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() + 1, loginTemplate.getPSvg().getY()
                );
            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    , loginTemplate.getPSvg().getY()
                );
            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() - 1, loginTemplate.getPSvg().getY()
                );
            break;
    }
}
```

* En el segundo caso (Cuando se oprima el botón **bOpcion2**) dependemos de una dirección ya que pueden haber dos casos, si se esta mostrando la imagen 1, para mostrar la segunda imágen se debe correr el panel hacia la izquierda (es decir restarle un 1 a la posición actual), por otro lado si se está mostrando la imágen 3, para mostrar la segunda imagén se debe recorrer el panel hacia la derecha (es decir sumarle un 1 a la posición actual). 

Para esto vamos a realizar una configuración adicional dentro del **ActionPerformed** en los botones **bOpcion1 y bOpcion3** y sera configurar la dirección.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1)){
        this.estado = 1;
        this.direccion = -1;
    }
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3)){
        this.estado = 3;
        this.direccion = 1;
    }
    loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
    timer.start();
}
```

Ahora en el caso 2 simplemente le vamos a sumar la **dirección** a la posición Actual del panel.

```javascript
private void moverImagenes(){
    switch(estado){
        case 1:
            if(loginTemplate.getPSvg().getX() == 100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() + 1, loginTemplate.getPSvg().getY()
                );
            break;
        case 2:
            if(loginTemplate.getPSvg().getX() == -500)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() + direccion, loginTemplate.getPSvg().getY()
                );
            break;
        case 3:
            if(loginTemplate.getPSvg().getX() == -1100)
                timer.stop();
            else
                loginTemplate.getPSvg().setLocation( 
                    loginTemplate.getPSvg().getX() - 1, loginTemplate.getPSvg().getY()
                );
            break;
    }
}
```

Nuestro método esta listo, ahora vamos a llamar este método al final del **ActionPerformed** justo después de iniciar el **Timer**:

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1)){
        this.estado = 1;
        this.direccion = -1;
    }
    if (e.getSource() == loginTemplate.getBOpcion(2))
        this.estado = 2;
    if (e.getSource() == loginTemplate.getBOpcion(3)){
        this.estado = 3;
        this.direccion = 1;
    }
    loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
    timer.start();
    moverImagenes();
}
```

Si corremos nuestra aplicación veremos algo como esto:

<div align='center'>
    <img  src='./gifs/Animacion1.gif'>
    <p>Primer prueba de animación</p>
</div>

Se puede apreciar la animación y el cambio del estado del botón, sin embargo se puede observar algunos detalles con la animación que no están muy bien, en la siguiente sección vamos a hablar de algunas particularidades para entender como funciona una animación a traves de un **Timer** y como resolver algunos conflictos que son visibles y algunos otros no.

## Particularidades con las animaciones

Como pudimos ver en la anterior demostración hay ciertos conflictos al realizar la animación ya que se ven rastros de las imágenes a medida que se van moviendo, ademas de este existen otras particularidades que debemos revisar.

Para empezar un **Timer** funciona como si fuera un Hilo a traves del método **ActionPerformed** asi que lo que hace es crear un ciclo que recorre todas las instrucciones que hay dentro de este evento hasta que llegue a un **Punto de freno**. Esta situación se cumple salvo ciertas situaciones, ya que si dentro del método **ActionPerformed** hay alguna estructura condicional (if) las instrucciones dentro de este condicional se realizaran una sola vez como si de la activación regular del evento se tratara, en cambio con las instrucciones que están directamente en el método sin estar contenido bajo alguna condición se repetirán hasta que se llegue al **Punto de freno**. A continuación podemos ver cuales se repetirán y cuales no.

<div align='center'>
    <img  src='https://i.imgur.com/6EP1VN7.png'>
    <p>Acciones que se repiten y no dentro del ActionPerformed</p>
</div>

Para comprobar esto vamos a realizar una prueba metiendo en cada condición el comienzo del **Timer** y la llamada del método **moverImagenes**.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    this.estadoAnterior = estado;
    loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
    if (e.getSource() == loginTemplate.getBOpcion(1)){
        this.estado = 1;
        this.direccion = -1;
        timer.start();
        moverImagenes();
    }
    if (e.getSource() == loginTemplate.getBOpcion(2)){
        this.estado = 2;
        timer.start();
        moverImagenes();
    }
    if (e.getSource() == loginTemplate.getBOpcion(3)){
        this.estado = 3;
        this.direccion = 1;
        timer.start();
        moverImagenes();
    }
    loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
}
```

Si ejecutamos nuestra aplicación notaremos algo como esto:

<div align='center'>
    <img  src='./gifs/Animacion2.gif'>
    <p></p>
</div>

Noten que a medida que se da click la imagen se moverá pero solo una vez, asi que es necesario dar varios clicks sobre el botón **bOpcion2** para que la imagen se vaya moviendo más. Ahora bien volviendo a como teníamos el método anteriormente notamos que hay varios problemas aquí, por ejemplo acciones como:
* El cambio del estadoAnterior.
* El Cambio de imagen del botón anteriormente seleccionado.
* El Cambio de imagen del botón que se selecciono.
* El inicio del Timer.

Son acciones que no queremos que se repitan, por que cada vez que el panel se esta moviendo un pixel se están repitiendo estas acciónes y de hecho la única acción que queremos que se repita es la llamada al método **moverImagenes** para que la animación se vea fluida, las anteriores instrucciones aunque no consumen mucho rendimiento dentro de muchas iteraciones como es el caso de una animación empieza a cobrar factura con el rendimiento de nuestra aplicación.

Tenemos varias opciones aquí, podríamos méter todas estas instrucciones en cada una de las opciones, aunque esto hará que repitamos ese código 3 veces, asi que una mejor opción es envolver todas las instrucciones anteriores a la llamadá del método **moverImagenes** en un If que haga una discriminación de clases por ejemplo así:

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() instanceof JButton){
        this.estadoAnterior = estado;
        loginTemplate.getBOpcion(estadoAnterior).setIcon(loginTemplate.getIPunto());
        if (e.getSource() == loginTemplate.getBOpcion(1)){
            this.estado = 1;
            this.direccion = -1;
        }
        if (e.getSource() == loginTemplate.getBOpcion(2))
            this.estado = 2;
        if (e.getSource() == loginTemplate.getBOpcion(3)){
            this.estado = 3;
            this.direccion = 1;
        }
        loginTemplate.getBOpcion(estado).setIcon(loginTemplate.getIPunto2());
        timer.start();
    }
    moverImagenes();
}
```

De esta forma la única instrucción que esta directamente en el **ActionPerformed** sera la llamada del método **moverImagenes** y las otras instrucciones solo se realizaran una vez. Usted puede comprobar la veracidad de esto colocando un mensaje en consola con la instrucción **System.out.println(estadoAnterior);** en ambos casos, y vera que con el código anterior este mensaje se mostrara muchas veces hasta que pare la animación y con el código actual solo se mostrara una vez.

Por otro lado cuando hay movimientos es normal que nuestra interfaz no pueda reaccionar adecuadamente por lo que hay que ayudarle un poco, la forma de hacerlo es diciendole que se debe actualizar por cada iteración que se realice y asi no se vean los rastros de las imágenes mientras se están moviendo. Recordamos que para actualizar la ventana debemos llamar al método **repaint**, en este caso vamos a poner esta instrucción al final del método **moverImagenes** aunque también se podría colocar al final del **ActionPerformed**, en ambos casos el efecto sera igual:

<div align='center'>
    <img  src='https://i.imgur.com/v3HBxJN.png'>
    <p>Actualización de la ventana por cada iteración dentro de la animación.</p>
</div>

Si ejecutamos nuestra aplicación se verá así:

<div align='center'>
    <img  src='./gifs/Animacion3.gif'>
    <p>Animación mejorada</p>
</div>

Podemos observar que hemos realizado nuestra animación y funciona perfectamente.

# Animaciones para muestra de información.

Las animaciones además de darle interactividad a nuestras interfaces gráficas también pueden ser nuestras aliadas para mostrar información adicional que posiblemente no podamos ubicar dentro de las dimensiones de nuestra interfaz. Para ilustrar este ejemplo vamos a enfocarnos en nuestra vista principal y especialmente en el componente **inicio**. Como recordaremos en la parte superior de nuestro componente **Inicio** tenemos una serie de tarjetas que hablan acerca de la organización.

<div align='center'>
    <img  src='https://i.imgur.com/HQUhJfh.png'>
    <p>Vista Principal mostrando el componente Inicio</p>
</div>

Ahora supongamos que la organización quiere mostrar más aspectos sobre ellos en esas tarjetas, podemos comprobar claramente que no tenemos espacio para colocarlas en nuestra interfaz, bien pues podemos posicionarlas más allá de los limites de las dimensiones de nuestra ventana y a traves de dos botones mostrar estas nuevas tarjetas. Vamos a hacerlo.

### **Preparativos para crear la animación.**

Para empezar vamos a agregar 3 nuevas tarjetas a nuestro componente **Inicio** asi que vamos a nuestra clase **InicioTemplate** y vamos a declarar tres nuevos paneles, ya que recordemos que el enfoque de reutilización usado para las tarjetas era de incorporación.

* **Declaración:**
```javascript
private JPanel pUsuarios, pDesarrollo, pGrupo;
```

Ahora bien la animación estará enfocada en las tarjetas, sin embargo son 6 tarjetas que tendremos que animar y configurar la animación para cada una resultara muy tedioso, es por eso que al igual que en el login vamos a crear un Panel que contenga estas tarjetas y de esta forma animar un único panel:

* **Declaración:**

```javascript
private JPanel pTarjetas;
```

Ahora vamos a terminar la creación de los paneles que acabamos de declarar y ademas vamos a cambiar la agregación de los paneles que anteriormente habíamos agregado directamente al componente (**pMision, pVision, pNosotros**) ahora se agregaran al panel **pTarjetas** también.

```javascript
public void crearJPanels(){

    this.pTarjetas = sObjGraficos.construirJPanel(0, 0, 2000, 245, new Color(0, 0, 0, 0), null);
    this.add(pTarjetas);

    this.pMision = sObjGraficos.construirJPanel(20, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pMision);

    this.pVision = sObjGraficos.construirJPanel(296, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pVision);
    
    this.pNosotros = sObjGraficos.construirJPanel(572, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pNosotros);
    
    this.pUsuarios = sObjGraficos.construirJPanel(848, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pUsuarios);
    
    this.pDesarrollo = sObjGraficos.construirJPanel(1124, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pDesarrollo);
    
    this.pGrupo = sObjGraficos.construirJPanel(1400, 15, 256, 230, Color.WHITE, null);
    pTarjetas.add(pGrupo);

    ...
    ...
}
```

Noten que nuestro panel **pTarjetas** tienen un ancho de **2000px** lo cual es muy ancho y solo una parte de este se vera en pantalla, los demás paneles se agregan y se distribuyen sobre este de manera horizontal y uniforme, también tiene un color de fondo transparente. 

Ahora recordemos que cada una de estas tarjetas tienen ciertas imágenes asi que las vamos a crearlas dentro del componente, también vamos a necesitar dos imágenes mas para dos botones que crearemos y que nos ayudaran con la animación: 

* **Declaración:**
```javascript
private ImageIcon iTarjeta4, iTarjeta5, iTarjeta6, iDerecha, iIzquierda, iDimAux;
```

* **Ejemplificación:**
```javascript
// Dentro del método crearObjetosDecoradores
this.iTarjeta4 = new ImageIcon("Clase12/resources/img/tarjeta4.jpg");
this.iTarjeta5 = new ImageIcon("Clase12/resources/img/tarjeta5.jpg");
this.iTarjeta6 = new ImageIcon("Clase12/resources/img/tarjeta6.jpg");
this.iDerecha = new ImageIcon("Clase12/resources/img/derecha.png");
this.iIzquierda = new ImageIcon("Clase12/resources/img/izquierda.png");
```

Ahora vamos a crear los dos botones antes mencionados: 

* **Declaración:**
```javascript
private JButton bDerecha, bIzquierda;
```

* **Método crearJButtons:**
```javascript
public void crearJButtons(){
    iDimAux = new ImageIcon(
        iIzquierda.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
    );

    bIzquierda = sObjGraficos.construirJButton(
        null, 0, 125, 20, 20, sRecursos.getCMano(), iDimAux, null, null, null, null, "c", false
    );
    bIzquierda.addActionListener(inicioComponent);
    this.add(bIzquierda);

    iDimAux = new ImageIcon(
        iDerecha.getImage().getScaledInstance(20, 20, Image.SCALE_AREA_AVERAGING)
    );

    bDerecha = sObjGraficos.construirJButton(
        null, 830, 125, 20, 20, sRecursos.getCMano(), iDimAux, null, null, null, null, "c", false
    );
    bDerecha.addActionListener(inicioComponent);
    this.add(bDerecha);
}
```
Podemos notar que estos Botones se están agregando directamente al componente y no a otro panel creado en el, también notamos que se esta agregando de una vez la escucha a eventos **ActionListener** sin embargo nuestra clase **InicioComponent** aun no ha implementado esta interfaz, esto provocará un error asi que por ahora podemos dejar comentada esas dos lineas de código y en la siguiente sección corregiremos el error.

* **Llamada del método dentro del constructor:**

Para evitar conflictos en el eje Z y para que los botones permanezcan siempre encima de las tarjetas vamos a llamar este método dentro del constructor antes de la construcción de los paneles.

```javascript
public InicioTemplate(InicioComponent inicioComponent){
    ...
    ...
    this.crearObjetosDecoradores();
    this.crearJButtons();
    this.crearJPanels();
    this.crearContenidoPMision();
    this.crearContenidoPVision();
    this.crearContenidoPNosotros();
    ...
    ...
}
```

Se van a crear las otras 3 tarjetas de la misma manera que hicimos con las 3 anteriores, así que se va a crear un método por cada una, podríamos realizar la automatización de estas a traves del uso de servicios pero para no hacer mas larga la sesión continuaremos con el modelo que tenia la aplicación:

* **Tarjeta Usuarios:**

```javascript
public void crearContenidoPUsuarios(){
    this.pUsuarios.add(
        new TarjetaComponent(
            "Nuestros Usuarios", 
            iTarjeta4, 
            "Creamos experiencias de aprendizaje acordes a nuestros usuarios."
        ).getTarjetaTemplate()
    );
}
```

* **Tarjeta Desarrollo:**
```javascript
public void crearContenidoPDesarrollo(){
    this.pDesarrollo.add(
        new TarjetaComponent(
            "Desarrollo", 
            iTarjeta5, 
            "Nuestro Enfoque principal esta en la creación de Software."
        ).getTarjetaTemplate()
    );
}
```

* **Tarjeta Grupo:**
```javascript
public void crearContenidoPGrupo(){
    this.pGrupo.add(
        new TarjetaComponent(
            "El grupo", 
            iTarjeta6, 
            "Nuestra comunidad y el aspecto social es lo mas importante."
        ).getTarjetaTemplate()
    );
}
```

* **Llamada de los métodos en el constructor:**
```javascript
// Dentro del constructor
this.crearContenidoPUsuarios();
this.crearContenidoPDesarrollo();
this.crearContenidoPGrupo();
this.crearContenidoPAcciones();
```

Nuestra aplicación al ejecutarla se verá asi:

<div align='center'>
    <img  src='https://i.imgur.com/FEEiANa.png'>
    <p>Componente inicio después de los cambios realizados</p>
</div>

Hasta el momento no hay un mayor cambio evidente salvo los dos botones a los costados de nuestro componente sin embargo sabemos que nuestras tarjetas están ubicadas después de los limites de nuestra ventana.

Por ultimo vamos a crear los métodos **get** a los dos botones y al panel que contiene las tarjetas:

```javascript
public JButton getBDerecha(){
    return bDerecha;
}

public JButton getBIzquierda(){
    return bIzquierda;
}

public JPanel getPTarjetas(){
    return pTarjetas;
}
```

### **Realizando la animación**

Ahora vamos a concentrarnos en la clase **InicioComponent**, lo primero que haremos es implementar la interfaz **ActionListener** y su respectivo método:

```javascript
public class InicioComponent implements ActionListener {
    ...
    ...
}
```

```javascript
@Override
public void actionPerformed(ActionEvent e) {
}
```

Esto nos debe quitar el error que teníamos en la clase **InicioTemplate** con la adición de la escucha de estos eventos en los botones asi que podemos descomentarlos.

Ahora vamos a crear el objeto **Timer** que es el encargado de gestionar la animación.

* **Declaración:**
```javascript
private Timer timer;
```

* **Ejemplificación:**
```javascript
// Dentro del constructor
timer = new Timer(1, this);
```

Recordemos que por prevención ejemplificamos este objeto antes de crear la inyección con la clase **InicioTemplate**.

Vamos a declarar por ultimo dos atributos enteros que nos ayudaran con el propósito de la animación:
```javascript
private int direccion, posicionInicial;
```
* La dirección nos va indicar hacia que lado se va a mover el panel **pTarjetas**.
* La posición inicial nos indicara la posición exacta del panel **pTarjetas** una vez inicia la animación.

Ahora dentro del método implementado **ActionPerformed** vamos a realizar una discriminación de objetos con nuestros botones:
```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() == inicioTemplate.getBIzquierda())

    if(e.getSource() == inicioTemplate.getBDerecha())
}
```

Dentro de cada condicional vamos a configurar la direccion pero hay que tener algo de cuidado ya que el movimiento que queremos hacer esta cruzado, es decir:
* Cuando oprimamos el boton **bDerecha** queremos ver las tarjetas que están más a la derecha esto quiere decir que debemos correr el panel a la izquierda (-1).
* Cuando oprimamos el boton **bIzquierda** queremos ver las tarjetas que están más a la izquierda esto quiere decir que debemos correr el panel a la derecha (1).

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() == inicioTemplate.getBIzquierda())
        this.direccion = 1;
    if(e.getSource() == inicioTemplate.getBDerecha())
        this.direccion = -1;
}
```

Ahora antes de que empiece la animación queremos en ambos casos capturar la posición inicial de donde se encuentra el panel **pTarjeta** asi que lo pondremos debajo de los condicionales. Recordemos que para capturar la posición en el eje X (nuevamente es el único eje que nos interesa por que solo habrá movimiento sobre este) debemos llamar al método **getX**.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() == inicioTemplate.getBIzquierda())
        this.direccion = 1;
    if(e.getSource() == inicioTemplate.getBDerecha())
        this.direccion = -1;
    posicionInicial = inicioTemplate.getPTarjetas().getX();
}
```

Ahora esta todo listo para que inicie la animación por lo que vamos a activar al Timer:

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() == inicioTemplate.getBIzquierda())
        this.direccion = 1;
    if(e.getSource() == inicioTemplate.getBDerecha())
        this.direccion = -1;
    posicionInicial = inicioTemplate.getPTarjetas().getX();
    this.timer.start();
}
```

Como se explico previamente debemos tener cuidado con las instrucciones que no están bajo una condición dentro del **ActionPerformed** y que no queremos que se repitan, este es el caso de las dos ultimas instrucciones ya que solo necesitamos capturar la **posición inicial** una vez o de lo contrario esta se actualizara todo el tiempo, también queremos activar el timer una sola vez cada que se oprime cualquiera de los botones. Para esto los envolveremos en una discriminación de clases.

```javascript
@Override
public void actionPerformed(ActionEvent e) {
    if(e.getSource() instanceof JButton){
        if(e.getSource() == inicioTemplate.getBIzquierda())
            this.direccion = 1;
        if(e.getSource() == inicioTemplate.getBDerecha())
            this.direccion = -1;
        posicionInicial = inicioTemplate.getPTarjetas().getX();
        this.timer.start();
    }
}
```

Es hora de crear el método que se encargara de la animación pero vamos a crear dos enfoques que serán explicados en la siguiente sección.

## Enfoques de animación.

A continuación vamos a realizar dos enfoques de animaciónes:
* El primero mostrará las otras 3 tarjetas una vez se oprima el boton de la derecha y volverá a mostrar las 3 tarjetas anteriores una vez se oprima el botón de la izquierda, es decir que solo habrá un movimiento para mostrar las 3 primeras tarjetas o las 3 ultimas.
* El segundo ira recorriendo las tarjetas pero moviendo en tramos cortos ya sea a la izquierda o derecha, esto quiere decir que para mostrar completamente las tarjetas que están de ultimas se deberá oprimir varias veces el boton de la derecha, igualmente en el caso contrario.

Para esto vamos a crear dos métodos:

```javascript
public void moverTarjetas1(){

}

public void moverTarjetas2(){

}
```

### **Enfoque #1**

Vamos a concentrarnos primero en el método **moverTarjeta1** y vamos a definir primero los **puntos de freno** para eso realizamos un if:
```javascript
public void moverTarjetas1(){
    if(/*Condición para que la animación se detenga*/)
        timer.stop();
    else
}
```

En este caso tenemos 2 **Puntos de Freno**:
* Cuando las primeras 3 tarjetas se están mostrando completamente (como en el estado inicial) y se quiera oprimir el botón **bIzquierda** no debería poder correr el panel más a la derecha ya que no hay mas tarjetas a la izquierda por mostrar. Es decir que cuando el panel esta posicionado en **0px** con respecto al eje X y se quiera oprimir el boton de la izquierda (**1 en dirección**) la animación se va a detener:

```javascript
public void moverTarjetas1(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1
    )
        timer.stop();
    else
}
```

* Cuando las ultimas 3 tarjetas se están mostrando completamente y se quiera oprimir el botón **bDerecha** no debería poder correr el panel más a la izquierda ya que no hay mas tarjetas a la derecha por mostrar. Es decir que cuando el panel esta posicionado en **-830px** con respecto al eje X y se quiera oprimir el boton de la derecha (**-1 en dirección**) la animación se va a detener:

```javascript
public void moverTarjetas1(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        timer.stop();
    else
}
```

Ahora en el caso contrario simplemente vamos a indicarle al panel que se mueva sumándole la dirección configurada previamente en el **ActionPerformed**, con el método **setLocation**. Como lo hicimos en el login, la posicion en Y la dejaremos intacta asi que vamos a dejarla con un **getY** mientras que en la posición X se realizara el proceso antes mencionado:

```javascript
public void moverTarjetas1(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        timer.stop();
    else
        inicioTemplate.getPTarjetas().setLocation(
            inicioTemplate.getPTarjetas().getX() + direccion, inicioTemplate.getPTarjetas().getY()
        );
}
```

Finalmente y para que la animación pueda verse fluidamente y sin conflictos de rastros en el movimiento vamos a actualizar la ventana por cada iteración en la animación:
```javascript
public void moverTarjetas1(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        timer.stop();
    else
        inicioTemplate.getPTarjetas().setLocation(
            inicioTemplate.getPTarjetas().getX() + direccion, inicioTemplate.getPTarjetas().getY()
        );
    inicioTemplate.repaint();
}
```

Vamos a llamar al método justo al final del **ActionPerformed** y como queremos que este método se repita en cada iteración de la animación hasta que se detenga la vamos a dejar afuera del condicional: 

<div align='center'>
    <img  src='https://i.imgur.com/STRGira.png'>
    <p>Llamada del método moverTarjetas1 en ActionPerformed</p>
</div>

Una vez corramos nuestra aplicación veremos algo como esto:
<div align='center'>
    <img  src='./gifs/Animacion4.gif'>
    <p>Primer Enfoque de animación implementado</p>
</div>

### **Enfoque #2**

Ahora nos concentraremos en el método **moverTarjetas2**, recordemos que en este no queremos mostrar las otras tarjetas en una sola animación sino que vamos a recorrer todo el panel **pTarjetas** en tramos.

Podemos guiarnos del anterior método implementado que usaba los **Puntos de frenos** ya que los limites serán los mismos para ambos casos sin embargo para este caso este no sera nuestros **Puntos de freno** son solo los limites, asi que en lugar de parar la animación vamos a indicarle al programa que en esos casos no realice nada, esto con la instrucción **assert true**:

```javascript
public void moverTarjetas2(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        assert true;
    else

}
```

Ahora para el caso contrario vamos a tener que configurar nuestros **puntos de freno** reales:

```javascript
public void moverTarjetas2(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        assert true;
    else{
        if(/*Condición para que la animación se detenga*/)

        else
    }
}
```

En este caso queremos que cada vez que oprimamos el botón **bDerecha o bIzquierda** el panel tenga un movimiento horizontal de **200px** ya sea a la izquierda o derecha. Asi que nuestros **Puntos de freno** serán cuando el panel **pTarjetas** haya recorrido esta cantidad de pixeles. Para poder reconocer si el panel recorrió esta cantidad necesitamos de nuestro atributo **posicionInicial** que se configuro previamente en el **ActionPerformed**:

```javascript
public void moverTarjetas2(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        assert true;
    else{
        if( 
            inicioTemplate.getPTarjetas().getX() == posicionInicial + 200 ||
            inicioTemplate.getPTarjetas().getX() == posicionInicial - 200 
        )
            timer.stop();
        else
        
    }
}
```

Ahora falta configurar nuestra animación que será la misma para en este caso, mover el panel sumandole la **dirección** que fue configurada anteriormente en el **ActionPerformed**:

```javascript
public void moverTarjetas2(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        assert true;
    else{
        if( 
            inicioTemplate.getPTarjetas().getX() == posicionInicial + 200 ||
            inicioTemplate.getPTarjetas().getX() == posicionInicial - 200 
        )
            timer.stop();
        else
            inicioTemplate.getPTarjetas().setLocation(
                inicioTemplate.getPTarjetas().getX() + direccion, inicioTemplate.getPTarjetas().getY()
            );
    }
}
```

Para terminar con el método vamos a actualizar la ventana por cada iteración de la animación: 

```javascript
public void moverTarjetas2(){
    if(
        inicioTemplate.getPTarjetas().getX() == 0 && direccion == 1 ||
        inicioTemplate.getPTarjetas().getX() == -830 && direccion == -1
    )
        assert true;
    else{
        if( 
            inicioTemplate.getPTarjetas().getX() == posicionInicial + 200 ||
            inicioTemplate.getPTarjetas().getX() == posicionInicial - 200 
        )
            timer.stop();
        else
            inicioTemplate.getPTarjetas().setLocation(
                inicioTemplate.getPTarjetas().getX() + direccion, inicioTemplate.getPTarjetas().getY()
            );
    }
    inicioTemplate.repaint();
}
```


Finalmente para probar este enfoque vamos a llamar ahora a este método, quitando la llamada del anterior en el **ActionPerformed**:

<div align='center'>
    <img  src='https://i.imgur.com/lO3WvQk.png'>
    <p>Llamada del método moverTarjetas2 en el ActionPerformed</p>
</div>

Nuestra animación se vera así:

<div align='center'>
    <img  src='./gifs/Animacion5.gif'>
    <p>Implementación de segundo enfoque.</p>
</div>

# Resultado

Si llegaste hasta aquí **!Felicidades!** has aprendido a crear animaciones con ayuda del objeto **Timer**, ademas aprendiste algunas particularidades sobre el uso de este para crear animaciones. Vimos varios usos de las animaciónes y revisamos finalmente varios enfoques para realizar una misma animación. Ahora es turno de que implementes estos conocimientos, la imaginación es el limite. 

En la siguiente sesión vamos a ver como dibujar formas, textos, imágenes y demás a traves de un lienzo conocido como **Canvas**.

# Actividad

Implementar las animaciones en tu proyecto para añadirle más interactividad a tus interfaces gráficas.