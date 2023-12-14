`v.1.0 - Diciembre 2023`

Ref: https://flask.palletsprojects.com/en/3.0.x/

Mínima Aplicación
```  
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

## 5 EJERCICIOS BÁSICOS
### Ejercicio 1: "Hola Mundo" con Flask
**Objetivo**: Crear una API simple que responda con "Hola Mundo" cuando se acceda a la raíz.

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hola Mundo'

if __name__ == '__main__':
    app.run(debug=True)

```

**Tarea**: Ejecuta este script en tu entorno local y visita `http://localhost:5000` en tu navegador para ver el mensaje "Hola Mundo".

### Ejercicio 2: "Endpoint de Saludo Personalizado"
**Objetivo**: Expandir la API para aceptar un nombre a través de la ruta y devolver un saludo personalizado.
```
from flask import Flask

app = Flask(__name__)

@app.route('/saludo/<nombre>')
def saludo_personalizado(nombre):
    return f'Hola, {nombre}!'

if __name__ == '__main__':
    app.run(debug=True)

```
**Tarea**: Accede `http://localhost:5000/saludo/<tu_nombre>` con tu propio nombre para recibir un saludo personalizado.

### Ejercicio 3: "Lista de Tareas Simple"
**Objetivo:** Crear una API que permita al usuario obtener una lista de tareas.
```
from flask import Flask, jsonify

app = Flask(__name__)

tareas = [
    {"id": 1, "titulo": "Comprar leche"},
    {"id": 2, "titulo": "Estudiar Flask"}
]

@app.route('/tareas', methods=['GET'])
def get_tareas():
    return jsonify({'tareas': tareas})

if __name__ == '__main__':
    app.run(debug=True)
```
**Tarea:** Usa `http://localhost:5000/tareas` para obtener la lista de tareas en formato JSON.

### Ejercicio 4: "Agregar Tarea"
**Objetivo:** Permitir al usuario agregar una nueva tarea a la lista a través de un endpoint POST.
```
from flask import Flask, jsonify, request

app = Flask(__name__)

tareas = [
    {"id": 1, "titulo": "Comprar leche"},
    {"id": 2, "titulo": "Estudiar Flask"}
]

@app.route('/tareas', methods=['GET'])
def get_tareas():
    return jsonify({'tareas': tareas})

@app.route('/tareas', methods=['POST'])
def add_tarea():
    if not request.json or not 'titulo' in request.json:
        abort(400)
    tarea = {
        'id': tareas[-1]['id'] + 1,
        'titulo': request.json['titulo']
    }
    tareas.append(tarea)
    return jsonify({'tarea': tarea}), 201

if __name__ == '__main__':
    app.run(debug=True)
```
**Tarea:** Utiliza una herramienta como `curl` o Postman para enviar una solicitud POST a `http://localhost:5000/tareas` con un cuerpo JSON que incluya el título de la nueva tarea.

Estos ejercicios cubren conceptos básicos de Flask y API REST, desde una ruta simple hasta la manipulación de datos con GET y POST. Son adecuados para principiantes y proporcionan una base sólida para construir aplicaciones más complejas.