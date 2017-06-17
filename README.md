aqua_note
=========

A Symfony project created on June 17, 2017, 4:46 pm.

# TODO
## Step 1 : Joyful Development with Symfony 3
- [x] 1. [Start Project](http://knpuniversity.com/screencast/symfony/start-project)
- [x] 2. Setup PhpStorm + git
- [x] 3. First Page
- [x] 4. [Routing Wildcards](http://knpuniversity.com/screencast/symfony/routing-wildcards#play)
- [ ] 5. Intro to services
- [ ] 6. Listening and Using Services
- [ ] 7. Twig Layouts
- [ ] 8. Twig: For a Good time with Templates
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
####How? Change the URL to /genus/{genusName}:
##### src/AppBundle/Controller/GenusController.php
```

```

#### ! Tip
Be careful when rendering direct user input (like we are here)! It introduces a security issue called XSS - [read more about XSS here.](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)

#### Troubleshooting
##### Show routes
`$ bin/console debug:router | grep /genus/`
##### Clear cache
`bin/console cache:clear -e prod`