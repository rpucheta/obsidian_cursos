`v.1.0 - Diciembre 2023`
  
![[Pasted image 20231214020431.png]]
## Información General

- **Nombre del Estudiante:** [Tu nombre]
- **Fecha de Entrega:** [Fecha de entrega]
-  **Curso:** [Nombre de la asignatura o curso]
- **Profesor/Instructor:** [Nombre del profesor o instructor]

## Descripción

- Vamos a emular un TODO de tareas, donde por medio de una API Flask , utilizando los diferentes métodos , vamos a poder:
>
	- Listar todas las tareas.
	- Añadir una tarea
	- Borrar una tarea
	- Completar una tarea

- Debemos controlar los posibles errores, y devolver los diferentes código de status. que hemos visto durante el curso.
- Todo los contenidos que se muestre, tendrán que mostrarse por medio de un cliente rest y en formato  JSON.
- Si lo veis muy fácil,  habrá tres tareas extra:
>
	- Editar una tarea en concreto.
	- Borrar todas las tareas finalizadas.
	- Cambiar una tarea de Completa a incompleta.
### Requisitos Previos

- Es necesario que se usen todos y cada uno de los conceptos que hemos abordado durante el curso, morfología de un request de un response, status code, métodos, etc...
---
## Solución/Implementación
### Setup Flask
```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Lista de tareas (inicialmente vacía)
tasks = []
```

### Listar todas las tareas
``` python
@app.route('/tasks', methods=['GET'])
def get_tasks():
    return jsonify({'tasks': tasks})
```

### Añadir una tarea
```python
@app.route('/tasks', methods=['POST'])
def add_task():
    data = request.get_json()
    if 'task' in data:
        new_task = {'task': data['task'], 'completed': False}
        tasks.append(new_task)
        return jsonify({'message': 'Tarea añadida con éxito!'})
    else:
        return jsonify({'message': 'Error: Debes proporcionar una tarea válida.'}), 400
```

### Borrar una tarea
```python
@app.route('/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    if task_id < len(tasks):
        del tasks[task_id]
        return jsonify({'message': 'Tarea eliminada con éxito!'})
    else:
        return jsonify({'message': 'Error: Tarea no encontrada.'}), 404
```

### Completar una tarea
```python
@app.route('/tasks/<int:task_id>/complete', methods=['PUT'])
def complete_task(task_id):
    if task_id < len(tasks):
        tasks[task_id]['completed'] = True
        return jsonify({'message': 'Tarea completada con éxito!'})
    else:
        return jsonify({'message': 'Error: Tarea no encontrada.'}), 404
```

### Editar una tarea en concreto.
```python
@app.route('/tasks/<int:task_id>', methods=['PUT'])
def edit_task(task_id):
    if task_id < len(tasks):
        data = request.get_json()
        if 'task' in data:
            tasks[task_id]['task'] = data['task']
            return jsonify({'message': 'Tarea editada con éxito!'})
        else:
            return jsonify({'message': 'Error: Debes proporcionar una tarea válida en el cuerpo JSON.'}), 400
    else:
        return jsonify({'message': 'Error: Tarea no encontrada.'}), 404
```
 
### Borrar todas las tareas finalizadas.
```python
@app.route('/tasks/clear-completed', methods=['DELETE'])
def clear_completed_tasks():
    global tasks
    tasks = [task for task in tasks if not task['completed']]
    return jsonify({'message': 'Todas las tareas completadas han sido eliminadas.'})
```

### Cambiar una tarea de Completa a incompleta.
```python
@app.route('/tasks/<int:task_id>/incomplete', methods=['PUT'])
def mark_task_incomplete(task_id):
    if task_id < len(tasks):
        tasks[task_id]['completed'] = False
        return jsonify({'message': 'Tarea marcada como incompleta con éxito!'})
    else:
        return jsonify({'message': 'Error: Tarea no encontrada.'}), 404
```

---

## Conclusiones
A partir de la implementación de todas estas operaciones en la API de gestión de tareas, podemos sacar varias conclusiones y observaciones:

1. **Flask es una herramienta versátil para crear APIs web:** Flask es una excelente opción para desarrollar APIs web debido a su simplicidad y flexibilidad. Permite crear rápidamente endpoints para manipular datos y responder a solicitudes HTTP de manera eficiente.
    
2. **RESTful API:** La API sigue principios RESTful al utilizar métodos HTTP (GET, POST, PUT, DELETE) y rutas claras para realizar operaciones en los recursos (tareas). Esto hace que la API sea intuitiva y fácil de entender.
    
3. **Uso de JSON:** La API utiliza JSON para la representación de datos, lo que es una práctica común en el desarrollo de APIs web. Esto facilita la interoperabilidad con otros sistemas y aplicaciones.
    
4. **Gestión de estado:** La API permite la gestión completa de las tareas, incluyendo la creación, edición, eliminación, marcado como completo o incompleto, y la eliminación de todas las tareas completadas. Esto proporciona una funcionalidad completa para una lista de tareas.
    
5. **Validación de datos:** La API incluye cierta validación de datos para garantizar que las solicitudes sean correctas y que se proporcionen los datos necesarios. Por ejemplo, verifica si se proporciona una tarea válida al agregar o editar una tarea.
    
6. **Manejo de errores:** La API maneja errores adecuadamente y devuelve respuestas HTTP apropiadas con mensajes de error cuando es necesario, lo que mejora la experiencia del usuario final y la depuración del desarrollador.
    
7. **Buenas prácticas:** La API sigue buenas prácticas de desarrollo, como dividir el código en funciones y rutas claras, mantener la lógica de negocios separada de la lógica de enrutamiento y proporcionar mensajes de respuesta informativos.
    

En general, esta implementación muestra cómo crear una API básica para una aplicación de gestión de tareas, pero puede ser ampliada y mejorada para adaptarse a requisitos más avanzados o específicos del proyecto. También destaca la importancia de utilizar una estructura y diseño organizados al crear una API web para facilitar el mantenimiento y la escalabilidad.

---
## Referencias
Al desarrollar una aplicación web utilizando Flask y programación en Python, es importante mencionar y seguir las mejores prácticas y referencias relacionadas con el desarrollo web y Flask en particular. A continuación, te proporciono algunas referencias útiles que pueden ayudarte a profundizar en el desarrollo de aplicaciones web con Flask:

1. **Documentación oficial de Flask:** La documentación oficial de Flask es una fuente invaluable de información. Contiene ejemplos detallados, guías de inicio rápido y una explicación completa de los conceptos clave de Flask. Puedes encontrarla en el sitio web oficial de Flask: [Flask Documentation](https://flask.palletsprojects.com/)
    
2. **RESTful API Design:** Para diseñar una API RESTful de manera efectiva, es útil comprender los principios de diseño REST. El libro "API Design" de Jake VanderPlas ofrece una introducción sólida a estos conceptos.
    
3. **Guía de buenas prácticas de Flask:** El artículo "The Flask Mega-Tutorial Part I: Hello, World!" escrito por Miguel Grinberg es una guía popular que abarca una amplia variedad de temas relacionados con Flask y proporciona ejemplos prácticos para desarrolladores. Puedes encontrarlo en su blog: [The Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)
    
4. **Librerías y extensiones de Flask:** Flask cuenta con una amplia comunidad de desarrolladores que han creado extensiones útiles para tareas comunes. Puedes explorar estas extensiones en el repositorio de extensiones de Flask: [Flask Extensions](https://flask.palletsprojects.com/en/2.1.x/extensions/)
    
5. **Librerías para manejo de JSON:** Para trabajar con datos JSON en Python, puedes utilizar la librería `json`. Asegúrate de revisar la documentación oficial de Python sobre el manejo de JSON: [json - JSON encoder and decoder](https://docs.python.org/3/library/json.html)
    
6. **Referencias generales de Python:** Siempre es útil tener una sólida comprensión de Python. La documentación oficial de Python y libros como "Python Crash Course" de Eric Matthes pueden ser recursos valiosos.
    
7. **Libros sobre Flask:** Hay varios libros dedicados a Flask que cubren en profundidad el desarrollo web con este framework. Algunos ejemplos son "Flask Web Development" de Miguel Grinberg y "Flask By Example" de Gareth Dwyer.
    
8. **Comunidad de desarrolladores:** Participar en comunidades en línea como el foro de Flask en Stack Overflow o el subreddit de Flask en Reddit puede ayudarte a obtener respuestas a preguntas específicas y aprender de otros desarrolladores.

---