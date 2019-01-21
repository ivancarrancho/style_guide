# style-guide

Descripción de la guía de estilos en Senseta, además de la documentación de  pep8 para el cual usamos el linter de flake8
    https://www.python.org/dev/peps/pep-0008/


## Python - Flask

#### Variables

Cuando usemos variables que representen estados, valores, elementos en texto ejemplo:

> Bad example

```python
    operator_status = {
        'status': 'active',
    }

    operator_status = {
        'max_quantity': 18,
        'gender': 'Masculino',
    }

    operator_status = {
        'city': 'Bogotá DC',
    }
```

Se deben crear variables globales para que se maneje información homogénea en todo el proyecto
entonces debería verse así

```python
    ACTIVE_CHOICES =  'active'
    MAX_QUANTITY_CHOICES =  18
    GENDER_MALE_CHOICES =  'Masculino'
    CITY_BOGOTA_CHOICES =  'Bogotá DC'
```

Estas variables van alojadas en un data.py

> Good example

```python
    from app.XX import data

    operator_status = {
        'status': data.ACTIVE_CHOICES,
    }

    operator_status = {
        'max_quantity': data.MAX_QUANTITY_CHOICES,
        'gender': data.GENDER_MALE_CHOICES,
    }

    operator_status = {
        'city': data.CITY_BOGOTA_CHOICES,
    }
```

### Generalities
* Use comillas sencillas `''` para strings.

### URLS

Usar `/` slash al final de las URLs

> Bad example

```python
    view_path = 'events/_view/by_opened_by_operator'
```

> Good example

```python
    view_path = 'events/_view/by_opened_by_operator/'
```

### Importaciones

Usar una linea por cada dependencia, aún si son del mismo paquete.

> Bad example

```python
    from app.method import function_one, function_two, function_tree
```

> Good example

```python
    from app.method import function_one
    from app.method import function_two
    from app.method import function_tree
```

En el caso de ser demasiadas, debemos importar solamente el paquete base

> Good example

```python
    from app import method

    response = method.function_two()
    response = method.function_tree()
  ...
```

### Get context

Use asignación directa cuando pase una **sola** variable al contexto

> Bad example

```python
def get_context_data(self, **kwargs):
    ...

    context.update({
        'comment_form': CommentForm,
    })

    ...
```

> Good example

```python

def get_context_data(self, **kwargs):
    ...

    context['comment_form'] = CommentForm

    ...
```

Use `update` cuando pase multiples variables.

 > Bad example

 ```python

 def get_context_data(self, **kwargs):
     ...

     context['comment_form'] = CommentForm
     context['items_list'] = items_list
     context['users_list'] = users_list

     ...
 ```

 > Good example

 ```python
 def get_context_data(self, **kwargs):
     ...

     context.update({
         'comment_form': CommentForm,
         'items_list': items_list,
         'users_list': users_list,
     })

     ...
 ```


### Llamados a función

Se deben especificar los valores en el llamado de una función

```python
def function_name(attr1, attr2):
    return True
```

> Bad example

```python
function_name(626, "Modelo 50"):
    return True
```

> Good example

```python
function_name(attr1=626, attr2="Modelo 50"):
    return True
```

### Funciones después de 80 caracteres

Se debe partir la función en varias líneas

```python
def function_name(attr1, attr2):
    return True
```

> Bad examples

```python
# Has 93 characters
def function_name(attr_name_1=attr_data_1, attr_name_2=attr_data_2, attr_name_3=attr_data_3):
    return True
```

```python
def function_name(attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3
):
    return True
```

```python
def function_name(
    attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3):
    return True
```

> Good example

```python
def function_name(
    attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3
):
    return True
```


### Diccionarios/Json después de 80 caracteres

> Bad example

```python
# Has 93 lines
dict_1 = {'first_parameter_1': 'first_parameter_1', 'first_parameter_2': 'first_parameter_2'}
```

```python
dict_1 = {
    'first_parameter_1': 'first_parameter_1',
    'first_parameter_2': 'first_parameter_2'}
```

> Good example

```python
dict_1 = {
    'first_parameter_1': 'first_parameter_1',
    'first_parameter_2': 'first_parameter_2'
}
```

### String después de 80 caracteres

> Bad example

```python
# Has 93 lines
string_1 = 'Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, consectetur, ...'
```

> Good example

```python
string_1 = 'Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, '
    'consectetur, ...'
```

```python
string_1 = '''
    Neque porro quisquam est qui dolorem ipsum quia dolor si
    t amet, consecteturt amet, consecteturt amet, consecteturt amet,
    consectetu t amet, consectetur, ...
    '''
```

## Versionamiento

### Mensaje

Se sugiere usar los emojis de https://gitmoji.carloscuesta.me/

### Ammend

Si se realiza un commit con una parte de los cambios y se necesita adjuntar una segunda parte, es recomendable fusionar estos dos o sobrescribir el commit así:

*Aquí se agregan los primeros cambios*

    git add .
    git commit -m "primer commit"

*Aquí se realizan los cambios siguientes*

    git add .
    git commit --amend -m "Commit informando los nuevos cambios"

Si el commit ya fué enviado "*push*" se debe hacer nuevamente "*push -f*" para sobrescribir el commit viejo , de lo contrario se hace el push request o merge request normal.

##### Nota
Esto solamente sobrescribe el commit anterior, para fusionar más de 2 commits ver el **Rebase** a continuación

### Rebase

Si hay más de un commit:
[usa `git rebase` interactively](https://help.github.com/articles/about-git-rebase/)
y se quiere integrar en un solo commit más completo y bien explicado:

    git rebase -i <commit-id>~1

*<commit-id> es el commit hasta donde se quiere unificar.*
*Agrega el "~1" para incluir también el commit hasta donde se quiere unificar.*

    *Va a mostrar el listado de commits del mas antiguo al mas nuevo

    pick e162eb3 :soccer: Commit 3
    pick 1b9a12d :soccer: Commit 2
    pick 7b4d76e :soccer: Commit 1

*Y se debería cambiar así (en caso de querer unificarlos todos):*

    pick e162eb3 :soccer: Commit 3
    s 1b9a12d :soccer: Commit 2
    s 7b4d76e :soccer: Commit 1

*luego de unificar los commits, debes poner un solo mensaje:*

    # This is a combination of 3 commits.
    # This is the 1st commit message:

    :soccer: Commit 3

    # This is the commit message #2:

    :soccer: Commit 2

    # This is the commit message #3:

    :soccer: Commit 1

*Así:*

    # This is a combination of 3 commits.
    # This is the 1st commit message:

    :soccer: Commit 1 contains commit 2 and commit 3
