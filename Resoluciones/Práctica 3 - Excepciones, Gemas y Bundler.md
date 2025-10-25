
### Mini resumen de que son las cosas mencionadas 

💎 **Gemas** 
Las Gemas son librerías que amplían las funcionalidades de Ruby.
Ej : Existen gemas para conectarse a una base de datos, manejar JSON o crear aplicaciones webs (como RAILS)

```c
gem install colorize
```

```c
requiere 'colorize'
puts "hola mundo".colorize(:blue) 
```

> Las Gemas se descargan y publican desde : RubyGems.org


📘 **Bundler**
Blunder es una herramienta que administra las gemas que son usadas en el proyecto.
Lo que permite : 
- Definir que gemas necesita y en que versión
- Instalar exactamente esas versiones para todos los que trabajen en el proyecto
Se usa un archivo `Gemfile` para declararlas.

```c
#Gemfile

source "https://rubygems.org"

gem "sinatra"
gem "colorize", "~> 0.8"
```

Para luego hacer : 

```c
bundle install
```

Crea un `Gemfile.lock` con las versiones exactas instaladas. 
Y para ejecutar con esas gemas : 

```c
bundle exec ruby app.rb
```
> Este comando lo que hace es decirle a Ruby que ejecute la app usando las gemas que están definidas en **Gemfile/Gemfile.lock**.
> Esto lo que hace es evitar que usemos Gemas instaladas globalmente con versiones distintas.


### Excepciones 

#### Ejercicio 1

Investiga la jerarquía de clases que presenta Ruby para las excepciones. ¿Para qué se utilizan las siguientes clases? 
-  ArgumentError 
- IOError 
- NameError 
- NotImplementedError 
- RuntimeError 
- StandardError 
- StopIteration 
- SystemExit 
- SystemStackError 
- TypeError 
- ZeroDivisionError

**Respuesta** : 

Todas las excepciones en Ruby heredan de la clase base : 
```
Exception
 ├── NoMemoryError
 ├── ScriptError
 │    ├── LoadError
 │    ├── NotImplementedError
 │    └── SyntaxError
 ├── SecurityError
 ├── SignalException
 │    └── Interrupt
 ├── StandardError
 │    ├── ArgumentError
 │    ├── IOError
 │    ├── NameError
 │    │    └── NoMethodError
 │    ├── RuntimeError
 │    ├── StopIteration
 │    ├── SystemCallError
 │    ├── TypeError
 │    └── ZeroDivisionError
 ├── SystemExit
 └── SystemStackError
```
> Cuando usamos el comando `rescue` sin especificar nada, Ruby lo que haces es capturar las excepciones que heredan de *StandardError* 

1. ArgumentError : Se lanza cuando un método recibe un numero o un tipo de argumento incorrecto. 
2. IOError : Ocurre cuando hay problemas con operaciones de Entrada/Salida (como leer o escribir archivos, sockets, etc).
3. NameError : Se lanza cuando se intenta usar una variable o constante que no esta definida.
4. NotImplementedError : Básicamente indica que un método no fue implementado en una clase. Se usa cuando una clase base define un método que las subclases deben sobrescribir.
5. RuntimeError : Es la excepción por defecto que se lanza con `raise` si no se especifica otra clase. 
6. StandardError : Clase base de la mayoria de las excepciones comunes. Si hacemos un `rescue` sin indicar una clase, capturamos `StandardError` y sus subclases. 
7. StopIteration : Se lanza cuando un iterador (Enumerator) llega al final. 
8. SystemExit : Se genera cuando se llama a `exit` . Permite salir del programa ordenadamente.
9. SystemStackError : Se lanza cuando ocurre un desbordamiento de pila  (stack overflow), normalmente por una recursión infinita. 
10. TypeError : Ocurre cuando se usa un **tipo de dato no válido** para una operación.

#### Ejercicio 2 

¿Cuál es la diferencia entre raise y throw? ¿Para qué usarías una u otra opción?

Respuesta : 

RAISE es usado para levantar excepciones. Estas son manejadas con : 
```c
begin 
....
rescue ...
end
```

Es usado cuando algo salió mal en el flujo normal del programa y queremos interrumpirlo con un error controlado. 

Ej : 
```c
def dividir(a, b)
  raise ZeroDivisionError, "No se puede dividir por cero" if b == 0
  a / b
end

begin
  dividir(10, 0)
rescue ZeroDivisionError => e
  puts "Error: #{e.message}"
end
```
> Raise crea un objeto Exception.
> El flujo se interrumpe y Ruby **busca el bloque `rescue` más cercano**.

THROW no es para errores. Es usado para salir de bucles o bloques anidados de manera controlada. 
Funciona parecido a hacer un break pero es más flexible, permitiendo salir de múltiples niveles. 

Ej : 
```c
catch(:fin) do
  (1..10).each do |i|
    throw(:fin) if i == 5
    puts i
  end
end

puts "Terminó"
```

La salida seria : 
```
1
2
3
4
Terminó
```

> catch lo que hace es definir un punto de salida con un símbolo o etiqueta (`:fin`).
> `throw(:fin)` salta directamente fuera de ese bloque.
> No tiene nada que ver con excepciones ni `rescue`.



#### Ejercicio 3

¿Para qué sirven begin .. rescue .. else y ensure ? 
Pensá al menos 2 casos concretos en que usarías estas sentencias en un script Ruby.

Respuesta : 

```
begin
     # Código que podría fallar
rescue TipoDeError => e
     # Qué hacer si ocurre un error
else
     # Qué hacer si NO ocurrió ningún error
ensure
     # Qué hacer SIEMPRE (haya o no error)
end
```

Las usaría cuando trabajo con archivos o una base de datos. 

#### Ejercicio 4

¿Para qué sirve retry? ¿Cómo podés evitar caer en un loop infinito al usarla?

Respuesta : 
La operación `retry` es utilizada dentro de los bloques **`begin ... rescue`** para reintentar ejecutar el bloque completo después  de capturar una excepción.
Nos es útil cuando un error puede ser temporal y tiene sentido volver a intentarlo. 

Puede pasar que el error nunca desaparezca, y el bloque retry hará que el bloque se ejecute una y otra vez sin parar, bloqueando el programa. 

Opciones para evitarlo  : 
1. Tener un contador de intentos
2. Un delay entre intentos


#### Ejercicio 5

¿Para qué sirve redo? ¿Qué diferencias principales tiene con retry?

Respuesta : 

Redo es utilizado dentro de bucles (while, for, each). 
Hace que la iteración actual del bucle se repita, sin reevaluar la condición del bucle ni pasar a la sig iteración. 

Ej : 
```c
i = 0
while i < 3
  i += 1
  puts "Intento #{i}"
  redo if i == 2
end
```

```
Intento 1
Intento 2
Intento 2
Intento 3
```
>`redo` no reinicia ni resetea variables. Por eso es que en la tercer linea tenemos "Intento 2".


`redo` :  repite **solo la iteración actual** de un bucle.
`retry` : repite **todo un bloque `begin`** después de un error.

#### Ejercicio 6

Analizá y probá los siguientes métodos, que presentan una lógica similar, pero ubican el manejo de excepciones en distintas partes del código. ¿Qué resultado se obtiene en cada caso? ¿Por qué?

```c
def opcion_1
  a = [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]
  b = 3
  c = a.map do |x|
    x * b
  end
  puts c.inspect
rescue
  0
end

//-------------------------

def opcion_2
  c = begin
    a = [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]
    b = 3
    a.map do |x|
      x * b
    end
  rescue
    0
  end
  puts c.inspect
end

// ------------------------

def opcion_3
  a = [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]
  b = 3
  c = a.map { |x| x * b } rescue 0
  puts c.inspect
end

//-------------------------

def opcion_4
  a = [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]
  b = 3
  c = a.map { |x| x * b rescue 0 }
  puts c.inspect
end


	
```

Respuesta : 

Opción 1 :  Cuando x toma el valor de nil e intenta hacerse la operación Ruby lanza un error, al no haber un rescue dentro del map el error se propaga. Cuando es atrapado dentro del rescue devuelve 0 y no imprime nada. 

Opción 2 : El rescue esta dentro del begin ... rescue ... end, por lo que cuando se genera un error es capturado por el rescue y guardado en c. Luego hace el c.inspect imprimiendo 0. 

Opción 3 : Ruby lo que hace es evaluar el map completo, si el map completo falla, el rescue después del bloque toma el control. Terminando C con valor 0. 
Cuando uno de los elementos da error, todo el map falla y C termina con valor cero. 

Opción 4 :  Acá el rescue se encuentra dentro del map. Cuando x tome el valor de nil, el error de esa iteración se captura y se devuelve Cero solo para ese elemento. Las demás iteraciones siguen normalmente.  Como resultado ([3, 0, 9, 0, 15, 0, 21, 0, 27, 0])

> Cuando más localizado ubiquemos el rescue en los lugares donde pueda fallar, más localizado es el manejo de excepciones. 
> La opción 4 es la única que maneja las excepciones de manera granular. 
#### Ejercicio 7

Suponé que tenés el siguiente script y se te pide que lo hagas resiliente(tolerante a fallos), intentando siempre que se pueda recuperar la situación y volver a intentar la operación que falló. Realizá las modificaciones que consideres necesarias para lograr que este script sea más robusto.

```c
# Este script lee una secuencia de no menos de 15 números desde teclado y luego imprime
# el resultado de la división de cada número por su entero inmediato anterior.

cantidad = 0
while cantidad < 15
  puts 'Cuál es la cantidad de números que ingresará? Debe ser al menos 15'
  cantidad = Integer(gets)
end

# Luego se almacenan los números
numeros = 1.upto(cantidad).map do
  puts 'Ingrese un número'
  numero = Integer(gets)
end

# Finalmente se imprime cada número dividido por su número entero inmediato anterior
resultado = numeros.map { |x| x / (x - 1) }
puts 'El resultado es: %s' % resultado.join(', ')

```

Respuesta : 

```c

# Debemos pedir que ingrese una cantidad de numeros validos > 15 y que sea un numero

cantidad = 0
begin 
  puts 'Cuál es la cantidad de números que ingresará? Debe ser al menos 15'
  cantidad = Integer(gets)
  
  raise ArgumentError, 'Debe ser al menos 15 el valor' if cantidad < 15
  
rescue ArgumentError, TypeError => e 
	puts 'Lo ingresado es invalido : #{e.message}'
	retry
	
end

# Luego se almacenan los números verificando lo que se lee 

numeros = []
while numeros.size < cantidad
	puts 'Ingrese un número'
	numero = Integer(gets)
	
	numeros << numero
	
	raise ArgumentError, 'Lo ingresado no es un numero', TypeError 
		puts 'Lo ingresado es invalido'
		retry
	end
end


# Debemos tener en cuanta que no podemos dividir por Cero

begin
	resultado = numeros.map do |x|
		begin
			x / (x-1)
		rescue ZeroDivisionError
			puts 'No se puede dividir #{x} por (#{x}-1) por lo que se asigna 0'
			0
		end
	end
	
	puts 'El resultado es : #{resultado.join(', ')}'
rescue => e
	puts 'Ocurrio un error : #{e.message}'
end
```

#### Ejercicio 8

Partiendo del script que modificaste en el inciso anterior, implementé una nueva clase de excepción que se utilice para indicar que la entrada del usuario no es un valor numérico entero válido. ¿De qué clase de la jerarquía de Exception heredaría?

Respuesta : 

Debemos heredar de la clase `StandardError`, que es la clase base de la mayoría de las excepciones (no del sistema).
Esto los que nos permite es que se comporte como cualquier otra excepción en Ruby y pueda atraparse con rescue. 


### Librerías reutilizables en Ruby (Gemas) y Bundler

#### Ejercicio 9

¿Qué es una gema? ¿Para qué sirve? ¿Qué estructura general suele tener?

Respuesta : 

Una gema es es un paquete de código reutilizable en Ruby. 
Es la forma en la que el lenguaje redistribuye y comparte librerías, herramientas o componentes que pueden ser usados por los programadores. 

> Estas las podemos instalar y usar con `require`

Una gema tiene una estructura de directorios y archivos estándar. 
Por ejemplo una gema llamada `mi_gema` se vería así : 

```
mi_gema/
├── lib/
│   └── mi_gema.rb
├── mi_gema.gemspec
├── Gemfile
├── README.md
├── LICENSE.txt
└── Rakefile

```

| Archivo / Carpeta     | Función                                                                                  |
| --------------------- | ---------------------------------------------------------------------------------------- |
| **`lib/`**            | Contiene el código Ruby de la gema (el corazón del proyecto).                            |
| **`lib/mi_gema.rb`**  | Archivo principal que carga las demás partes de la gema.                                 |
| **`mi_gema.gemspec`** | Archivo que define la información de la gema: nombre, versión, autor, dependencias, etc. |
| **`Gemfile`**         | Lista las gemas necesarias para desarrollar o testear esta gema.                         |
| **`README.md`**       | Explica qué hace la gema y cómo usarla.                                                  |
| **`LICENSE.txt`**     | Licencia del proyecto.                                                                   |
| **`Rakefile`**        | Define tareas automáticas, como empaquetar o publicar la gema.                           |
#### Ejercicio 10

¿Cuáles son las principales diferencias entre el comando gem y Bundler? ¿Hacen lo mismo?

Respuesta : 

Ambos comandos están relacionados con la gestión de gemas pero no hacen lo mismo. 

*Gem* : 
Es la herramienta base del sistema Ruby para instalar, actualizar o eliminar gemas en nuestro entorno. 
Esto actúa a nivel global, sin relación directa con un proyecto especifico. 

Ej : 
```
gem install colorize
gem list
gem uninstall sinatra
```
> Las gemas instaladas con este comando quedan disponibles para cualquier proyecto de Ruby en nuestro sistema.


*Blunder* : 
Es una capa superior que administra las gemas y sus versiones especificas de un proyecto.
Usa un archivo llamado `GemFile` para definir gemas y versiones necesarias para el proyecto.

Cuando hacemos : `bundle install`
Lo que pasa es que bundler instala solo las gemas declaradas. Guarda versiones exactas en `GemFile.lock`. Asegurando que todos los que usen el mismo proyecto usen el mismo entorno. 

> Bundler garantiza **consistencia y aislamiento entre proyectos**.

> `gem` :  instala gemas **individualmente y globalmente**.
   `bundler` : **gestiona colecciones de gemas y sus versiones** dentro de un proyecto.

#### Ejercicio 11

¿Dónde almacenan las gemas que se instalan con el comando gem? ¿Y aquellas instaladas con el comando bundle?

Respuesta : 

Las gemas instaladas con gem se guardan en el directorio global de RubyGems del sistema del usuario. 
Podemos ver su ubicación con (gem which nombre_gema). 

Las gemas instaladas con bundle se guardan en el entorno del proyecto, por defecto en la carpeta del sistema o en el archivo  `vendor/bundle` si usamos : 
```c
bundle install --path vendor/bundle
```
Y podemos ver su ubicación con (bundle show nombre_gema)


#### Ejercicio 12

¿Para qué sirve el comando gem server? ¿Qué información podés obtener al usarlo?

Respuesta : 

El comando `gem server` lo que hace es iniciar un servidor web local `http://localhost:8808`
que nos permite navegar la documentación de las gemas instaladas en el sistema. 

Nos sirve para : 
- Ver las gemas instaladas 
- Acceder a la documentación de cada gema
- Consultar la versión, dependencias y descripción de cada una

#### Ejercicio 13

#### Ejercicio 14

#### Ejercicio 15









