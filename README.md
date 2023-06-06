# **Tarjeta de evaluación para el sensor PIR D203S**

Hola!

En este repositorio encontrarás toda la información de funcionamiento y desarrollo de un sensor PIR
para el curso de electrónica analógica 3 del periodo 2023-1.

Antes que nada, nos gustaría que sepas quienes estamos detrás de esto:

  - Laura Nicolle García Villareal (20202192838)
  - Laura Sofía Polania Mendez (20211197481)
  - Juan Esteban Vargas Rivera (20202191289)
  - Cesar Diego Vargas Motta (20202191503)

Pertenecemos al programa de ingeniería electrónica de la universidad surcolombiana.

## **¿Qué es un PIR?**

Un sensor PIR (del inglés Passive Infrared Sensor, que significa "sensor infrarrojo pasivo") es un dispositivo electrónico utilizado para detectar la presencia de objetos o personas en base a los cambios en la radiación infrarroja emitida por ellos. Estos sensores son comúnmente utilizados en sistemas de seguridad, iluminación automática y control de energía en edificios.

El funcionamiento del sensor PIR se basa en la detección de los cambios en la radiación infrarroja emitida por los cuerpos en movimiento. Los seres humanos y otros objetos emiten radiación infrarroja en forma de calor, y el sensor PIR es capaz de detectar estos cambios en el patrón de calor para identificar la presencia de movimiento.

El sensor PIR está compuesto por una serie de elementos detectores, como piroeléctricos o termopilas, que generan una señal eléctrica cuando se produce un cambio en el patrón de radiación infrarroja. Esta señal eléctrica se procesa y se utiliza para activar o desactivar dispositivos conectados al sensor, como alarmas, luces o sistemas de seguridad.

Es importante destacar que los sensores PIR son pasivos, lo que significa que no emiten ningún tipo de radiación para detectar movimiento. En su lugar, solo responden a los cambios en la radiación infrarroja existente en su entorno. Esto los hace eficientes en términos de consumo de energía y los convierte en una opción popular para aplicaciones de detección de movimiento en diversas áreas.

## **Acondicionamiento (Etapas)**

La señal de un sensor PIR generalmente requiere ser acondicionada antes de ser utilizada en otros circuitos o dispositivos. El acondicionamiento de la señal se realiza para mejorar su calidad, amplificarla o adaptarla a las necesidades del sistema en el que se va a utilizar. A continuación, te presento los pasos típicos para acondicionar la señal de un sensor PIR:

  1. Amplificación: La señal del sensor PIR suele ser débil, por lo que puede requerir amplificación. Esto se logra utilizando un amplificador operacional u otro circuito amplificador adecuado para aumentar la amplitud de la señal.

  2. Filtrado: El siguiente paso es aplicar un filtrado a la señal para eliminar ruidos o frecuencias no deseadas. Se pueden utilizar filtros activos o pasivos, como filtros RC (resistencia-condensador) o filtros pasa-bajos, dependiendo de los requerimientos específicos del sistema.

  3. Acondicionamiento de nivel: En algunos casos, es necesario ajustar el nivel de voltaje o corriente de la señal para que sea compatible con otros componentes o dispositivos del sistema. Esto se logra mediante el uso de circuitos acondicionadores de nivel, como amplificadores de nivel o circuitos comparadores.

  4. Conversión de señal: Si es necesario, la señal puede requerir ser convertida a otro formato o tipo de señal. Por ejemplo, si la señal del sensor PIR es analógica pero el sistema utiliza una entrada digital, se puede utilizar un conversor analógico a digital (ADC) para convertir la señal.

  5. Procesamiento adicional: Dependiendo de las necesidades específicas del sistema, es posible que se requiera realizar un procesamiento adicional a la señal acondicionada. Esto puede incluir operaciones como la detección de umbrales, el conteo de pulsos o la integración de la señal.

Es importante tener en cuenta que el acondicionamiento de la señal puede variar según la aplicación y los requisitos del sistema en el que se utiliza el sensor PIR. Los pasos mencionados anteriormente son una guía general, pero pueden adaptarse y personalizarse según las necesidades específicas de cada caso.

### **Etapa 1**

La primera etapa arquitectónica amplifica la señal. Cancela la parte DC de la señal yfiltra el ruido de alta frecuencia que podría conducir a detecciones falsas. El esquema de este La primera etapa arquitectónica se muestra en la imagen "Etapa1".

La figura 3 muestra que el ruido se filtra gracias a los componentes R1 y C1. el corte la frecuencia es de 5 Hz (fhigh1 = 1/(2 x π x R1 x C1)). Esta aplicación no necesita funcionar en frecuencias más altas porque generalmente estamos detectando movimiento humano. El segundo filtro se utiliza para rechazar la parte de CC de la señal. R2 y C2 realizan un pase alto filtro que tiene una frecuencia de corte de 0,6 Hz (flow1 = 1/(2 x π x R2 x C2)). 

Como no amplificamos la parte de CC de la señal, el Vio, que es el voltaje de compensación de entrada del amplificador operacional, no tiene importancia en esta aplicación.
La ganancia de etapa es 53,3 (Ganancia1 = 1 + R1/R2). Esta amplificación permite una señal utilizable que es superior al nivel de ruido. El sesgo DC de la primera etapa arquitectónica está determinado por el sensor. Para evitar la saturación, la ganancia no debe ser demasiado grande porque la amplificación se hace alrededor de este voltaje de modo común (que no es Vcc/2).

El amplificador operacional TSU101 de STMicroelectronics encaja perfectamente en esta etapa arquitectónica. Así como Al tener un consumo muy bajo, el TSU101 opera con un consumo de corriente típico de solo 600 nA a Vcc = 3,3 V.

### **Etapa 2**

En cuanto a la función de filtrado, las frecuencias de corte bajas y altas respectivamente son de 0,6 Hz (flujo2 = 1/(2 x π x R4 x C4)) y 5 Hz (fhigh2 = 1/(2 x π x R3 x C3)). La ganancia de esta etapa es -52,3 (Ganancia2 = -R3/R4). Esta ganancia significa que después de la etapa 2 la señal entre 0,6 Hz y 5 Hz habrá sido amplificado 2790 veces (69 dB).

En la etapa 2, el puente divisor compuesto por R6 establece el voltaje de modo común del amplificador operacional, R7, R8 y R9 a Vcc/2. Esto permite una mayor amplificación de la señal de CA.

Al igual que con la etapa 1, el Vio del amplificador operacional no tiene importancia ya que la parte de CC de la señal es no amplificado. Además, no hay restricción en el producto de ganancia de ancho de banda (GBP), porque el amplificador operacional tiene que tener un GBP superior a 2,6 kHz (fhigh2 x gain x 10). un factor de
10 se ha utilizado para evitar cualquier limitación por parte de la GBP. Casi todos los amplificadores GBP se ajustan a esto
Requisito de 2,6 kHz.

El amplificador operacional TSU101 es muy adecuado para esta etapa arquitectónica. Su bajo consumo es particularmente ventajoso. Aunque es un amplificador operacional de nanopotencia, su GBP es más alto que 2,6kHz Tenga en cuenta que TSU101 también está disponible en canal doble (TSU102) y cuádruple (TSU104) versiones para optimizar la huella a bordo

### **Etapa 3**

La tercera y última etapa arquitectónica permite al usuario realizar un comparador de ventanas. Como consecuencia, la señal está perfectamente acondicionada para ir a un microcontrolador.

Cuando se detecta una fuente de calor, la salida del amplificador operacional U3 y/o U4 está en estado bajo. El puente divisor, compuesto por las resistencias R6, R7, R8 y R9, se utiliza para establecer la referencia de voltaje de estos dispositivos. El amplificador operacional TSU101 es un amplificador operacional de riel a riel de entrada/salida, y no hay restricciones en el voltaje de modo común de entrada. Por lo tanto, mientras las referencias de voltaje de U3 y U4 estén dentro del rango de Vcc, el comparador de ventana funciona. Cuando la señal (Vout2) está por encima de esta referencia (2,77 V si Vcc = 3,3 V), la salida de U3 está en estado bajo, cerca de tierra.



