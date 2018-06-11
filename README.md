# Angular

## Instalaciones.
Lo primero es instalar NodeJs para obtener el npm en la cmd de windows, una vez instalado se puede instalar TypeScript y Angular-Cli por npm.
>
* [NodeJs](https://nodejs.org/es/)
* [TypeScript](https://www.typescriptlang.org/)
* [Angular-Cli](https://cli.angular.io/)

Cuando ya este todo instalado se puede correr el servidor con:
```shell
ng serve
```

## Comandos utilizados:
```shell
ng new my-app #crea una aplicación con AngularCli
ng serve #monta el servidor
```
### Componentes
En un component tenemos el constructor y el método que se crea por defecto `ngOnInit()`.
El primero se lanza antes; el `ngOnInit()` cuando se renderiza la página
```shell
ng g c example #crea un componente example
ng g c components/example #crea un componente example dentro de la carpeta components que deberia estar creada antes de ejecutar este commando
```
Al momento de generar un componente se crean 4 archivos pero se elimina el de estilos(css) y el de test (spec).
Hay que eliminar dentro del componente el campo que hace referencia al css.

### Router
El archivo de rutas se debe llamar `app.routes.ts` y contiene el siguiente código:
```typescript=
//se importa el router
import { Routes, RouterModule } from '@angular/router';
//se importa un componente a utilizar
import { HomeComponent } from "./components/home/home.component";

/*
 En el path: se inidica el url que va a utilizar
 y en el component: el componente que utilizará ese path.
 En caso de que no se encuentre la ruta, se utiliza
 el path:'**' que va a redirigir al componente home.
 */
const APP_ROUTES: Routes = [
    { path: 'home', component: HomeComponent },
    { path: '**', pathMatch: 'full', redirectTo: 'home'}
];

export const APP_ROUTING = RouterModule.forRoot(APP_ROUTES);
```
Luego de crear este archivo se debe importar en el `app.module.ts`.


Para configurar las rutas de la aplicación deberemos crear una etiqueta `<router-outlet></router-outlet>` en el `app.component.html` donde queramos que se carguen las páginas que pidamos:
```html
<app-navbar></app-navbar>
<div class="container">
  <router-outlet></router-outlet>
</div>
```

### RouterLink y RouterLinkActive
El RouterLink reemplaza al href del html:
```html
<a class="nav-link" [routerLink]="['home']">Home</a>
```
El RouterLinkActive permite saber cual es la ruta activa, se coloca en un bloque padre de los links:
```htmlmixed
<li class="nav-item" routerLinkActive="active">
    <a class="nav-link" [routerLink]="['home']">Home</a>
</li>
```

### Extra de rutas.
Si se desea redirigir a una ruta iniciar se puede agregar un `/`, por ejemplo `[/home]`

#### Rutas con parametros.
```typescript=
{ path: 'heroe/:id', component: HeroeComponent}
```
Se importa en el componente que quiera utilizar este parametro:
```typescript=
import { ActivatedRoute } from "@angular/router";
constructor(private activatedRoute:ActivatedRoute){
this.activatedRoute.params.subscribe(params =>{
      console.log(params['id']);
    })  
}
```

### Servicios
Los servicios envian información a los componentes, para crear un servicio se debe generar un archivo del tipo `name.service.ts` en una carpeta `services`,este debe contener el siguiente código:
```typescript=
import { Injectable } from '@angular/core';

@Injectable()
export class NameService {
    constructor(){
        console.log("servicio listo");
    }
}
```
y se debe agregar al `app.module.ts`.
Para utilizar este servicio se debe importar en el componente y crear una variable privada en el constructor del tipo de servicio.

### Pipes
Se utilizan para cambiar la forma visual de los datos recibidos de algun servicio en el html.
Ejemplos:
```htmlmixed=
{{heroe.nombre | uppercase}}
({{heroe.aparicion | date:'y'}})
```
con el `:` se le envia un parámetro al pipe.