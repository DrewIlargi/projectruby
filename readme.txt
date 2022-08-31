1. Agregamos las gemas en nuestro archivo Gemfile:

# Instalamos las dos gemas necesarias y luego vamos a 
assets/stylesheets/application.css y cambiamos el tipo 
por .scss

# Bootstrap 5
gem "bootstrap"

# Use sassc to process CSS
gem "sassc-rails"
# Tiramos el bundle install 

2. Estando en el scss ya modificado agregamos:

@import "bootstrap"

/* Importamos la libreria bootstrap y luego lanzamos el comando 
 bin/importmap pin bootstrap --download 
 luego nos redirigimos a config/importmap.rb */

3. Estando en el importmap eliminamos los archivos generados y anclamos

pin "popper", to: 'popper.js', preload: true
pin "bootstrap", to: 'bootstrap.min.js', preload: true 

# Recursos que usaremos, ahora para utilizarlos vamos a app/javascript/controllers/application.js

4. Agregamos nuevos componentes

import "popper"
import "bootstrap"

// Para hacer que se compilen se redirige a config/initializers/assets.rb y agregamos:

Rails.application.config.assets.precompile += %w( bootstrap.min.js popper.js )

Ahora vamos y probamos usando las clases, para diferentes usos hay que utilzar 
 los js de los diferente componentes a usar.

Ejemplo 1:

Agregar un navbar

1. Buscamos el codigo en Bootstrap 5 de un navbar

	<nav class="navbar navbar-expand-lg navbar-light bg-light">
		<div class="container-fluid">
			<a class="navbar-brand" href="#">Navbar</a>
			<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
			<div class="collapse navbar-collapse" id="navbarSupportedContent">
				<ul class="navbar-nav me-auto mb-2 mb-lg-0">
					<li class="nav-item">
						<a class="nav-link active" aria-current="page" href="#">Home</a>
					</li>
					<li class="nav-item">
						<a class="nav-link" href="#">Link</a>
					</li>
					<li class="nav-item dropdown">
						<a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
							Dropdown
						</a>
						<ul class="dropdown-menu" aria-labelledby="navbarDropdown">
							<li><a class="dropdown-item" href="#">Action</a></li>
							<li><a class="dropdown-item" href="#">Another action</a></li>
							<li><hr class="dropdown-divider"></li>
							<li><a class="dropdown-item" href="#">Something else here</a></li>
						</ul>
					</li>
					<li class="nav-item">
						<a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
					</li>
				</ul>
				<form class="d-flex">
					<input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
					<button class="btn btn-outline-success" type="submit">Search</button>
				</form>
			</div>
		</div>
	</nav>

2. Creamos un archivo en nuestra views/layouts/_navbar.html.erb y pegamos nuestro codigo

3. Nos dirigimos a views/layouts/application.html.erb y agregamos en nuestro body la llamada al layouts

	<%= render "layouts/navbar" %>

Ejemplo 2:

Agregar popover

En nuestro index añadimos:

	<button type="button" class="btn btn-lg btn-danger" data-bs-toggle="popover" title="Popover title" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button>

Para darle vida o utilidad agregamos en app/javascript/application.js

	// Popover
	var popoverTriggerList = [].slice.call(document.querySelectorAll('[data-bs-toggle="popover"]'))
	var popoverList = popoverTriggerList.map(function (popoverTriggerEl) {
		return new bootstrap.Popover(popoverTriggerEl)
	})

Ejemplo 3:

Para agregar un Toast Notify podemos hacer la misma del ejemplo 2 o esta, en nuestro index agregamos el code

	<button type="button" data-controller="toast" data-action="click->toast#notify" class="btn btn-primary" id="liveToastBtn">Show live toast</button>

	<div class="position-fixed bottom-0 end-0 p-3" style="z-index: 11">
		<div id="liveToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
			<div class="toast-header">
				<strong class="me-auto">Bootstrap</strong>
				<small>11 mins ago</small>
				<button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
			</div>
			<div class="toast-body">
				Hello, world! This is a toast message.
			</div>
		</div>
	</div>

Generamos un controlador con 

	rails g stimulus toast 

Nos dirigimos al archivo generado app/javascript/controllers/toast_controller.js

	Dentro de la export agregamos un metodo notify()

notify() {
	// Push notifications
	var toastLiveExample = document.getElementById('liveToast')
	var toast = new bootstrap.Toast(toastLiveExample)
	toast.show()
}

Esto lo encontramos en la documentacion del componente que queremos usar

En el index, en el boton agregamos dos propiedades: 

	data-controller="toast"

	data-action="click->toast/notify"
