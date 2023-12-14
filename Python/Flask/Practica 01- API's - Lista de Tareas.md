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
[Incluye código de ejemplo]
 
### Borrar todas las tareas finalizadas.
[Incluye código de ejemplo]

### Cambiar una tarea de Completa a incompleta.
[Incluye código de ejemplo] 

---

## Conclusiones
[Resumen de lo que has aprendido de esta práctica y cualquier dificultad o desafío que hayas enfrentado.]

---
## Referencias (si es aplicable)
[Lista de referencias si la hay]

---