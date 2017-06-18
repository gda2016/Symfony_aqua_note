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
- [ ] 8. Twig Layouts (Template Inharitance)
- [ ] 9. Loading CSS & JS Assets
- [ ] 10. JSON Responses + Route Generation
- [ ] 11. Generating URLs
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

