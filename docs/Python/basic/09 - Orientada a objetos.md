# Programación orientada a objetos

La Programación Orientada a Objetos (POO) - en inglés "Object-Oriented Programming (OOP)" - es un paradigma de programación que utiliza "objetos" y sus interacciones para diseñar aplicaciones y programas. Python es un lenguaje que soporta POO, lo que permite crear programas más estructurados y reutilizables. Esto permite crear programas más organizados y fáciles de mantener.

## Clases y Objetos

- **Clase**: Es un plano o plantilla para crear objetos. Define un conjunto de atributos y métodos que los objetos creados a partir de la clase tendrán.
- **Objeto**: Es una instancia de una clase. Es la entidad real que se crea utilizando la clase como plantilla.

```python
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

    def saludar(self):
        print(f"Hola, mi nombre es {self.nombre} y tengo {self.edad} años.")

# Crear un objeto de la clase Persona
persona1 = Persona("Juan", 30)
persona1.saludar()
```

## Herencia

La herencia permite crear una nueva clase que es una modificación de una clase existente. La nueva clase hereda los atributos y métodos de la clase base.

```python
class Empleado(Persona):
    def __init__(self, nombre, edad, salario):
        super().__init__(nombre, edad)
        self.salario = salario

    def mostrar_salario(self):
        print(f"Mi salario es {self.salario}.")

# Crear un objeto de la clase Empleado
empleado1 = Empleado("Ana", 28, 50000)
empleado1.saludar()
empleado1.mostrar_salario()
```

## Encapsulamiento

El encapsulamiento es el mecanismo que restringe el acceso directo a algunos de los componentes de un objeto. En Python, se puede usar un guion bajo (_) para indicar que un atributo o método es privado.

```python
class CuentaBancaria:
    def __init__(self, titular, saldo):
        self.titular = titular
        self.__saldo = saldo

    def mostrar_saldo(self):
        print(f"El saldo de {self.titular} es {self.__saldo}.")

    def depositar(self, cantidad):
        self.__saldo += cantidad

# Crear un objeto de la clase CuentaBancaria
cuenta1 = CuentaBancaria("Carlos", 1000)
cuenta1.mostrar_saldo()
cuenta1.depositar(500)
cuenta1.mostrar_saldo()
```

## Polimorfismo

El polimorfismo permite que diferentes clases puedan ser tratadas como instancias de una misma clase a través de una interfaz común. Esto es útil para funciones que pueden recibir objetos de diferentes clases y tratarlos de manera uniforme.

```python
class Animal:
    def hacer_sonido(self):
        pass

class Perro(Animal):
    def hacer_sonido(self):
        print("Guau")

class Gato(Animal):
    def hacer_sonido(self):
        print("Miau")

def hacer_sonido_animal(animal):
    animal.hacer_sonido()

# Crear objetos de las clases Perro y Gato
perro = Perro()
gato = Gato()

hacer_sonido_animal(perro)
hacer_sonido_animal(gato)
```
