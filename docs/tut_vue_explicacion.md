# Vistas
## Disposición
### Layout
La pantalla principal tiene:

- una *toolbar* en la parte superior.
- el contenido irá justo debajo

El comportamiento depende del tamaño de la ventana (reactive design):

- Si la pantalla es grande en la parte superior derecha se accederá a distintas vistas.
- Si la pantalla es pequeña, aparece el botón de menú arriba a la izquierda. Nos da acceso a la sidebar que contendrá un menú (equivalentes en función a los botones de la parte superior derecha).

### Contenido
Para el contenido hay varias vistas:

- Landing: es la página de entrada.
- Sign Up: para entrar con un nuevo usuario
- Sign In: para hacer el login.
- Home: página en la que se entra cuando se ha hecho el login.
- NotFound: cuando se busca acceder a una página inexistente.

Cuando se pulsan los botones de la parte superior o a los del menú se llega a estas diferentes vistas. Esto se consigue mediante el *routing*.

## Comportamiento
