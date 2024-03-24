# calisthenics.py 

import sqlite3

# Función para crear la tabla de ejercicios si no existe
def crear_tabla_ejercicios():
    conexion = sqlite3.connect('calistenia.db')
    cursor = conexion.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS ejercicios (
                        id INTEGER PRIMARY KEY,
                        nombre TEXT NOT NULL,
                        descripcion TEXT
                    )''')
    conexion.commit()
    conexion.close()

# Función para crear la tabla de entrenamientos si no existe
def crear_tabla_entrenamientos():
    conexion = sqlite3.connect('calistenia.db')
    cursor = conexion.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS entrenamientos (
                        id INTEGER PRIMARY KEY,
                        id_ejercicio INTEGER,
                        repeticiones INTEGER,
                        sets INTEGER,
                        FOREIGN KEY (id_ejercicio) REFERENCES ejercicios(id)
                    )''')
    conexion.commit()
    conexion.close()

# Función para agregar un nuevo ejercicio
def agregar_ejercicio(nombre, descripcion=''):
    conexion = sqlite3.connect('calistenia.db')
    cursor = conexion.cursor()
    cursor.execute('''INSERT INTO ejercicios (nombre, descripcion) VALUES (?, ?)''', (nombre, descripcion))
    conexion.commit()
    conexion.close()

# Función para obtener todos los ejercicios disponibles
def obtener_ejercicios():
    conexion = sqlite3.connect('calistenia.db')
    cursor = conexion.cursor()
    cursor.execute('''SELECT * FROM ejercicios''')
    ejercicios = cursor.fetchall()
    conexion.close()
    return ejercicios

# Función para registrar un entrenamiento
def registrar_entrenamiento(id_ejercicio, repeticiones, sets):
    conexion = sqlite3.connect('calistenia.db')
    cursor = conexion.cursor()
    cursor.execute('''INSERT INTO entrenamientos (id_ejercicio, repeticiones, sets) VALUES (?, ?, ?)''', (id_ejercicio, repeticiones, sets))
    conexion.commit()
    conexion.close()

# Crear las tablas si no existen
crear_tabla_ejercicios()
crear_tabla_entrenamientos()

# Agregar ejercicios de ejemplo
agregar_ejercicio("Flexiones", "Ejercicio para trabajar el pecho")
agregar_ejercicio("Sentadillas", "Ejercicio para trabajar las piernas")
agregar_ejercicio("Dominadas", "Ejercicio para trabajar la espalda")
agregar_ejercicio("Fondos", "Ejercicio para trabajar triceps")



# Obtener ejercicios disponibles
print("Ejercicios disponibles:")
ejercicios = obtener_ejercicios()
for ejercicio in ejercicios:
    print(f"{ejercicio[0]}. {ejercicio[1]}")

# Solicitar al usuario que seleccione un ejercicio
id_ejercicio = int(input("Seleccione el ejercicio (ingrese el ID): "))

# Solicitar al usuario el número de repeticiones y sets
repeticiones = int(input("Ingrese el número de repeticiones: "))
sets = int(input("Ingrese el número de sets: "))

# Registrar el entrenamiento
registrar_entrenamiento(id_ejercicio, repeticiones, sets)

print("Entrenamiento registrado correctamente.")


# Manipulación de datos: podemos agregar nuevos ejercicios o registros de entrenamientos utilizando instrucciones INSERT INTO.
# Integridad referencial: use claves foráneas en las tablas para establecer relaciones entre ellas. Esto asegura la integridad referencial de los datos, lo que significa que no podemos tener registros de entrenamientos que hagan referencia a ejercicios que no existen en la tabla de ejercicios.
# Optimización de consultas: Podría mencionar que SQL nos proporciona herramientas para optimizar consultas y mejorar el rendimiento de nuestras operaciones de base de datos. Por ejemplo, podríamos indexar columnas relevantes en nuestras tablas para acelerar las búsquedas.
# Seguridad de datos: SQL nos permite establecer permisos de acceso a las tablas y controlar quién puede realizar operaciones específicas en la base de datos. Esto ayuda a garantizar la seguridad y la privacidad de los datos.
# Mejoras posibles: Se podrian incluir los tiempos de descanso, la cantidad de peso o si el usuario se sintio fatigado o no despues de cada ejercicio para posteriormente generar el entrenamiento mas optimo 
