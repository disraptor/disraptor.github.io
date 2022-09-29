# 4. Setup Disraptor
&larr; [3. Deploy Discourse with Disraptor](/docs/deploy-discourse-with-disraptor.md)

---

Before setting up Disraptor, you have to setup either a development ([Setup a Discourse development environment (Ubuntu)](setup-a-discourse-development-environment-ubuntu.md)) or a production environment ([Deploy Discourse with Disraptor](deploy-discourse-with-disraptor.md)).

---

After setting up a working development or production environment for Discourse and installing the Disraptor plugin, the following steps need to be performed for Disraptor to operate reliably:

1. Open Discourse’s settings and configure the value for “disraptor app secret key”.

   Use this secret key in your web application as a signal that the Discourse instance is properly set up and allowed to communicate with the server your web application is running on. Only if the key is correct can you be sure that requests made to the server are legitimately coming from the Discourse instance.

2. Open Discourse’s “Plugins” page. From there, open the Disraptor plugin page.

   Configure all routes that your web application needs to expose. You can use Rails’ route syntax to configure wildcard routes like `/static/*wildcard`. See Rails’ documentation on [dynamic path segments](https://guides.rubyonrails.org/routing.html#dynamic-segments) and [wildcard path segments](https://guides.rubyonrails.org/routing.html#route-globbing-and-wildcard-segments) for more examples.

Any Disraptor application is subject to a set of limitations that are necessary for Discourse and Disraptor to interact nicely: [Limitations for Disraptor documents and resources](disraptor-limitations-for-documents-and-resources.md)
