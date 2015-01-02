Node app launcher is a node app to handle the deployment and bootstrapping of your node app.

It is usefull if your node.js hosting does not support it natively.

#Motivation

In 2014, Gandi offered to be a partner of [Silex Labs foundation](http://www.silexlabs.org) for the development of [Silex, the free website builder](http://www.silex.me). Since then, Gandi offers node.js hosting for Silex staging and production servers.

As [this article states, Gandi node.js hosting has limitations](http://the.webapp.cat/???) and issues with:

* deployment with git push
* nodejs bootstrapping
* sftp access
* ssh access


    git is available but will not work with submodules
    deployment with git push is only intended to push new code, not to deploy with make, grunt or other automation tools. this will not restart the instance either
    nodejs bootstrapping: one need to have a server.js file, which is not standard and will not be included in the code base.. Use a Procfile, or package.json file would be a better approach
    sftp access is available but deploying using sftp is dangerous and still requires to access the gandi admin panel in order to reboot the instance
    ssh access requires to access the gandi admin panel to be enabled and the console is buggy as stated by users in the forums
    as nodejs often crashes, the instance should be monitored and rebooted a lot faster than it is
    pas de mode "test" / sandbox / preprod, qui soit gratuit et indépendant de l'instance en prod, avec exactement les mêmes conditions (mais une charge quasi inexistante)

This project aims at offering a node.js solution to adress these issues.

#What it does

This application exposes web services to manage the app.

The functionnalities exposed as web services:

* deployment of your app from a git branch or tag
* (re)start your app as a new process, and stop the old process only if the new one has started normally

Here are other things in the scope of this app, which I hope to add soon:

* check that your app is always running
* enable several monitoring tools (e.g. look)

#License

license: GPL v2

----

?? deployment script for silex
=> http://doc.rpc.gandi.net/hosting/usage.html
=> clone, make, reboot
    ssh -i ~/.ssh/id_rsa.pub 1740829@console.dc0.gpaas.net
    git clone https://github.com/lexoyo/Silex.git Silex
    cd Silex/
    git submodule update --init --recursive
    # not on gandi: make
    npm install
    => pas de java
    rsync

releases / déploiement gandi
* online
    activer ssh
* remote
    ssh -i ~/.ssh/id_rsa.pub 1740829@console.dc0.gpaas.net
    ou prod :
    ssh -i ~/.ssh/id_rsa.pub 1729213@console.dc0.gpaas.net
    cd web/vhosts/default/Silex/
    ?? git reset
    git pull --rebase origin master
    git submodule update --recursive
    npm install => error mais c'est ok
* local
    make
    copy with filezilla
    * dist/ => /lamp0/web/vhosts/default/Silex/dist
        or prod (ne marche pas sur gandi)
        cd ~/.ssh/
        rsync -vaz --rsh="ssh -i id_rsa.pub 1729213@console.dc0.gpaas.net" ~/Dropbox/fdt-workspace/Silex/ 1729213@console.dc0.gpaas.net:/srv/data/web/vhosts/default/Silex
* online
    reboot

Logs

tail -f /srv/data/var/log/www/nodejs.log

The standard output and errors of your website are sent to a log file on your disk:

via the SSH console: /srv/data/var/log/www/nodejs.log
via SFTP: /lamp0/var/log/www/nodejs.log
This log file also contains the output of the NPM program that is in charge of your website's dependencies.

Additionally, you can view the history of actions taken by the program in charge of launching your site:

via the SSH console: /srv/data/var/log/www/nodejs-watchd.log
via SFTP: /lamp0/var/log/www/nodejs-watchd.log

bug report gandi
=> faire un article the.webapp.cat
  + demander un article pour advertise notre partenariat sur http://www.lebardegandi.net/
=> Pour ce qui est de tes remontées de bug, elles peuvent être précieuses :) Est-ce que tu pourrais alors remonter celles-ci sur feedback@gandi.net s'il te plait ?
=> @lexoyo Hi! We'd love more details on this graph. Could you contact us (to AJ's attention)?  https://twitter.com/lexoyo/status/503092236348039168
* j'ai fais qques wish sur https://www.gandi.net/hosting/simple/wishlist/
* git
  pas de sous module => pas possible
  push comme dans la doc => fatal: repository not found
* sftp
  simple hosting instance admin page: wrong help page for sftp (http://wiki.gandi.net/en/simple/git instead of http://wiki.gandi.net/en/simple/sftp)
* nodejs :
  Your Node.js application must be composed of at least a file called server.js.
  => convention de merde
  systeme de vhost pour nodejs => 1 seule app, et si on utilise un proxy, on doit reload tout pour mettre en preprod? manipuler les thread sinon?
  vhosts modifie direct le record => tres bon ca!
* ssh :
    Console SSH à réactiver toutes les heures?
    pas de java => pas de deploy, et pas de apt-get => pas d'install


