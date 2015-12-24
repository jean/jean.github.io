.. title: Hello world
.. slug: hello-world
.. date: 2015-12-24 16:43:53 UTC+07:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

The first post might as well be a write-up of configuring this blog.

In my case, I wanted to use Nikola for a Github Pages *user* site.

Setting up this site
````````````````````

Create a blank github repo for the site. For my user, that is
``https://github.com/jean/jean.github.io``.

Clone the repo and prepare the required branches. For a user site, Nikola will
*deploy* to the ``master`` branch, so you need another branch to keep the site
source in. I picked ``site-src``; the `documentation suggests
<https://getnikola.com/handbook.html#deploying-to-github>`_ ``deploy``.
Whatever name you choose, this will be the branch that you will keep checked
out and use for authoring.

.. code:: bash

    git clone git@github.com:USER/YOURSITE.github.io.git
    cd YOURSITE.github.io
    git checkout -b site-src
    git push origin site-src

Create the Nikola site. In my case I prefer to install Nikola into a
*virtualenv* local to the site. Nikola is dropping support for Python 2 soon,
so use Python 3.

.. code:: bash

    virtualenv -p `which python3` .   
    . bin/activate
    pip install nikola
    nikola init YOURSITE
    echo "
        include/
        lib/
        pip-selfcheck.json
        share/
        YOURSITE/cache
        YOURSITE/output
        YOURSITE/__pycache__
        YOURSITE/themes
        YOURSITE/.doit.db
        " > .gitignore
    git add YOURSITE
    git commit -a -m "First commit"

The default Nikola config assumes a *project* site (which publishes to the
``gh-pages`` branch), so needs to be updated to use the *user* format. Edit
``YOURSITE/conf.py``.

.. code:: python

      GITHUB_SOURCE_BRANCH = 'site-src' # Or whatever you picked.
      GITHUB_DEPLOY_BRANCH = 'master'

Now you're ready to start adding posts and pages. To do this, you need to be
in the ``YOURSITE`` directory:

.. code:: bash

   cd YOURSITE
   nikola new_post
   # Edit post
   nikola github_deploy

Nikola will build the static site in the ``output`` directory (by default) and
push it to the ``master`` branch at github.  Note however that the source will
not be automatically committed. You need to do this. To commit everything in
one go:

.. code:: bash

   git add .
   git commit -a -m "New post"

In my case I also installed a theme (``nikola install_theme material-theme``),
which requires another change to ``conf.py`` to activate the theme. I also
activated `disqus <http://disqus.com>`_ as comments provider.

