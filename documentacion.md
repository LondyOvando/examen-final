 # autenticacion de Auth0.2
 se describira como se realiza la configuración para poder utilizar los servicios de autho dentro de nuestras aplicaciones

 # Obtencion de sus claves de aplicación
 luego de haberse registrado en Autho, se creó una nueva aplicación, o se crea una nueva. Necesitará algunos detalles sobre esa aplicación para comunicarse con Auth0.

 ![pantalla 1](https://auth0.com/docs/media/articles/dashboard/client_settings.png)

 # informacion necesaria

Dominio
Identificación del cliente

# Configurar direcciones URL de devolución de llamada
Una URL de devolución de llamada es una URL en su aplicación donde Auth0 redirige al usuario después de que se haya autenticado. La URL de devolución de llamada para su aplicación debe agregarse al campo URL de devolución de llamada permitidas.

# Configurar URL de cierre de sesión.
Una URL de cierre de sesión es una URL en su aplicación a la que Auth0 puede regresar después de que el usuario haya cerrado la sesión del servidor de autorización. Esto se especifica en el returnToparámetro de consulta.

# Configurar orígenes web permitidos.
Debe agregar la URL de su aplicación al campo Orígenes web permitido.

npm install @auth0/auth0-angular

El SDK expone varios tipos que lo ayudan a integrar Auth0 con su aplicación Angular idiomáticamente, incluido un módulo y un servicio de autenticación.

# Registrar y configurar el módulo de autenticación

El SDK exporta AuthModule, un módulo que contiene todos los servicios necesarios para el funcionamiento del SDK. Para registrar esto con su aplicación:

abre el app.module.tsarchivo
Importar el AuthModuletipo del @auth0/auth0-angularpaquete
Agregue AuthModulea la aplicación llamando AuthModule.forRooty agregando a la importsmatriz de su módulo de aplicación.

ejemplo.
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

// Import the module from the SDK
import { AuthModule } from '@auth0/auth0-angular';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,

    // Import the module into the application, with configuration
    AuthModule.forRoot({
      domain: 'YOUR_DOMAIN',
      clientId: 'YOUR_CLIENT_ID'
    }),
  ],

  bootstrap: [AppComponent],
})
export class AppModule {}

# Agregar inicio de sesión a su aplicación
El SDK de Auth0 Angular le brinda herramientas para implementar rápidamente la autenticación de usuario en su aplicación Angular, como crear un
acceso
botón usando el loginWithRedirect()método de la AuthServiceclase de servicio. Ejecutar loginWithRedirect()redirige a sus usuarios a la página de inicio de sesión universal de Auth0, donde Auth0 puede autenticarlos.

import { Component } from '@angular/core';

// Import the AuthService type from the SDK
import { AuthService } from '@auth0/auth0-angular';

@Component({
  selector: 'app-auth-button',
  template: '<button (click)="auth.loginWithRedirect()">Log in</button>'
})
export class AuthButtonComponent {
  // Inject the authentication service into your component through the constructor
  constructor(public auth: AuthService) {}
}

 ![pantalla 2](https://auth0.com/docs/media/quickstarts/universal-login.png)

 # cierre de sesión.
 se Puede crear un botón de cierre de sesión utilizando el logout()método del AuthServiceservicio. Ejecutar logout()redirige a sus usuarios a su
Extremo de cierre de sesión de Auth0
( https://YOUR_DOMAIN/v2/logout) y luego los redirige inmediatamente a su aplicación.

ejemplo:
import { Component, Inject } from '@angular/core';
import { AuthService } from '@auth0/auth0-angular';
import { DOCUMENT } from '@angular/common';

@Component({
  selector: 'app-auth-button',
  template: `
    <ng-container *ngIf="auth.isAuthenticated$ | async; else loggedOut">
      <button (click)="auth.logout({ returnTo: document.location.origin })">
        Log out
      </button>
    </ng-container>

    <ng-template #loggedOut>
      <button (click)="auth.loginWithRedirect()">Log in</button>
    </ng-template>
  `,
  styles: [],
})
export class AuthButtonComponent {
  constructor(@Inject(DOCUMENT) public document: Document, public auth: AuthService) {}
}

