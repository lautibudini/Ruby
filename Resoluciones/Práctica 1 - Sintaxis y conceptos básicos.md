
> **irb** es el interprete interactivo de Ruby, se abre en la terminal escribiendo "irb".

> En Ruby un símbolo es una etiqueta inmutable y única que sirve para identificar algo.
> Se escribe con ":" delante como :ejemplo.

> Las funciones y variables tienen algunos caracteres para representar cosas 
> ? - Significa que es una consulta

> Que es un modulo y una def(funcion) las dif.


### Qué es un **Mixin**

Un **mixin** es un **módulo que aporta funcionalidad a una clase** sin necesidad de heredar de otra clase.

- Ruby permite **incluir un módulo en una clase** usando `include` o `extend`.
    
- Los métodos del módulo se **mezclan** en la clase, como si fueran parte de ella.
    
- Esto se llama **“composición de comportamiento”** en vez de herencia.
#### Ejercicio 1

Investiga y proba en un intérprete de Ruby (irb, por ejemplo) cómo crear objetos de los siguientes tipos básicos, tanto mediante el uso de literales como utilizando el constructor new (cuando sea posible): 

• Symbol 
• String 
• Array 
• Hash

*Respuesta* : 

En Ruby tenemos dos formas de crear objetos: 
1. Con literales (Escribiendo directamente el valor)
2. Con **new** (Cuando la clase lo permite)

 Symbol  : 

```c

// Con literal 

:mi_objeto

// Con constructor (No existe el new, así que podes convertir un string a un simbolo)

"mi_objeto".to_sym

```

String : 

```c

// Con literal

"Hola"

// Con constructor

String.new("hola")

```


Array : 

```c

// Con literal

[1,2,3]

// Con constructor

Array.new
Array.new(2)
Array.new(3,2)

// Array.new(dimF, inicialización)

```

Hash : 

```c

// Con literal

{nombre: "lauti", edad: 21}

// Con constructor

Hash.new
h = Hash.new(0) //crea hash con valor por defecto 0
h["x"]          // => 0

```

#### Ejercicio 2

¿De qué forma(s) se puede convertir un objeto (cualquiera fuere su tipo o clase) en String?

**Respuesta :** 

En Ruby hay más de una forma de transformar cualquier objeto en un String, algunas de ellas son : 

```c
// Con el metodo .to_s

[1,2,3].to_s 
:hola.to_s

```

```c
// Con interpolación de Strings

edad = 21
"Mi edad es #{edad}"

```

```c
// Con String(parametro)

String("hola")
String(:ejemplo)

```

#### Ejercicio 3

¿De qué forma(s) se puede convertir un objeto String en un símbolo?

Respuesta : 

Algunas de las formas para convertir Strings a símbolos son : 

```c
// Con .to_sym

"hola".to_sym  // -- > :hola
```

```c
// Con intern (es un alias de .to_sym)

"hola".intern  // -- > :hola

```

#### Ejercicio 4

¿Qué devuelve la siguiente comparación? ¿Por qué?

```c

'TTPS Ruby'.object_id == 'TTPS Ruby'.object_id

```

**Respuesta** : 

Esto nos devuelve False (tienen distintos object id ) Ya que cada vez que escribimos un nuevo String entre comillas Ruby crea nuevos objetos en memoria. 
En cambio si los transformamos a símbolos nos devuelve el mismo object id. 

#### Ejercicio 5

Escribí una función llamada reemplazar que, dado un String que recibe como parámetro, busque y reemplace en éste cualquier ocurrencia de "{" por "do\n" y cualquier ocurrencia de "}" por "\nend", de modo que convierta los bloques escritos con llaves por bloques multilínea con do y end. Por ejemplo:

```c

reemplazar("3.times { |i| puts i }") 
# => "3.times do\n |i| puts i \nend"

```

Respuesta : 

```c

// g.sub(Caracter a cambiar, Caracter por el que se reemplazara)

def reemplazar(str)
	str.gsub("{", "do\n").gsub("}", "\nend")
end

puts reemplazar("3.times { |i| puts i }")

```

#### Ejercicio 6 - Terminar

Escribí una función que exprese en palabras la hora que recibe como parámetro, según las siguientes reglas:

• Si el minuto está entre 0 y 10, debe decir “en punto”, 
• si el minuto está entre 11 y 20, debe decir “y cuarto”, 
• si el minuto está entre 21 y 34, debe decir “y media”, 
• si el minuto está entre 35 y 44, debe decir “menos veinticinco” con la hora siguiente, 
• si el minuto está entre 45 y 55, debe decir “menos cuarto” con la hora siguiente, 
• y si el minuto está entre 56 y 59, debe decir “Casi son las” con la hora siguiente

Tomé como ejemplos los siguientes casos:

```c

tiempo_en_palabras(Time.new(2025, 10, 21, 10, 1))  # => "Son las 10 en punto"  tiempo_en_palabras(Time.new(2025, 10, 21, 9, 33))  # => "Son las 9 y media" tiempo_en_palabras(Time.new(2025, 10, 21, 8, 45))  # => "Son las 9 menos cuarto" tiempo_en_palabras(Time.new(2025, 10, 21, 6, 58))  # => "Casi son las 7" tiempo_en_palabras(Time.new(2025, 10, 21, 0, 58))  # => "Casi es las 1"

```

Es importante considerar que cuando la hora es 1, la forma correcta de expresarla no es “Son las 1 en punto”, sino “Es la 1 en punto”. Esto debe tenerse en cuenta en cada uno de los casos expresados en el enunciado de este ejercicio.

> Tip: resolver utilizando rangos numéricos

Respuesta : 

```c

def tiempo_con_palabras(time)
	hora = time.hour
	minutos = time.min
	
	# Debemos chequear si se trata de 1
	
	if hora == 1 
		respuesta = "Es la 1"
	else
		respuesta = "Son las #{hora}"
	end
	
	# Ahora los rangos para poder concatenar lo que sigue
	case minutos
		when 0..10
			respuesta2 = "en punto"
		when 11..20 
			respuesta2 = "y cuarto"
		when 21..34
			respuesta2 = "y media"
		when 35..44
			respuesta2 = "menos veinticinco"
		when 45..55
			respuesta2 = "menos cuarto"
		when 56..59
			respuesta2 = "casi son las"
			
	
	[respuesta, respuesta2]
	

```



#### Ejercicio 7

Escribí una función llamada contar que reciba como parámetro dos String y que retorne la cantidad de veces que aparece el segundo String en el primero, en una búsqueda caseinsensitive (sin distinguir mayúsculas o minúsculas). Por ejemplo:

```c
contar("La casa de la esquina tiene la puerta roja y la ventana blanca.", "la")
# => 5
```


**Respuesta** : 

```c

// Al String le hacemos downcase
// cadena1.scan(cadena2) devuelve un arreglo con las coincidencias
// .length devuelve el tamaño del arreglo

def contar(cadena1, cadena2)
	puts cadena1.downcase.scan(cadena2.downcase).length
end
contar("La casa de la esquina tiene la puerta roja y la ventana blanca.", "la")

```


#### Ejercicio 8

Modifica la función anterior para que sólo considere como aparición del segundo String cuando se trate de palabras completas. Por ejemplo : 

```c

contar_palabras("La casa de la esquina tiene la puerta roja y la ventana blanca.", "la") 
2 # => 4
```

**Respuesta** : 

```c 

// .split - lo que hace es seperar las palabras que hay a partir de cada espacio.
// .count - cuenta cuántas palabras coinciden exactamente con la palabra buscada.

def contar_palabras(cadena1, cadena2)
	puts cadena1.downcase.split.count(cadena2.downcase)
end

contar_palabras("La casa de la esquina tiene la puerta roja y la ventana blanca.", "la")
```



