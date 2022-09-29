# What is Disraptor?

## Introduction

The plugin does two things. *One*, it allows an administrator of a Discourse forum to configure routes from the Discourse instance to your web application. *Two*, it hooks into Discourse’s routing mechanism and redirects all requests to the configured source paths to their target URLs. Disraptor will render documents obtained with such a redirection inside the Discourse instance. In other words, Disraptor effectively turns Discourse into a reverse proxy for your web application.

Here are a few examples of possible route configurations:

- `/` → `http://example.org/`
- `/tasks` → `http://example.org/tasks/`
- `/static/*wildcard` → `http://example.org/static/*wildcard`

Disraptor uses Rails’ route syntax; thus, it’s possible to use [dynamic path segments](https://guides.rubyonrails.org/routing.html#dynamic-segments) and [wildcard path segments](https://guides.rubyonrails.org/routing.html#route-globbing-and-wildcard-segments) when setting up routes as shown in the third route configuration.



## Authentication

Your web application can make use of Discourse’s own authentication infrastructure. If a user is logged in via Discourse, Disraptor sends an `X-Disraptor-User` header with their user name to your web application. Similarly, the `X-Disraptor-Groups` header contains a list of associated groups.

Since Disraptor will only populate these header fields if there is any information to send, you can use the presence of the `X-Disraptor-User` header as the signal that the user is logged in.

Note that Disraptor will also allow authentication via POST requests. Any responses with status “303 See Other” will redirect to the URL in the `Location` header.



## App secret key

Disraptor requires a secret key in order to communicate with your web application. In essence, the secret key is really just a signal that *allows the Discourse instance to send requests to your web application*. Without it, Disraptor will not send requests to your web application. If set up, Disraptor will always send an `X-Disraptor-App-Secret-Key` header with its requests to your web application. **Your web application has to evaluate whether the secret key is correct**.



## Rendering modes

Disraptor has two rendering modes for HTML. The current default mode is the legacy mode. The shadow DOM mode can be enabled in the Discourse settings under “disraptor shadow dom”. Be aware, that the shadow DOM mode has yet to be tested in a live environment.

### Legacy mode

The legacy mode parses your web application’s HTML and injects the contents of the `<body>` tag into a Discourse page. All `<link>`, `<style>`, and `<script>` tags are injected into the `head` element of the Discourse page.

### Shadow DOM mode

The experimental shadow DOM mode parses the HTML and hooks it into the host document as a shadow tree.

This way, no `head` content needs to be transferred to the host document. Also, style isolation between Discourse and Disraptor documents is achieved without the need for prefixing, etc.

Known issues with the shadow DOM mode:

- [Shadow tree navigation doesn’t go through Ember router](https://meta.discourse.org/t/shadow-tree-navigation-doesn-t-go-through-ember-router/103712): Fixed in the plugin; can be fixed in Discourse.
- Unstyled document: Occasionally, a document will appear completely unstyled in Firefox until the user opens or closes the developer tools. That’s potentially a browser bug in Firefox.
- @font-face issue: Loading fonts with the CSS `@font-face` at-rule doesn’t work when the rule is inside the shadow DOM. This should be evaluated again in the future.
