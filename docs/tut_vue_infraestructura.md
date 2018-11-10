# Infraestructura
## Instalamos una versión de node adecuada
Podemos usar nvm para instalar diferentes versiones de nvm.

Ejecutamos primero:

```
source /usr/share/nvm/init-nvm.sh
```

Instalamos la versión v10.3:

```
nvm install 10.3
```

E indicamos que la use:

```
nvm use 10.3
```

> Instalamos la version v10.3 porque con ella podemos instalar **grpc** (necesario para poder instalar **firebase**)


## Prerrequisitos
Instalamos:

```bash
$ yaourt -S vue-cli
$ yaourt -S ngrok
```
> Otra forma es `npm i -g @vue/cli`, que instala vue 3.


## Creamos el proyecto
Hacemos:

```bash
vue create agenda
```
y elegimos el default (babel, eslint).

```bash
cd agenda
vue add router
vue add vuex
vue add vuetify
npm run serve
```

en donde:

- router: instala *vue-router* que controla las vistas a mostrar en cada caso.
- vuex: es un store centralizado para el estado de la aplicación.
- vuetify: es el toolkit de visualización (basado en material design).

### Opcional - iconos
Podemos instalar los iconos localmente. Podemos elegir entre diferentes sets de iconos:

```bash
npm i -S vue-material-design-icons
```

## PWA
Instalamos *pwa*:

```bash
vue add pwa
```
