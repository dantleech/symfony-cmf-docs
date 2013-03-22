BlogBundle
==========

This bundle aims to provide everything you need to create a full blog or
blog-like system. It also includes in-built support for the Sonata Admin
bundle to help you get up-and-running quickly.

Current features:

* Host multiple blogs within a single website.
* Place blogs anywhere within your route hierarchy.
* Sonata Admin integration.

Pending features:

* Full tag support
* Frontend pagination (using knp-paginator)
* RSS/ATOM feed
* Comments
* User support (FOSUserBundle)

Dependencies
------------

* :doc:`SymfonyCmfRoutingExtraBundle<routing-extra>` is used to manage the routing.
* :doc:`SymfonyCmfRoutingAutoBundle<routing_auto>` is used to automatically create routes.
* :doc:`PHPCR-ODM<phpcr-odm>` is used to persist the bundles documents.

Configuration
-------------

The default configuration will work with the ``cmf-sandbox`` but you will probably
need to cusomize it to fit your own requirements.

Parameters:

* **blog_basepath** - *required* Specify the path where the blog content should be placed.

Example:

.. code-block:: yaml

    symfony_cmf_blog:
        blog_basepath: /cms/content

Routing
~~~~~~~

To enable the routing system to automatically forward requests to the blog
controller when a Blog content is associated with a route, add the following
under the ``controllers_by_class`` section of ``symfony_cmf_routing_extra``
in ``app/config/config.yml``:

.. code-block:: yaml

    symfony_cmf_routing_extra:
        ...
        dynamic:
            ...
            controllers_by_class:
                ...
                Symfony\Cmf\Bundle\BlogBundle\Document\Blog: symfony_cmf_blog.blog_controller:listAction

.. note::

   In the BlogBundle the controller is a *service*, and is referenced as such. You can
   of course specify a controller using the standard `MyBundle:Controller:action`
   syntax. See `controllers as services <http://symfony.com/doc/current/cookbook/controller/service.html>`_ in the official sf2 docs.

To enable the automatic creation of routes you will also need to configure
the automatic routing system. This can be done simply by importing the default
configuration:

.. code-block:: yaml

    import: { @SymfonyCmfBlogBundle/Resources/config/routing/autoroute_default.yml }

This default configuration will create URLs of the format ``/blog/my-blog/2013-12-11/my-blog-post``
where ``my-blog`` is the blog title, ``2013-12-11`` is the post date and ``my-blog-post``
is the title of the post.

You can easily customize this configuration, see the :doc:`routing_auto` documentation 
for more information.

Sonata Admin
~~~~~~~~~~~~

The ``BlogBundle`` has admin services defined for Sonata Admin, to make the blog
system visible on your dashboard, add the following to the
``sonata_admin`` section:

.. code-block:: yaml

    sonata_admin:
        ...
        dashboard:
            groups:
                ...
                blog:
                    label: blog
                    items:
                        - symfony_cmf_blog.admin
                        - symfony_cmf_post.admin

Tree Browser Bundle
~~~~~~~~~~~~~~~~~~~

If you use the Symfony CMF Tree Browser bundle you can expose the blog routes
to enable blog edition from the tree browser. Expose the routes in the
``fos_js_routing`` section of ``app/config/config.yml``:

.. code-block:: yaml

    fos_js_routing:
        routes_to_expose:
            ...
            - admin_bundle_blog_blog_create
            - admin_bundle_blog_blog_delete
            - admin_bundle_blog_blog_edit

Templating
~~~~~~~~~~

The default templates are marked up for `Twitter Bootstrap <http://twitter.github.com/bootstrap/>`_.
But it is easy to completely customize the templates by **overriding** them.

The one template you will have to override is the default layout, you will need
to change it and make it extend your applications layout. The easiest way to do
this is to create the following file:

.. code-block:: jinja

    {# /app/Resources/SymfonyCmfBlogBundle/views/default_layout.html.twig #}

    {% extends "MyApplicationBundle::my_layout.html.twig" %}

    {% block content %}
    {% endblock %}

The blog will now use ``MyApplicationBundle::my_layout.html.twig`` instead of
``SymfonyCmfBlogBundle::default_layout.html.twig``.

See `Overriding Bundle Templates <http://symfony.com/doc/2.0/book/templating.html#overriding-bundle-templates>`_ in the Symfony documentation for more information.
