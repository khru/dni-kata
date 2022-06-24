# Repasando conceptos

## Qué es eso del DNI (si no eres de España)

Un DNI (Documento Nacional de Identidad) es un identificador que consta de ocho cifras numéricas y una letra que actúa como dígito de control. Existen algunos casos particulares en los que el primer número se sustituye por una letra y ésta, a su vez, por un número para el cómputo de validez que viene a continuación. Este último es el caso del NIE o Número de Identificación para Extranjeros residentes.

El algoritmo para validar un DNI es muy sencillo: se toma la parte del numero del documento y se divide entre 23 y se obtiene el resto. Ese resto es un índice que se consulta en una tabla de letras. La letra correspondiente al índice es la que debería tener un DNI válido. Por tanto, si la letra del DNI concuerda con la que hemos obtenido, es que es válido.

La tabla en cuestión es esta:

| Resto | Letra |
|------|-------|
| 0 | T |
| 1 | R |
| 2 | W |
| 3 | A |
| 4 | G |
| 5 | M |
| 6 | Y |
| 7 | F |
| 8 | P |
| 9 | D |
| 10 | X |
| 11 | B |
| 12 | N |
| 13 | J |
| 14 | Z |
| 15 | S |
| 16 | Q |
| 17 | V |
| 18 | H |
| 19 | L |
| 20 | C |
| 21 | K |
| 22 | E |

## Qué es un Value Object

Value Object es un tipo de objeto que representa un concepto importante de un dominio el cual nos interesa por su valor, y no por su identidad. Esto quiere decir que dos Value Object del mismo tipo se consideran iguales e intercambiables si representan el mismo valor.

En el mundo físico tenemos un gran ejemplo de Value Object: el dinero. Los billetes de 10 euros, por ejemplo, representan todos la misma cantidad, y da igual el ejemplar concreto que tengamos, siempre representará 10 euros y lo podremos cambiar por otro del mismo valor, o por una combinación de billetes y monedas que sumen el mismo valor. Los billetes, de hecho, tienen una identidad (tienen un número de serie) pero no se tiene en cuenta para su utilización como medio de pago.

El valor representado por un billete no cambia. En el ámbito de la programación, los Value Objects tampoco pueden cambiar de valor a lo largo de su ciclo de vida: son inmutables. Se instancian con un valor determinado que no puede cambiarse. Para tener un valor nuevo se debe instanciar un objeto nuevo de ese tipo con el nuevo valor.

Para instanciar un Value Object debemos asegurarnos de que los valores que le pasamos nos permiten hacerlo de forma consistente, por lo que serán importantes las validaciones. Lo bueno, es que una vez creado, siempre podemos confiar en que ese Value Object será válido y lo podemos usar sin ningún problema.


## La *kata* del DNI

Nuestro ejercicio consistirá en crear un Value Object que nos sirva para representar un DNI o NIF. Por tanto, queremos que se pueda instanciar un objeto solo si tenemos un DNI válido. Así que vamos a ello.

Lo que queremos es algo así:


```php

$validDni = new Dni('04560732Q');

printf('%s is a valid DNI', (string) $validDni);

//

$invalidDni = new Dni('12345678K');

>>> Throws Exception
```

# ¿Qué vamos a testear?

Esencialmente, un DNI no es más que una cadena de caracteres con un formato específico. De todas las cadenas de caracteres que se podrían generar solo un subconjunto de ellas cumplen todas las condiciones exigidas para ser un DNI. Estas condiciones se pueden resumir en:

* Son cadenas de 9 caracteres.
* Los primeros 8 caracteres son números, y el último es una letra.
* La letra puede ser cualquiera, excepto U, I, O y Ñ.
* La última letra se obtiene a partir de un algoritmo que la consulta de una tabla a partir de obtener el resto de dividir la suma de los dígitos numéricos entre 23. Si la letra suministrada no se corresponde con la calculada, el DNI no es válido.
* El primer carácter puede ser X, Y o Z, lo que indica un NIE (Número de identificación para personas extranjeras).
* Para la validación, las letras XYZ se reemplazan por 0, 1 ó 2, respectivamente.

En caso de que alguna de las condiciones no se cumple, el DNI no es válido.

Si nos fijamos en las condiciones recogidas en la lista anterior, vemos que cada una de ellas reduce el número de cadenas de caracteres candidatas a ser un DNI.


## Ejemplos validos
*DNI*
```
31970165G
10448738E
68163822X
68132163E
50791233B
90250990W
87477013D
34272318H
54956042A
78176129A
49390008S
90583399S
08004624A
00062477D
94985972C
87819112Y
92683017D
17402629R
17206298K
24473205D
```
*NIE*
```
Z5090857L
Y9661016H
Y1019582Y
Y2517413P
Z5470399S
Y0468622B
X4326926M
Z6552219F
X8327408M
X1404966B
Z4791181X
Y5545378T
X0098688H
Z1246847E
Z0178750E
Y3699208V
Y0326894D
Z3936888Y
Y0375340V
X0715825L
```


# Agradecimientos a el creador
[Fran Iglesis](https://franiglesias.github.io/iniciacion-tdd/)

---

# Base para hacer tests

Configuración básica para empezar a hacer una kata o aprender a hacer tests en los siguientes lenguajes:

- PHP y PHPUnit
- Javascript con Jest
- Java, Junit y Mockito
- Scala, Munit y Scalacheck
- Kotlin, JUnit5 y MockK

# Configuración específica por lenguaje

## PHP
1. Instalar [composer](https://getcomposer.org/) `curl -sS https://getcomposer.org/installer | php`
2. `composer install` (estando en la carpeta php)
3. `./vendor/bin/phpunit`

## Javascript
1. Instalar [Node](http://nodejs.org/)
2. `npm install` (Estando en la carpeta javascript)
3. `npm test`

## Java
1. Instalar las dependencias y tests con Maven [mvn test]
2. Ejecutar los tests con el IDE

## Scala
1. `sbt` (en la carpeta scala)
2. `~test` para ejecutar los test en hot reload

### Linux/Mac
1. Instalar [SDKMan](https://sdkman.io/)
2. `sdk install java 11.0.12-open` instala OpenJDK
3. `sdk install sbt` una vez instalado SDKMan

### Windows
1. Instalar [OpenJDK](https://docs.microsoft.com/es-es/java/openjdk/download#openjdk-110141-lts--see-previous-releases)
2. Instalar [SBT](https://www.scala-sbt.org/download.html)

### Visual Studio Code
1. Descargar [Visual Studio Code](https://code.visualstudio.com/)
2. Instalar para VS Code [Metals](https://scalameta.org/metals/docs/editors/vscode)

## Kotlin
1. Por consola: Puedes instalar dependencias y lanzar los tests con `gradlew test`
2. Usando IDE: Simplemente abre el proyecto desde el raiz de la plantilla Kotlin

# Documentación
## Javascript
[Jest](https://jestjs.io)

## PHP
[PHPUnit](https://phpunit.readthedocs.io/)

[Prophecy](https://github.com/phpspec/prophecy) para dobles de prueba

## Java
[JUnit](https://github.com/junit-team/junit/wiki)

[Mockito](http://site.mockito.org/mockito/docs/current/org/mockito/Mockito.html)

## Scala

[Munit](https://scalameta.org/munit/docs/tests.html)

[Scalacheck](https://github.com/typelevel/scalacheck/blob/main/doc/UserGuide.md) para testing basado en propiedades

## Kotlin
[JUnit5](https://junit.org/junit5/)

[MockK](https://mockk.io/)
