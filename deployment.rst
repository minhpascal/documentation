Deployment
==========

This application can be deployed on Heroku or on a traditional server.

After installation, you will be able to connect with the email
*root@jarr.localhost* and the password *password*.

Deploying the application with Vagrant
--------------------------------------

Installation of VirtualBox and Vagrant

.. code-block:: bash

    $ sudo apt-get install virtualbox
    $ wget https://dl.bintray.com/mitchellh/vagrant/vagrant_1.7.4_x86_64.deb
    $ sudo dpkg -i vagrant_1.7.4_x86_64.deb
    $ rm vagrant_1.7.4_x86_64.deb

Deployment of JARR

.. code-block:: bash

    $ git clone https://github.com/JARR-aggregator/JARR.git
    $ cd JARR/vagrant/
    $ vagrant up

Once the VM configured, go to the address http://127.0.0.1:5000.

Deploying the application on Heroku
-----------------------------------

An instance of JARR is running `here <https://jarr.herokuapp.com>`_.

The geek way
''''''''''''

.. code-block:: bash

    $ git clone https://github.com/JARR-aggregator/JARR.git
    $ cd JARR
    $ heroku create
    $ heroku addons:add heroku-postgresql:dev
    $ heroku config:set HEROKU=1
    $ git push heroku master
    $ heroku run init
    $ heroku ps:scale web=1

To enable account creation for users, you have to set some environment
variables:

.. code-block:: bash

    $ heroku config:set SELF_REGISTRATION=1
    $ heroku config:set PLATFORM_URL=<URL-of-your-platform>
    $ heroku config:set SECRET_KEY=<a secret token only you know in order to use sessions>
    $ heroku config:set SECURITY_PASSWORD_SALT=<a secret to confirm user account>
    $ heroku config:set TOKEN_VALIDITY_PERIOD=3600
    $ heroku config:set NOTIFICATION_EMAIL=<notification-email>
    $ heroku config:set POSTMARK_API_KEY=<your-postmark-api-key>
    $ heroku addons:add postmark:10k

`Postmark <https://postmarkapp.com/>`_ is used to send account confirmation links.

If you don't want to open your platform just set *SELF_REGISTRATION* to 0.
You will be still able to create accounts via the administration page.


The simple way
''''''''''''''

Alternatively, you can deploy your own copy of the app using this button:

.. image:: https://www.herokucdn.com/deploy/button.png
    :target: https://heroku.com/deploy?template=https://github.com/JARR-aggregator/JARR.git

You will be prompted to choose an email and a password for the administrator's account.
And some other optional environment variables, as previously presented.

Deploying the application on a traditional server
-------------------------------------------------

Configuration (database url, email, user agent, etc.) is done via the
file `src/conf/conf.cfg`.
Check this file before installing JARR.


.. code-block:: bash

    $ git clone https://github.com/JARR-aggregator/JARR.git
    $ cd JARR/

If you want to use PostgreSQL
'''''''''''''''''''''''''''''
.. code-block:: bash

    $ ./install.sh postgres

If you want to use SQLite
'''''''''''''''''''''''''

.. code-block:: bash

    $ ./install.sh sqlite

JARR is now ready. By default the one page app will be loaded from
`here <https://cdn.cedricbonhomme.org/bundle.min.js>`_. But you can also built
it yourself. You'll have to have Node.js installed:

.. code-block:: bash

    $ npm install
    $ npm run build

Then in the configuration file `src/conf/conf.cfg` set the variable
*cdn_address* to the empty string.

Finally launch the Web server:

.. code-block:: bash

    $ python src/runserver.py
     * Running on http://0.0.0.0:5000/
     * Restarting with reloader



Automatic updates
=================

You can fetch new articles with `cron <https://en.wikipedia.org/wiki/Cron>`_.
For example if you want to check for updates every 30 minutes, add this line to
your cron rules (*crontab -e*):

.. code-block:: bash

    */30 * * * * cd ~/.JARR/ ; python src/manager.py fetch_asyncio None None
