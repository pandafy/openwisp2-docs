Configuration Templates
=======================

Templates are designed to store configuration that can be reused by
some or all the devices in the system.

Updating the configuration stored in a template allows to update the
configuration of all the devices that have that template assigned.

This means that configuration can be defined only once for multiple
devices, and if the need to update a specific piece of configuration
arises, it can be easily achieved by updating the template.

.. contents:: **Table of Contents**:
   :backlinks: none
   :depth: 3

Template ordering and override
------------------------------

A device can use multiple templates, **the order in which templates are
assigned to each device matters**: templates assigned last can override
templates assigned earlier, the order can be changed by drag and dropping
the template in the device configuration page as in the animated
screenshot below.

.. image:: ../images/templates/template-ordering.gif
   :align: center
   :alt: Template ordering: drag and drop to change order

The device configuration can also override what is defined in templates.

Overriding means redefining a specific configuration section in a way that
overwrites the template.

**Overriding involves some form of duplication of information, which is
not great, it should be used as a last resort**. The recommended way to
define parts of the configuration that are specific for each device is to
use :doc:`Configuration variables <../user/configuration-variables>`.

Shared templates vs organization specific
-----------------------------------------

Templates can be *organization specific* or *shared*
(no organization specified).

.. image:: ../images/templates/organization-specific-vs-shared.gif
   :align: center
   :alt: Shared templates vs organization specific

**Organization specific templates** will be available and usable only
within the same organization which they are assigned to.

If no organization is specified when creating a template, a shared
template will be created, **shared templates are available to any
organization in the system**.

Here are a few typical use cases of shared templates:

- Management VPN
- Authorized SSH keys belonging to network administrators
- Crontab with generic periodic management operations

Default Templates
-----------------

.. image:: ../images/templates/default-templates.gif
   :align: center
   :alt: Templates enabled by default

When templates are flagged as **"Enabled by default"**,
they will be automatically assigned to new devices.

This is a very powerful feature: **once default templates are correctly
configured to implement the use case you need, you will only have to
register a device into OpenWISP for it to auto-configure itself**.

Moreover, you can change the default templates any time you need, which
is the reason this feature has replaced the practice of storing default
configuration in firmware images (which would need to be recompiled and
redistributed): with default templates, the default firmware image only
needs to contain the bare minimum configuration to connect to OpenWISP,
once the device connects to OpenWISP it will download and apply the
default templates without the need of manual intervention from
the network operators.

An organization specific template flagged as default will be automatically
assigned to any new device which will be created in the same organization.

A shared default template instead will be automatically assigned to all
the new devices which will be created in the system, regardless of
organization.

Required Templates
------------------

.. image:: https://raw.githubusercontent.com/openwisp/openwisp-controller/docs/docs/required-templates.png
  :alt: Required template example

Required templates are similar to
`Default templates <#default-templates>`__
but cannot be unassigned from a device configuration,
they can only be overridden.

They will be always assigned earlier than default templates,
so they can be overridden if needed.

In the example above, the "SSID" template is flagged as "(required)"
and its checkbox is always checked and disabled.

Template tags
-------------

.. image:: ../images/templates/template-tags.gif
   :align: center
   :alt: Template tags

`Default Templates`_ are an incredibly useful tool, but they're limited:
**only one set of default templates can be created**.

In some cases, you may have multiple set of default settings to use,
let's explain this with a practical example: you may have 2 different
device types in your network:

- Mesh routers: they connect to one another, forming a
  wireless mesh network
- Dumb access points: they connect to the mesh routers on the LAN
  port and offer internet access which is routed via the mesh
  network by the routers

In this example case, the default configuration to use in each
device type can greatly differ.

In such a setup, default templates would only contain configuration
which is common to both device types, while configuration which is
specific for each type would be stored in specific templates which
are then tagged with specific keywords:

- ``mesh``: tag to use for mesh configuration
- ``dumb-ap``: tag to use for dumb AP configuration

The `openwisp-config <https://github.com/openwisp/openwisp-config/#configuration-options>`_
configuration of each device type must specify the correct tag before
each device registers in the system.

Here's the sample ``/etc/config/openwisp`` configuration for mesh devices:

.. code-block::

    config controller 'http'
        option url 'https://openwisp2.mynetwork.com'
        option shared_secret 'mySharedSecret123'
        option tags 'mesh'

Once devices with the above configuration will register into the system,
any template tagged as ``mesh`` (as in the screenshot below) will be
assigned to them.

.. image:: ../images/templates/mesh-template-tag.png
   :align: center
   :alt: Template tags: mesh example

The sample ``/etc/config/openwisp`` configuration for dumb access
points is the following:

.. code-block::

    config controller 'http'
        option url 'https://openwisp2.mynetwork.com'
        option shared_secret 'mySharedSecret123'
        option tags 'dumb-ap'

Once devices with the above configuration will register into the system,
any template tagged as ``dumb-ap`` (as in the screenshot below)
will be assigned to them.

.. image:: ../images/templates/dumb-ap-template-tag.png
   :align: center
   :alt: Template tags: dumb AP example
