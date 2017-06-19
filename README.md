aqua_note
=========

A Symfony project created on June 17, 2017, 4:46 pm.

# TODO
## Step 1 : Joyful Development with Symfony 3
- [x] 1. [Start Project](http://knpuniversity.com/screencast/symfony/start-project)
- [x] 2. Setup PhpStorm + git
- [x] 3. First Page
- [x] 4. [Routing Wildcards](http://knpuniversity.com/screencast/symfony/routing-wildcards#play)
- [x] 5. [Intro to services](http://knpuniversity.com/screencast/symfony/services-useful-objects#play)
- [x] 6. [Listening and Using Services](http://knpuniversity.com/screencast/symfony/listing-services#play)
- [x] 7. [Twig: For a Good time with Templates](http://knpuniversity.com/screencast/symfony/hello-twig#play)
- [x] 8. [Twig Layouts (Template Inharitance)](http://knpuniversity.com/screencast/symfony/twig-layouts#play)
- [x] 9. [Loading CSS & JS Assets](http://knpuniversity.com/screencast/symfony/layout-assets#the-asset-function)
- [X] 10. [JSON Responses + Route Generation](http://knpuniversity.com/screencast/symfony/json-api#play)
- [x] 11. Generating URLs
- [ ] 12. ReactJS talks to your API

# DOCUMENTATIONS:
## Step 1 :
### The Symfony Installer
```
$ sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
$ sudo chmod a+x /usr/local/bin/symfony
```

### Create New Project
`$ symfony new aqua_note && cd aqua_note`

### Start Local Server
`$ php bin/console server:run`

## Step 2 : 
### Setup! PhpStorm + git
```
$ git init
$ git add .
$ git status
$ git commit 
```

## Step 3 :
### First Page
##### src/AppBundle/Controller/GenusController.php
```
<?php

namespace AppBundle\Controller;

use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

use Symfony\Component\HttpFoundation\Response;

class GenusController
{
    /**
     * @Route("/genus")
     */
    public function showAction()
    {
        return new Response('Under the sea!');
    }
}
```

## Step 4 : 
### Routing Wildcards
#### How? Change the URL to /genus/{genusName}:
##### src/AppBundle/Controller/GenusController.php
```
<?php

namespace AppBundle\Controller;

use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Component\HttpFoundation\Response;

class GenusController
{
    /**
     * @Route("/genus/{genusName}")
     */
    public function showAction($genusName)
    {
        return new Response('The genus: '.$genusName);
    }
}
```

#### ! Tip
> Be careful when rendering direct user input (like we are here)! It introduces a security issue called XSS - [read more about XSS here.](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)

#### Troubleshooting
##### Show routes
`$ bin/console debug:router | grep /genus/`
##### Clear cache
`bin/console cache:clear -e prod`

## Step 5 :
### Intro to Services
#### [Service Container](http://knpuniversity.com/screencast/symfony/services-useful-objects#service-container)
#### [Accessing the Container](http://knpuniversity.com/screencast/symfony/services-useful-objects#accessing-the-container)
#### @ Go Deeper !
> Why does extending Controller give you access to the container? Find out: [Injecting the Container: ContainerAwareInterface](http://knpuniversity.com/screencast/symfony-journey/determine-the-controller#injecting-the-container-containerawareinterface) (advanced).
```
<?php

namespace AppBundle\Controller;

use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;

class GenusController extends Controller
{
    /**
     * @Route("/genus/{genusName}")
     */
    public function showAction($genusName)
    {
        $templating = $this->container->get('templating');
        $html = $templating->render('genus/show.html.twig', [
                'name' => $genusName
        ]);

        return new Response($html);
    }
}
```

#### @ Go Deeper !
You can actually return anything from a controller via the kernel.view event: [The kernel.view Event](http://knpuniversity.com/screencast/symfony-journey/kernel.view-event) (advanced)

#### [Create the Template](http://knpuniversity.com/screencast/symfony/services-useful-objects#create-the-template)
##### app/Resources/views/genus/show.html.twig
`<h1>The Genus {{ name }}</h1>`

## Step 6 :
### Listing and Using Services
#### Replace all of this code with a simple return $this->render:
##### src/AppBundle/Controller/GenusController.php

```
... lines 1 - 8
class GenusController extends Controller
{
... lines 11 - 13
    public function showAction($genusName)
    {
        return $this->render('genus/show.html.twig', array(
            'name' => $genusName
        ));
    }
}
```

#### So what does this magic-looking render() function actually do? Let's find out! 
##### vendor/symfony/symfony/src/Symfony/Bundle/FrameworkBundle/Controller/Controller.php
```
... lines 1 - 38
abstract class Controller implements ContainerAwareInterface
{
... lines 41 - 185
    protected function render($view, array $parameters = array(), Response $response = null)
    {
        if ($this->container->has('templating')) {
            return $this->container->get('templating')->renderResponse($view, $parameters, $response);
        }
... lines 191 - 202
    }
... lines 204 - 396
}
```

#### [What Services are there?](http://knpuniversity.com/screencast/symfony/listing-services#what-services-are-there)
`$ php bin/console`

##### Check out that debug:container command - run that!
`$ php bin/console debug:container`

To search for a specific service

##### We could Google that, or we could pass an argument to this command to search for services matching "log" 
`$ php bin/console debug:container log`


## Step 7 :
### Twig: For a Good time with Templates
#### [{{ SaySomething }}, {% doSomething %}](http://knpuniversity.com/screencast/symfony/hello-twig#saysomething-dosomething)
##### Head over to Twig's website at [twig.sensiolabs.org](http://twig.sensiolabs.org/), click Documentation and scroll down. Ah, this is a list of everything Twig does.

#### [The dump() Function ](http://knpuniversity.com/screencast/symfony/hello-twig#the-dump-function)
##### src/AppBundle/Controller/GenusController.php
```
<?php

namespace AppBundle\Controller;

use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;

class GenusController extends Controller
{
    /**
     * @Route("/genus/{genusName}")
     */
    public function showAction($genusName)
    {
    	$notes = [
            'Octopus asked me a riddle, outsmarted me',
            'I counted 8 legs... as they wrapped around me',
            'Inked!'
        ];

        return $this->render('genus/show.html.twig', [
                'name' => $genusName,
                'notes' => $notes
        ]);
    }
}
```

##### But before we loop over this, I want to show you a small piece of awesome: the dump() function:
##### app/Resources/views/genus/show.html.twig
`{{ dump() }}`

This is like *var_dump()* in PHP, but better, and you can use it without any arguments to print details about every available variable.

#### @ Go Deeper !
>See more usage examples of the dump() function in [The dump Function for Debugging](https://knpuniversity.com/screencast/twig/functions-filters#the-dump-function-for-debugging) of the separate Twig screencast. chapter.

#### [The for Tag ](http://knpuniversity.com/screencast/symfony/hello-twig#the-for-tag)
##### app/Resources/views/genus/show.html.twig
```
<h1>The Genus {{ name }}</h1>

<ul>
    {% for note in notes %}
        <li>{{ note }}</li>
    {% endfor %}
</ul>
{# {{ dump() }} #}
```

## Step 8 :
### Twig Layouts (Template Inheritance)
#### To get a layout
##### app/Resources/views/genus/show.html.twig
```
{% extends 'base.html.twig' %}
<h1>The Genus {{ name }}</h1>
<ul>
    {% for note in notes %}
        <li>{{ note }}</li>
    {% endfor %}
</ul>
```

##### app/Resources/views/base.html.twig
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <title>{% block title %}Welcome!{% endblock %}</title>
        {% block stylesheets %}{% endblock %}
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
    </head>
    <body>
        {% block body %}{% endblock %}
        {% block javascripts %}{% endblock %}
    </body>
</html>
```

### [The Web Debug Toolbar and Profiler](http://knpuniversity.com/screencast/symfony/twig-layouts#the-web-debug-toolbar-and-profiler)
#### [Overriding a Second Block](http://knpuniversity.com/screencast/symfony/twig-layouts#overriding-a-second-block)

#### @ Go Deeper !
>If you want more, we have a whole screencast on just Twig templating engine: [Twig Templating for Friendly Frontend Devs](http://knpuniversity.com/screencast/twig).

## Step : 9
### Loading CSS & JS Assets
#### [Copying web files](http://knpuniversity.com/screencast/symfony/layout-assets#copying-web-files)
```
cp -r tutorial/web/* web/
web/css
web/images
web/js
web/vendor
```

#### @ Go Deeper !
> One way to get fancy is by using Gulp to process, minify and combine assets. See [Gulp! Refreshment for Your Frontend Assets](https://knpuniversity.com/screencast/gulp).


#### [Including Static Assets](http://knpuniversity.com/screencast/symfony/layout-assets#including-static-assets)
##### app/Resources/views/base.html.twig
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>{% block title %}AquaNote!{% endblock %}</title>
        {% block stylesheets %}
            <link rel="stylesheet" href="{{ asset('vendor/bootstrap/css/bootstrap.min.css') }}">
            <link rel="stylesheet" href="{{ asset('css/styles.css') }}">
            <link rel="stylesheet" href="{{ asset('vendor/fontawesome/css/font-awesome.min.css') }}">
        {% endblock %}
        <link rel="icon" type="image/x-icon" href="{{ asset('favicon.ico') }}" />
    </head>
    <body>
        <div class="search-bar">
            <form method="GET" action="" class="js-sea-search sea-search">
                <input type="search" name="q" placeholder="Search Sea Creatures" autocomplete="off" class="search-input">
            </form>
        </div>
        <header class="header">
            <img class="logo-icon" src="{{ asset('images/aquanote-logo.png') }}">
            <h1 class="logo">AquaNote</h1>
            <ul class="navi">
                <li class="search"><a href="#" class="js-header-search-toggle"><i class="fa fa-search"></i></a></li>
                <li><a href="#">Login</a></li>
            </ul>
        </header>
        {% block body %}{% endblock %}
        <div class="footer">
            <p class="footer-text">Made with <span class="heart"><3</span> <a href="https://knpuniversity.com">KnpUniversity</a></p>
        </div>
        {% block javascripts %}
            <script src="//code.jquery.com/jquery-2.1.4.min.js"></script>
            <script src="{{ asset('js/main.js') }}"></script>
        {% endblock %}
    </body>
</html>
```

#### @ Go Deeper !
> How can you add page-specific CSS or JS files? See [ReactJS talks to your API](https://knpuniversity.com/screencast/symfony/reactjs-api).

#### [Updating show.html.twig](http://knpuniversity.com/screencast/symfony/layout-assets#updating-code-show-html-twig-code)
##### app/Resources/views/genus/show.html.twig
```
{% extends 'base.html.twig' %}
{% block title %}Genus {{ name }}{% endblock %}
{% block body %}
    <h2 class="genus-name">{{ name }}</h2>
    <div class="sea-creature-container">
        <div class="genus-photo"></div>
        <div class="genus-details">
            <dl class="genus-details-list">
                <dt>Subfamily:</dt>
                <dd>Octopodinae</dd>
                <dt>Known Species:</dt>
                <dd>289</dd>
                <dt>Fun Fact:</dt>
                <dd>Octopuses can change the color of their body in just three-tenths of a second!</dd>
            </dl>
        </div>
    </div>
    <div class="notes-container">
        <h2 class="notes-header">Notes</h2>
        <div><i class="fa fa-plus plus-btn"></i></div>
    </div>
    <section id="cd-timeline">
        {% for note in notes %}
            <div class="cd-timeline-block">
                <div class="cd-timeline-img">
                    <img src="{{ asset('images/leanna.jpeg') }}" class="img-circle" alt="Leanna!">
                </div>
                <div class="cd-timeline-content">
                    <h2><a href="#">AquaPelham</a></h2>
                    <p>{{ note }}</p>
                    <span class="cd-date">Dec. 10, 2015</span>
                </div>
            </div>
        {% endfor %}
    </section>
{% endblock %}
```
## Step 10 :
### [JSON Responses + Route Generation](http://knpuniversity.com/screencast/symfony/json-api)
#### [Creating API Endpoints](http://knpuniversity.com/screencast/symfony/json-api#creating-api-endpoints)
##### src/AppBundle/Controller/GenusController.php
```
<?php
namespace AppBundle\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;
class GenusController extends Controller
{
    /**
     * @Route("/genus/{genusName}")
     */
    public function showAction($genusName)
    {
        return $this->render('genus/show.html.twig', array(
            'name' => $genusName,
        ));
    }
    /**
     * @Route("/genus/{genusName}/notes")
     * @Method("GET")
     */
    public function getNotesAction($genusName)
    {
        $notes = [
            ['id' => 1, 'username' => 'AquaPelham', 'avatarUri' => '/images/leanna.jpeg', 'note' => 'Octopus asked me a riddle, outsmarted me', 'date' => 'Dec. 10, 2015'],
            ['id' => 2, 'username' => 'AquaWeaver', 'avatarUri' => '/images/ryan.jpeg', 'note' => 'I counted 8 legs... as they wrapped around me', 'date' => 'Dec. 1, 2015'],
            ['id' => 3, 'username' => 'AquaPelham', 'avatarUri' => '/images/leanna.jpeg', 'note' => 'Inked!', 'date' => 'Aug. 20, 2015'],
        ];
        $data = [
            'notes' => $notes
        ];
        return new Response(json_encode($data));
    }
}
```

#### [Missing Annotation use Statement](http://knpuniversity.com/screencast/symfony/json-api#missing-annotation-use-statement)
##### src/AppBundle/Controller/GenusController.php
```
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;

$ php bin/console debug:router
```

#### [The JSON Controller](http://knpuniversity.com/screencast/symfony/json-api#the-json-controller)
##### src/AppBundle/Controller/GenusController.php

```
class GenusController extends Controller
<?php
namespace AppBundle\Controller;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Response;
class GenusController extends Controller
{
    /**
     * @Route("/genus/{genusName}")
     */
    public function showAction($genusName)
    {
        return $this->render('genus/show.html.twig', array(
            'name' => $genusName,
        ));
    }
    /**
     * @Route("/genus/{genusName}/notes")
     * @Method("GET")
     */
    public function getNotesAction($genusName)
    {
        $notes = [
            ['id' => 1, 'username' => 'AquaPelham', 'avatarUri' => '/images/leanna.jpeg', 'note' => 'Octopus asked me a riddle, outsmarted me', 'date' => 'Dec. 10, 2015'],
            ['id' => 2, 'username' => 'AquaWeaver', 'avatarUri' => '/images/ryan.jpeg', 'note' => 'I counted 8 legs... as they wrapped around me', 'date' => 'Dec. 1, 2015'],
            ['id' => 3, 'username' => 'AquaPelham', 'avatarUri' => '/images/leanna.jpeg', 'note' => 'Inked!', 'date' => 'Aug. 20, 2015'],
        ];
        $data = [
            'notes' => $notes
        ];
        return new Response(json_encode($data));
    }
}
```

#### [JsonResponse](http://knpuniversity.com/screencast/symfony/json-api#jsonresponse)
##### src/AppBundle/Controller/GenusController.php
Add
```
use Symfony\Component\HttpFoundation\JsonResponse;
...
return new JsonResponse($data);
```
#### Path : http://127.0.0.1:8001/genus/AquaNote/notes


## Step 11 :
### [Generating URLs](http://knpuniversity.com/screencast/symfony/url-generation)
##### src/AppBundle/Controller/GenusController.php

#### [The Twig path() Function](http://knpuniversity.com/screencast/symfony/url-generation#the-twig-path-function)