# Symfony2: A Simple Hello World

Simple application structure of symfony 2

~~~
+--- app
    +--- AppKernel.php
    +--- config/routing.yml
    +--- Resources/views/hello.html.twig
+--- src
    +--- Acme/TrainingBundle/Controller/HelloController.php
+--- web
    +--- index.php
~~~

## Controller

~~~ php
<?php

// src/Acme/TrainingBundle/Controller/HelloController.php

namespace Acme\TrainingBundle\Controller;

use Symfony\Component\HttpFoundation\Response;

class HelloController {

    public function helloAction() {
        return new Response('Hello World!');
    }

    public function helloAction() {
        // app/Resources/views/hello.html.twig
        return $this->render('hello.html.twig');
    }
}
~~~

## Web Facade

~~~ php
<?php

// web/index.php

require_once __DIR__ . "/../vendor/autoload.php";
require_once __DIR__ . "/../app/AppKernel.php";

use Symfony\Component\HttpFoundation\Request;

$request = Request::createFromGlobals();
$kernel = new AppKernel('dev', true);
$kernel->handle($request)->send();
~~~

## Application Kernel

~~~ php
<?php

// app/AppKernel.php

use Symfony\Component\HttpKernel\Kernel;
use Symfony\Component\Config\Loader\LoaderInterface;

class AppKernel extends Kernel {

    public function registerBundles() {
        return array(
            new Symfony\Bundle\FrameworkBundle\FrameworkBundle(),
            new Symfony\Bundle\TwigBundle\TwigBundle(),
        );
    }

    public function registerContainerConfiguration(LoaderInterface $loader) {
        $loader->load(function ($container) {
            $container->loadFromExtension('framework', array(
                'secret' => 'some secret here',
                'router' => array( 'resource' => '%kernel.root_dir%/config/routing.yml', ),
                'templating' => array( 'engines' => array( 'twig', ), ),
            ));
        });
    }

    public function getCacheDir() {
        return 'path to cache directory';
    }

    public function getLogDir() {
        return 'path to log directory';
    }
}
~~~

## Route

~~~ yaml
# app/config/routing.yml

hello:
    pattern: /
    defaults:
        _controller: "Acme\TrainingBundle\Controller\HelloController::helloAction"
~~~
