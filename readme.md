


Angular 2: Reactive Forms - Snapshot 3
===================
En esta parte veremos estos contenidos del curso de Pluralshight:

 - Validations

----------
### 1 - Validations
----------
Como ya hemos visto antes Template-driven utiliza los atributos HTML para poder validar. ¿Pero qué ocurre si nuestras validaciones dependen de un estado de aplicación?

Reactive Forms nos va a ayudar en esa tarea haciendo fácil implementar ese tipo de validaciones.

#### Setting built-in validations rules

Reactive Forms permite utilizar validaciones comunes como hicimos en HTML. Para ello utilizamos **Validators** del módulo @angular/forms

Dentro del FormBuilder pasaremos un array donde el primer valor será el valor por defecto y el segundo un Validator o un array de ellos. La sintaxis seria así:

![enter image description here](https://i.imgur.com/3JaL8vU.png)
#### Ajusting validation rules at runtime
En muchas ocasiones necesitamos cambiar nuestras validaciones dependiendo de un determinado estado o comportamiento. Para ello necesitaremos estos métodos del FormControl:
![enter image description here](https://i.imgur.com/uxq2KyA.png)
#### Custom validators
En ocasiones es necesario tener validaciones personalizadas en nuestra aplicación. La forma de hacerlo sería mediante esta función:
![enter image description here](https://i.imgur.com/9EPFOqr.png)

####  Custom validation with parameters
De manera muy similar podemos crear validadores con parámetros. Para ello necesitamos encapsular la función de arriba en otra y entonces hacer e uso de los parámetros:
 ![enter image description here](https://i.imgur.com/mmbE1se.png)

Un ejemplo para validar que el valor este entre dos números seria:

    import { AbstractControl, ValidatorFn } from '@angular/forms';
    
    export class NumberValidators {
    
        static range(min: number, max: number): ValidatorFn {
            return (c: AbstractControl): { [key: string]: boolean } | null => {
                if (c.value && (isNaN(c.value) || c.value < min || c.value > max)) {
                    return { 'range': true };
                }
                return null;
            };
        }
    }

####  Cross-field validation: Nested  FormGroup
Una característica muy importante que nos aporta Reactive Forms es la capacidad de hacer validaciones de un grupo de FormControl

![enter image description here](https://i.imgur.com/Qvox3Fe.png)

####  Cross-field validation: Custom validation

En muchas ocasiones esto no es suficiente y tenemos que implementar validaciones personalizadas dentro de un Nested FormGroup y validarlo en conjunto. Un ejemplo podría ser este:

![enter image description here](https://i.imgur.com/ztImuDo.png)

Un ejemplo de cómo hacer un validador que compare el valor de dos controles sería:

    import { AbstractControl, ValidatorFn } from '@angular/forms';
    
    export class StringValidators {
    
        static controlValueMatcher(name1: string, name2: string): ValidatorFn {
            return (c: AbstractControl): { [key: string]: boolean } | null => {
    
                let control1 = c.get(name1);
                let control2 = c.get(name2);
                if(control1.pristine || control2.pristine){
                    return null;
                }
                if(control1.value === control2.value){
                    return null;
                }
                return { 'match': true };
            };
        }
    }

----------
### 2 - Práctica
----------
El objetivo de esta práctica es transformar las validaciones HTML a validaciones de Reactive Forms. Tiene especial interés crear validadores personalizados, aquí crearemos dos, el primero substituye al patrón que teníamos para validar el rango y el segundo comprobará cuando se crea un producto que el código de producto coincida con su comprobador.



Para ello clonamos el **SnapShot 3** desde el primer commit:

    git clone https://github.com/tc-frontend/course_angular2_day2_snapshot3
    cd course_angular2_day2_snapshot3
    git checkout tags/init
    npm install

y hacer los pasos detallados en el historial de commits:

https://goo.gl/bWHWxe

----------
#### 1 - Eliminar los atributos de validación e implementarlos en la clase
Tenemos que eliminar todos esos atributos e ir añadiéndolos en los controles creados con el FormBuilder. Tenemos que importar la referencia Validators dentro de '@angular/forms'

https://goo.gl/2F2Y5F

----------
#### 2 - Implementar lógica de validación

Dependiendo de la disponibilidad seleccionada tenemos que aplicar unas reglas de validación u otras. Si esta seleccionado tenemos que comprobar que el usuario seleccione un motivo. En el caso contrario no deberá validarlo

También si el formulario esta en modo edición no tenemos que validar que el campo de confirmación de código de producto este relleno. En el caso de que estemos creando un producto si se tiene que validar

https://goo.gl/moaEmt

----------
#### 3 - Crear una validación personalizada para comprobar un rango para el rating

Tenemos que sustituir el patrón por una validación personalizada. Esta validación deberá estar contenida en "shared"

https://goo.gl/8nAeN5

----------
#### 4 - Agrupar los controles de código de producto

Tenemos que agrupar estos controles para luego aplicar una validación conjunta y verificar que los valores coinciden.

https://goo.gl/AvL6kS

----------
#### 5 - Control personalizado para verificar que los valores de los controles código de producto coinciden

Esta validación la incluiremos dentro del módulo "shared"

https://goo.gl/37sfSH

----------
Si queremos ver la App en nuestro browser

    npm start

Si queremos ver la solución final de este SnapShot:

    git checkout master
    npm install
    npm start







