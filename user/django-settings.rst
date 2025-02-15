How to Edit Django Settings
===========================

Modules (a.k.a Django Apps)
---------------------------

The OpenWISP server application is composed of a number of modules
called `Django apps <https://docs.djangoproject.com/en/4.0/intro/reusable-apps/>`_,
Django is the underlying web framework on top
of which OpenWISP is built.

Some of the Django apps used by OpenWISP are developed and maintained
by OpenWISP, other apps are developed and mainained by either Django
or third party organizations, but most of these apps are configurable
and customizable in different shapes or forms.

The most common way to modify the behavior of a Django app is by
editing the `settings.py file
<https://docs.djangoproject.com/en/4.0/topics/settings/>`_,
a file which holds all the global configuration of the application.

The OpenWISP django modules are highly configurable and over time
you may need to edit their settings, these settings are documented
in the respective README documentation files of each module,
for a list of all the available modules please see
:doc:`Architecture, Modules, Technologies <../general/architecture>`.

Editing settings with the ansible role
--------------------------------------

The official ansible OpenWISP role provides many
`configuration variables <https://github.com/openwisp/ansible-openwisp2#role-variables>`_
which offer a convenient way to edit the most widely used settings
of OpenWISP.

However, not all the possible settings have a corresponding variable
because doing so would be very costly to maintain and make the code
more complicated, for that reason the role provides a way to
add any python instruction to define and manipulate settings via the
``openwisp2_extra_django_settings_instructions`` variable, eg::

    # in the playbook variables add:
    openwisp2_extra_django_settings_instructions:
      - |
        OPENWISP_NETWORK_TOPOLOGY_NODE_EXPIRATION = 14

        OPENWISP_MONITORING_METRICS = {
            'ping': {
                'alert_settings': {'tolerance': 60}
            },
            'config_applied': {
                'alert_settings': {'tolerance': 60}
            },
            'disk': {
                'alert_settings': {'tolerance': 60}
            },
            'memory': {
                'alert_settings': {'tolerance': 60}
            },
            'cpu': {
                'alert_settings': {
                    'threshold': 95,
                    'tolerance': 60
                }
            },
        }

This allows for great flexibility in configuring and extending OpenWISP,
because additional custom modules can be added and configured too.

Editing settings with docker-openwisp
-------------------------------------

Similarly to the ansible role, the dockerized version of OpenWISP
provides mainly two ways of changing settings:

1. The most widely used setting have a
   `dedicated environment variable <https://github.com/openwisp/docker-openwisp/blob/master/docs/ENV.md>`_.
2. For more advanced use cases, it's possible
   `to provide an entirely custom django settings file
   <https://github.com/openwisp/docker-openwisp#custom-django-settings>`_.
