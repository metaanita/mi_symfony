# mi_symfony
crud en symfony con login y register ++

Requisitos
Symfony CLI: https://symfony.com/download, PHP 8.3.0: https://www.php.net/manual/en/install.php, Composer: https://getcomposer.org/download/, Symfony 6.4: https://symfony.com/releases/6.4
Instalación de Symfony y paquetes
symfony new symfony-6-login --version=6.4
cd back
composer require symfony/maker-bundle --dev (Comandos para construir)
composer require symfony/orm-pack (ORM para pegar la base de datos)
composer require symfony/profiler-pack --dev (Profiler para tener información)
composer require form (Para los formularios)
composer require validator (Validaciones)
composer require twig-bundle (Para plantillas)
composer require security-csrf
composer require api
composer require symfony/security-bundle
composer require lexik/jwt-authentication-bundle (Para seguridad. Si falla en Windows en el php.ini permitir la extension sodium. También puede ser necesaria la extensión composer requiere ext-openssl)
Pasos para el CRUD de users
La base de datos será un sqlite en el propio repo. Modifico el .env para trabajar con sqlite https://www.sqlite.org/index.html 
Crear la entidad user: php bin/console make:user 
Actualizo la Base de datos: php bin/console doctrine:schema:update --force
Crear formulario de registro: php bin/console make:registration-form (Sin validación de emails, te pide una url para ir una vez registrado, todavía no está, no la tenemos pues marco 0 y más tarde la cambiaré). 
Ya tengo el register: Ejecuto symfony server:start y compruebo https://127.0.0.1:8000/register 
Falla al registrar por la ruta, nos dice donde... Creamos un controlador para la home: php bin/console make:controller, lo llamammos home y añado el nombre de la ruta en el controlador: app_home
Creamos el CRUD de User: php bin/console make:crud de User (Sin test por ahora...). 
El new del controlador hago que redireccione al register. return $this->redirectToRoute('app_register'); 
Pongo bonito el CRUD:
En el head añado boostrap <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
En el body añado un div <div class="container mt-2 p-3 pt-5 p-md-5"> 
Hago un login: php bin/console make:auth con AppCustomAuthenticator y con ruta de log out. Genera un SecurityController. Me pide a dónde le llevo, en la Linea 53: return new RedirectResponse($this->urlGenerator->generate('app_home'));. 
En el profiler ya estamos logados. 
No queremos que todos el mundo pueda acceder a la tabla de user solo los admin: En el security.yaml, descomento y modifico: - { path: ^/user, roles:ROLE_ADMIN } y ya tenemos algo de seguridad.
Intento entrar un /user y me da información del error. Podemos cambiar el rol por base de datos para comprobar que funciona. (Aplicar y guardar cambios)

Seguiremos ampliando el CRUD, metiendo test etc
Gracias!
