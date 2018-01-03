.. _authentication:

Authentication
==============

Authentication allows you to control who has access to your JupyterHub deployment.
There are many options available to you in controlling authentication, many of
which are described below.

Authenticating with OAuth2
--------------------------

JupyterHub's `oauthenticator <https://github.com/jupyterhub/oauthenticator>`_
has support for enabling your users to authenticate via a third-party OAuth
provider, including GitHub, Google, and CILogon.

Follow the service-specific instructions linked on the
`oauthenticator repository <https://github.com/jupyterhub/oauthenticator>`_
to generate your JupyterHub instance's OAuth2 client ID and client secret. Then
declare the values in the helm chart (``config.yaml``).

Here are example configurations for common authentication services. Note
that in each case, you need to get the authentication credential information
before you can configure the helmchart for authentication.

Google
^^^^^^

For more information see the full example of Google OAuth2 in the next section.

.. code-block:: yaml

    auth:
      type: google
      google:
        clientId: "yourlongclientidstring.apps.googleusercontent.com"
        clientSecret: "adifferentlongstring"
        callbackUrl: "http://<your_jupyterhub_host>/hub/oauth_callback"
        hostedDomain: "youruniversity.edu"
        loginService: "Your University"

GitHub
^^^^^^

.. code-block:: yaml

      auth:
        type: github
        github:
          clientId: "y0urg1thubc1ient1d"
          clientSecret: "an0ther1ongs3cretstr1ng"
          callbackUrl: "http://<your_jupyterhub_host>/hub/oauth_callback"

CILogon
^^^^^^^

.. code-block:: yaml

      auth:
        type: cilogon
        github:
          clientId: "y0urc1logonc1ient1d"
          clientSecret: "an0ther1ongs3cretstr1ng"
          callbackUrl: "http://<your_jupyterhub_host>/hub/oauth_callback"

Globus
^^^^^^

Globus Auth is a foundational identity and access management platform service
designed to address unique needs of the science and engineering community.
Globus provides cloud-based services for reliably moving, sharing, publishing
and discovering data, whether your files live on a supercomputer, lab cluster,
tape archive, public cloud, or your own laptop. Start a Globus app
`here <https://developers.globus.org/>`_!

.. code-block:: yaml

      auth:
        type: globus
        globus:
          clientId: "y0urc1logonc1ient1d"
          clientSecret: "an0ther1ongs3cretstr1ng"
          callbackUrl: "https://<your_jupyterhub_host>/hub/oauth_callback"
          identityProvider: "youruniversity.edu"


To add a whitelist of usernames add to the config file under `auth`:

.. code-block:: yaml

     auth:
         whitelist:
             users:
                - user1
                - user2

Full Example of Google OAuth2
-----------------------------

If your institution is a `G Suite customer <https://gsuite.google.com>`_ that
integrates with Google services such as Gmail, Calendar, and Drive, you can
authenticate users to your JupyterHub using Google for authentication.

.. note::
       Google requires that you specify a fully qualified domain name for your
       hub rather than an IP address.

1. Log in to the `Google API Console <https://console.developers.google.com>`_.

2. Select a project > Create a project... and set 'Project name'. This is a
   short term that is only displayed in the console. If you have already
   created a project you may skip this step.

3. Type "Credentials" in the search field at the top and click to access the
   Credentials API.

4. Click "Create credentials", then "OAuth client ID". Choose
   "Application type" > "Web application".

5. Enter a name for your JupyterHub instance. You can give it a descriptive
   name or set it to be the hub's hostname.

6. Set "Authorized JavaScript origins" to be your hub's URL.

7. Set "Authorized redirect URIs" to be your hub's URL followed by
   "/hub/oauth_callback". For example, http://example.com/hub/oauth_callback.

8. When you click "Create", the console will generate and display a Client ID
   and Client Secret. Save these values.

9. Type "consent screen" in the search field at the top and click to access the
   OAuth consent screen. Here you will customize what your users see when they
   login to your JupyterHub instance for the first time. Click Save when you
   are done.

10. In your helm chart, create a stanza that contains these OAuth fields:

.. code-block:: bash

    auth:
      type: google
      google:
        clientId: "yourlongclientidstring.apps.googleusercontent.com"
        clientSecret: "adifferentlongstring"
        callbackUrl: "http://<your_jupyterhub_host>/hub/oauth_callback"
        hostedDomain: "youruniversity.edu"
        loginService: "Your University"

The ``callbackUrl`` key is set to the authorized redirect URI you specified
earlier. Set ``hostedDomain`` to your institution's domain name. The value of
``loginService`` is a descriptive term for your institution that reminds your
users which account they are using to login.

Adding a Whitelist
------------------

JupyterHub can be configured to only allow a specified
`whitelist <http://jupyterhub.readthedocs.io/en/latest/getting-started/authenticators-users-basics.html#create-a-whitelist-of-users>`_
of users to login. This is especially useful if you are
using an authenticator with an authentication service open to the general
public, such as GitHub or Google.

You can specify this list of usernames in your `config.yaml`:

.. raw:: yaml

   auth:
     whitelist:
       users:
         - user1
         - user2
