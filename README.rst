=============================
Horizon (OpenStack Dashboard)
=============================

Horizon is a Django-based project aimed at providing a complete OpenStack
Dashboard along with an extensible framework for building new dashboards
from reusable components. The ``openstack_dashboard`` module is a reference
implementation of a Django site that uses the ``horizon`` app to provide
web-based interactions with the various OpenStack projects.

* Project documentation: https://docs.openstack.org/horizon/latest/
* Release management: https://launchpad.net/horizon
* Blueprints and feature specifications: https://blueprints.launchpad.net/horizon
* Issue tracking: https://bugs.launchpad.net/horizon
* Release notes: https://docs.openstack.org/releasenotes/horizon/

.. image:: https://governance.openstack.org/tc/badges/horizon.svg
    :target: https://governance.openstack.org/tc/reference/tags/

Using Horizon
=============

See ``doc/source/install/index.rst`` about how to install Horizon
in your OpenStack setup. It describes the example steps and
has pointers for more detailed settings and configurations.

It is also available at
`Installation Guide <https://docs.openstack.org/horizon/latest/install/>`_.

Getting Started for Developers
==============================

``doc/source/quickstart.rst`` or
`Quickstart Guide <https://docs.openstack.org/horizon/latest/contributor/quickstart.html>`_
describes how to setup Horizon development environment and start development.

Building Contributor Documentation
==================================

This documentation is written by contributors, for contributors.

The source is maintained in the ``doc/source`` directory using
`reStructuredText`_ and built by `Sphinx`_

.. _reStructuredText: http://docutils.sourceforge.net/rst.html
.. _Sphinx: http://sphinx-doc.org/

To build the docs, use::

  $ tox -e docs

Results are in the ``doc/build/html`` directory




#Customizing OpenStack Horizon

Customizing OpenStack Horizon to reflect branding preferences involves modifying the Horizon source code and applying changes to the templates, stylesheets, and other relevant components. Keep in mind that the process might evolve over time as OpenStack Horizon gets updated, so remember to refer to the official documentation for any updates or changes in the Horizon codebase. 
Please note that making direct modifications to the Horizon codebase could lead to compatibility issues during future updates. It's recommended to create a custom theme or use the existing theming mechanisms provided by Horizon. In the following guide, we exemplify this recommendation by using Horizon's provided 'material' theme as a foundational element for customizing OpenStack Horizon. 
Note: Always test thoroughly in a development environment before applying changes to a production system. 

## Official Documentation

### 1. Configuring the Dashboard

- [Configuring the Dashboard](https://docs.openstack.org/horizon/latest/admin/customize-configure.html#configure-dashboard)

### 2. Customizing Styles and Themes

- [Customizing Styles and Themes](https://docs.openstack.org/horizon/latest/configuration/customizing.html)

## Custom Horizon Dashboard using Material Theme

In addition to the official documentation, here are examples of custom changes made to the OpenStack Horizon dashboard using material theming machanisms provided by Horizon.

## Enable material Theme

In `openstack_dashboard/settings.py`, update the available themes to include only the desired theme. For example, to enable the Material theme

```python
AVAILABLE_THEMES = [
    (
        'material',
        pgettext_lazy("Google's Material Design style theme", "Material"),
        'themes/material'
    ),
]

### Add Custom brand Images

Add necessary images (logos, backgrounds, favicons, etc.) in the `openstack_dashboard/static/dashboard/img/` directory. Update templates and stylesheets accordingly.

### CSS Adjustments

Adjust CSS for various Horizon components. For example, in `openstack_dashboard/static/dashboard/scss/components/_login.scss`

```scss
.splash-logo {
    margin: 65px 10px;
    max-width: 85%;
}

Change the pie chart color in `_pie_charts.scss`

```scss
.arc.inner {
  fill: #008000;
}

### Custom Favicons

Update the favicon references in `openstack_dashboard/templates/_stylesheets.html` with your custom brand favicon:

```html
<link rel="shortcut icon" href="{% themable_asset 'img/brand.png' %}"/>
<link rel="apple-touch-icon" sizes="180x180" href="{% themable_asset 'img/brand.png' %}" />
<link rel="mask-icon" href="{% themable_asset 'img/brand.png' %}" color="#5bbad5" />

### 5. Material Theme

For a more organized approach to material theme customization, consider creating a dedicated `_custom.scss` file within `openstack_dashboard/themes/material/static/`. This file will hold your new and unique custom styles.

For instance, to add a background image for the login page:

```scss
// _custom.scss
.img {
    background-image: url("../../../static/dashboard/img/brand-bg.png");
    background-color: #cccccc;
    height: 100vh;
    background-position: top left;
    background-repeat: no-repeat;
    background-size: cover;
    position: relative;
}

In `openstack_dashboard/themes/material/static/_styles.scss`, import your custom stylesheet:

```scss
// _styles.scss
@import "bootstrap/styles";
@import "horizon/styles";
@import "custom";
@import "_custom.scss";

### Theme brand Colors

Change color variables in `openstack_dashboard/themes/material/static/bootstrap/_variable_customizations.scss`

```scss
$brand-primary: #002D42;

### Additional Material Theme Adjustments

1. Adjust the size of the logo in `openstack_dashboard/themes/material/static/horizon/_styles.scss`

```scss
.login .splash-logo {
    width: 260px;
}

2. Replace Default logos with brand logos

   i) Login panel 
      `openstack_dashboard/themes/material/templates/auth/_splash.html`

      ```
      <div class="text-center">
        <img class="splash-logo" src="{{ STATIC_URL }}dashboard/img/brand-logo.png">
      </div>

  ii) Brand logo
      `openstack_dashboard/themes/material/templates/header/_brand.html`

      ```
      {% load branding %}

      <a class="navbar-brand" href="{% site_branding_link %}" target="_self">
        {% include "material/openstack-one-color.svg" %}
         <img src="{{ STATIC_URL }}dashboard/img/brand-logo.png" />
      </a>
