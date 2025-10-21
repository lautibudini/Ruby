

> [!NOTE] Nota
> En esta segunda práctica del taller aplicaremos lo visto sobre el lenguaje Ruby, analizando distintas situaciones con los elementos fundamentales del mismo: las clases, los módulos, los bloques, los enumeradores, y las excepciones


### Métodos

1 ) Implementa un método que reciba como parámetro un arreglo de números, los ordene y devuelva el resultado. Por ejemplo:

```c
 ordenar_arreglo([1, 4, 6, 2, 3, 0, 10]) 
 # => [0, 1, 2, 3, 4, 6, 10]
```

**Respuesta** : 

```c
def ordenar_arreglo(arreglo)
	arreglo.sort
end

arreglo = [1, 4, 6, 2, 3, 0, 10]
nuevoArreglo = ordenar_arreglo(arreglo) // No se modifica el arreglo original

puts arreglo.inspect // Original
puts nuevoArreglo.inspect //Nuevo

// .inspect lo imprime como un arreglo
```

2 ) Modifica el método anterior para que en lugar de recibir un arreglo como único parámetro, reciba todos los números como parámetros separados. Por ejemplo:

```c
ordenar(1, 4, 6, 2, 3, 5, 0, 10, 9) 
# => [0, 1, 2, 3, 4, 5, 6, 9, 10]
```

**Respuesta** : 

```c
def ordenar(*numeros)
	numeros.sort
	## Podria hacerse Array(numeros).sort
end

resultado = ordenar(1, 4, 6, 2, 3, 5, 0, 10, 9) //No hace falta guardarlo
puts resultado.inspect

```
> Ya lo que retorna la función es un arreglo 

3 ) Suponé que se te da el método que implementaste en el ejercicio 2 para que lo uses a fin de ordenar un conjunto de números que te son provistos en forma de arreglo. ¿Cómo podrías invocar el método? Por ejemplo, teniendo la siguiente variable con los números a ordenar:

```c
entrada = [10, 9, 1, 2, 3, 5, 7, 8] 2 
# Dada `entrada', invocá a #ordenar utilizando los valores que contiene la variable 3 
ordenar(entrada) 
# Esto no funciona. Corregí esta invocación para que funcione.

```

**Respuesta**  : 

> Lo que debemos hacer es recibir el arreglo y luego otros números como parámetros, los combinamos y retornamos ordenados.
```c
def ordenar(arreglo, *numeros)
	# Los combinamos
	res = arreglo + numeros
	# Retornamos ordenados
	res.sort
end

entrada = [10, 9, 1, 2, 3, 5, 7, 8]  
ordenar(entrada,2)
```

4 ) Escribí un método que dado un número variable de parámetros que pueden ser de cualquier tipo, imprima en pantalla la cantidad de caracteres que tiene su representación como String y la representación que se utilizó para contarla.

```c
longitud(9, Time.now, 'Hola', {un: 'hash'}, :ruby)  
# Debe imprimir:  
# "9" --> 1  
# "2025-09-14 13:22:10 +0000" --> 25  
# "Hola" --> 4  
# {:un=>"hash"} --> 13  
# ruby --> 4

// Nota: Para convertir cada parámetro a string utilizá el método #to_s presente en todos los objetos
```

**Respuesta**  : 

```c
def longitud(*args)
	args.each do |parametro| // |param| es una variable de bloque
		puts "Usa : #{parametro.to_s.size}"
  end
end

  longitud(9, Time.now, 'Hola', {un: 'hash'}, :ruby) 
```

5 ) Implementá el método cuanto_falta? que opcionalmente reciba como parámetro un objeto Time y que calcule la cantidad de minutos que faltan para ese momento. Si el parámetro de fecha no es provisto, asumí que la consulta es para la medianoche de hoy.

```c
cuanto_falta? Time.new(2032, 12, 31, 23, 59, 59) 
# => Retorna la cantidad de minutos que faltan para las 23:59:59 del 31/12/2032 
cuanto_falta?
# => Retorna la cantidad de minutos que faltan para la medianoche de hoy
```

**Respuesta** : 

```c
def cuanto_falta?(momento = nil) 
	ahora = Time.now
	
	# Si no se paso ningun parametro usamos la media noche de hoy
	aux = momento || Time.new(ahora.year, ahora.month, ahora.day + 1, 0, 0, 0)
	
	# Calculamos la diferencia en segundos y lo pasamos a minutos
	
	((aux - ahora) / 60).to_i
end

cuanto_falta? Time.new(2032, 12, 31, 23, 59, 59) 
cuanto_falta?
```
>Con `momento = nil` hacemos que el parámetro sea opcional

### Clases y módulos - 6, t10, 

6 ) Modelá con una jerarquía de clases la siguiente situación: Los usuarios finales de una aplicación tienen los atributos básicos que permiten identificarlos (usuario, clave, email - los que consideres necesarios), y un rol que determina qué operaciones pueden hacer. Los roles posibles son: Lector, Redactor, Director y Administrador. Cada usuario gestiona Documentos según su rol le permita, acorde a las siguientes reglas:

- Los Lectores pueden ver cualquier Documento que esté marcado como público. 
- Los Redactores pueden hacer todo lo que los Lectores y además pueden cambiar el contenido de los Documentos que ellos crearon. 
- Los Directores pueden ver y cambiar el contenido de cualquier documento (público o privado, y creado por cualquier usuario), excepto aquellos que hayan sido borrados. 
- Los Administradores pueden hacer lo mismo que los directores y además pueden borrar Documentos.

Utilizando el siguiente código para la clase Documento, Implementá las clases que consideres necesarias para representar a los usuarios y sus roles, completando la funcionalidad aquí presente:

```c
class Documento 
	attr_accessor :creador, :contenido, :publico, :borrado 
	
	def initialize(usuario, publico = true, contenido = '') 
		self.creador = usuario 
		self.publico = publico 
		self.contenido = contenido 
		self.borrado = false 
	end
	
	def borrar 
		self.borrado = true 
	end 
	
	def puede_ser_visto_por?(usuario) 
		usuario.puede_ver? self 
	end 
	
	def puede_ser_modificado_por?(usuario) 
		usuario.puede_modificar? self 
	end 
	
	def puede_ser_borrado_por?(usuario) 
		usuario.puede_borrar? self 
	end 
end
```

**Respuesta** : 

```c
class Usuario
	attr_acessor : :usuario, :clave, :mail
	
	def initialize(usuario,clave,mail)
		@usuario = usuario
		@clave = clave
		@mail = mail
	end
end

# -----------------------------------------------

class Lector < Usuario
	def puede_ver?(documento)
		documento.publico && !documento.borrado
	end
	
	def puede_modificar?(documento)
		false
	end
	
	def peude_borrar?(documento)
		false
	end
end

# -----------------------------------------------

class Redactor < Lector

	def puede_modificar?(documento)
		!documento.borrado && documento.creador == self
	end
	
end

# -----------------------------------------------

class Director < Usuario
	
	def puede_ver?(documento)
		!documento.borrado
	end
	
	def puede_modificar?(documento)
		!documento.borrado
	end
	
	def peude_borrar?(documento)
		false
	end
end

# -----------------------------------------------

class Administradores < Director

	def peude_borrar?(documento)
		true
	end
end
```

> La duda que tengo es si en la clase persona va self o @ en las variables de la clase.

7 ) Luego de implementar el ejercicio anterior, modificalo para que los usuarios implementen el método #to_s que debe retornar el atributo usuario (o email, según hayas decidido utilizar) y el rol que posee. Por ejemplo:

```c
lector.to_s 
# => "elhector@example.org (Lector)" 
administrador.to_s 
# => "admin@example.org (Administrador)"
```

**Respuesta**  : 

```c
// Se agrega este bloque de codigo en la clase Usuario

  def to_s
	# Método to_s que muestra usuario/email y rol
    "#{email} (#{self.class})"
  end
  
// self.class nos devuelve la clase "nombre" del objeto
```

8 ) ¿Qué diferencia hay entre el uso de include y extend a la hora de incorporar un módulo en una clase?
1. Si quisieras usar un módulo para agregar métodos de instancia a una clase, ¿Qué forma usarías a la hora de incorporar el módulo a la clase? 
2. Si en cambio quisieras usar un módulo para agregar métodos de clase, ¿Qué forma usarías en ese caso?
   
**Respuesta** : 

Include : 
- Añade los métodos del modulo como métodos de instancia de la clase.
- Eso significa que solo los objetos instanciados de la clase pueden usar esos métodos.
```c
module Saludos
  def saludar
    "Hola!"
  end
end

class Persona
  include Saludos
end

p = Persona.new
puts p.saludar  # => "Hola!"
# Persona.saludar -> ERROR, no existe como método de clase
```

Extend : 
- Añade los métodos del módulo como métodos de la clase.
- Eso significa que la clase misma puede usar esos métodos, no las instancias.
```c
module Saludos
  def saludar
    "Hola desde la clase!"
  end
end

class Persona
  extend Saludos
end

puts Persona.saludar  # => "Hola desde la clase!"
# Persona.new.saludar -> ERROR, no existe como método de instancia
```

1. Si se desea agregar módulos para agregar métodos de instancia de una clase usaría include.
2. Si se desea usar un modulo para agregar métodos de clase se usaría extend.

|Palabra clave|Tipo de método agregado|Quién lo puede usar|
|---|---|---|
|`include`|instancia|objetos/instancias|
|`extend`|clase|la clase misma|

9 ) Implementá el módulo Reverso para utilizar como Mixin e incluirlo en alguna clase para probarlo. Reverso debe contener los siguientes métodos:

1. #di_tcejbo: Imprime el object_id del receptor en espejo (en orden inverso). 
2. #ssalc: Imprime el nombre de la clase del receptor en espejo.

Respuesta : 

```c
module Reverso

# Imprime el object_id en orden inverso
  def di_tcejbo
    puts object_id.to_s.reverse
  end

# Imprime el nombre de la clase en orden inverso
  def ssalc
    puts self.class.to_s.reverse
  end
end   

# -------------------------------------------------------

class Persona
  include Reverso
end

p = Persona.new
p.di_tcejbo   
p.ssalc
```


10 )  Implementá el Mixin Countable que te permita hacer que cualquier clase cuente la cantidad de veces que los métodos de instancia definidos en ella es invocado. Utilízalo en distintas clases, tanto desarrolladas por vos como clases de la librería standard de Ruby, y chequea los resultados. El Mixin debe tener los siguientes métodos:

1. count_invocations_of(sym): método de clase que al invocarse realiza las tareas necesarias para contabilizar las invocaciones al método de instancia cuyo nombre es sym (un símbolo).
2. invoked?(sym): método de instancia que devuelve un valor booleano indicando si el método llamado sym fue invocado al menos una vez en la instancia receptora.
3. invoked(sym): método de instancia que devuelve la cantidad de veces que el método identificado por sym fue invocado en la instancia receptora.

Por ejemplo, su uso podría ser el siguiente:

```c
class Greeter 
	include Countable # Incluyo el Mixin
	 
	def hi 
		puts 'Hey!' 
	end 
	
	def bye 
		puts 'See you!' 
	end 
	# Indico que quiero llevar la cuenta de veces que se invoca el método #hi 
	
	count_invocations_of :hi 
end 

 a = Greeter.new 
 b = Greeter.new 
 a.invoked? :hi 
 # => false 
 b.invoked? :hi 
 # => false 
 a.hi 
 # Imprime "Hey!" 
 a.invoked :hi 
 # => 1 
 b.invoked :hi 
 # => 0
```
>Nota: para simplificar el ejercicio, asumí que los métodos a contabilizar no reciben parámetros. Tip: investigá Module#alias_method y Module#included.

**Respuesta** : 

```c

```

#### Ejercicio 11

Dada la siguiente clase abstracta GenericFactory, Implementá subclases de la misma que permitan la creación de instancias de dichas clases mediante el uso del método de clase Create, de manera tal que luego puedas usar esa lógica para instanciar objetos sin invocar directamente el constructor new.

```c
class GenericFactory 
	def self.create(**args) 
		new(**args) 
	end 

	def initialize(**args) 
		raiseNotImplementedError 
	end 
end
```

**Respuesta** : 

> Los que nos plantea este ejercicio es que las subclases de GenericFactory deban implementar el .create para no realizar el new como forma de crear el objeto, sino que lo realiza internamente el mismo. 

> Esto simula el Factory Method Pattern pero aprovechando la herencia en Ruby.

```c
class GenericFactory
  def self.create(**args)
    new(**args)
  end

  def initialize(**args)
    raise NotImplementedError, "Implementá initialize en la subclase"
  end
end

#--------------------------------------------

class Persona < GenericFactory
  attr_accessor :nombre

  def initialize(nombre:)
    @nombre = nombre
  end

  def to_s
    "Persona: #{@nombre}"
  end
end

#--------------------------------------------

p1 = Persona.create(nombre: "Lauti")
puts p1.to_s   
```

#### Ejercicio 12 

Modifica la implementación del ejercicio anterior para que GenericFactory sea un módulo que se incluya como Mixin en las subclases que implementaste. ¿Qué modificaciones tuviste que hacer en tus clases?

**Respuesta** : 

```c

module GenericFactory

	def self.included(base)
	    base.extend(ClassMethods)
	end
	
	module ClassMethods
	    def create(**args)
		    new(**args)
	    end
	end
end


#--------------------------------------------

class Persona 

    include GenericFactory
	attr_accessor :nombre

	  def initialize(nombre:)
	    @nombre = nombre
	  end
	  
	
	  def to_s
	    "Persona: #{@nombre}"
	  end
	end

#--------------------------------------------

p1 = Persona.create(nombre: "Lauti")
puts p1.to_s 
```

Cambios : 
1. GenericFactory paso de ser una clase a ser un módulo.
2. Como los módulos no permiten la herencia, lo que se hace es **incluirlo** (Include) en las clase Persona (En las que lo usemos).
3. Para que el método de clase create este disponible en las clases que lo incluyen, se debe usar `self.included(base)` + `base.extend(ClassMethods)`. Así `create` se convierte en un método de clase en Persona.
4. En las clases concretas ya no se hereda, simplemente se incluye como mixin.


#### Ejercicio 13

Extendé las clases TrueClass y FalseClass para que ambas respondan al método de instancia opposite, el cual en cada caso debe retornar el valor opuesto al que recibe la invocación al método. Por ejemplo:

```c
false.opposite 
# =>true 
true.opposite 
# =>false 
true.opposite.opposite 
# =>true
```

**Respuesta** : 

> En este caso no hace falta crear clases nuevas ya que podemos extenderlas y definir el método que nos esta pidiendo sin conocer los demás métodos que contiene.

```c
class TrueClass
  def opposite
    false
  end
end

class FalseClass
  def opposite
    true
  end
end

puts false.opposite   # => true
puts true.opposite    # => false
puts true.opposite.opposite  # => true
```

#### Ejercicio 14

Analizá el siguiente  script e indica :

```c
VALUE='global' 

module A 
	VALUE='A' 
	
	classB 
		VALUE='B' 
		
		def self.value 
			VALUE 
		end 

		def value 
			'iB' 
		end 
	end 

	def self.value 
		VALUE 
	end 
end

class C 
	class D 
		VALUE = 'D' 

		def self.value 
			VALUE 
		end 
	end 

	module E 
		def self.value 
			VALUE 
		end 
	end 

	def self.value 
		VALUE 
	end 
end 

class F < C 
	VALUE = 'F' 
end
```

¿Qué imprimen cada una de las siguientes sentencias? ¿De dónde está obteniendo el valor? 
1. puts A.value 
2. puts A::B.value 
3. puts C::D.value 
4. puts C::E.value 
5. puts F.value

Respuesta : 
1. Imprime A, ya que el self.value devuelve value que es A.
2. Imprime B, ya que la clase B también tiene self.value el cual retorna value que es B.
3. Imprime D, lo mismo que los anteriores.
4. Imprime Global, ya que E devuelve value pero no hay ninguna definida, sube a la clase que lo contiene que es C, pasa lo mismo por lo que Ruby sube al ámbito superior (Top-level) donde existe VALUE y retorna 'global'.
5. Imprime Global, La clase F hereda de C y por más que defina un valor en VALUE, no define el método por lo que va a C y hace el mismo recorrido que se menciona en el punto (4).

6.¿Qué pasaría si ejecutases las siguientes sentencias? ¿Por qué? 
1. puts A::value 
2. puts A.new.value 
3. puts B.value 
4. puts C::D.value 
5. puts C.value
6. puts F.superclass.value

Respuesta : 

1.   `NameError: uninitialized constant A::value` Ya que no hay una constante declarada en minúscula con ese nombre.  
2. `NoMethodError: undefined method 'new' for A:Module` Ya que esta queriendo acceder con el punto a un método NEW el cual no existe y además los módulos no pueden instanciar.
3. `NameError: uninitialized constant B` Ya que esta definida en A::B.
4. Imprime D.
5. Imprime Global.
6. Imprime Global.


### Bloques

#### Ejercicio 15 

Escribí un métododa_nil? que reciba un bloque, lo invoque y retorne si el valor de retorno del bloque fue nil. Por ejemplo:

```c
da_nil? { } 
# => true 
da_nil? do 
	'Algo distinto de nil' 
end 
# => false
```

Respuesta : 

> La idea es que el método `da_nil?` reciba un bloque (`&block`), lo ejecute, y cheque si el resultado es `nil`.

```c
def da_nil?(&block)
  #Invoco al bloque con `block.call`
  resultado = block.call
  resultado.nil?
end
```

#### Ejercicio 16

