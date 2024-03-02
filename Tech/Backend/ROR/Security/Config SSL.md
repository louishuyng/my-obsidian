---
tags: ROR, Backend
---
## Source
https://www.pluralsight.com/guides/using-https-with-ruby-on-rails

## Introduction

HTTPS is the secure, encrypted version of the HTTP protocol. To serve a Ruby on Rails application via HTTPS, there are three steps that you need to follow:

- Obtain an SSL certificate

- Configure the web server to use the SSL certificate

- Configure the Ruby on Rails application for HTTPS

In this guide, we'll take a look at each of these steps. Since there are several possible alternatives to serve a Ruby on Rails application via HTTPS, here we'll focus on the most common options and configurations.

If you are migrating an existing Ruby on Rails application from HTTP to HTTPS, there are a number of small changes and best practices that you should follow to simplify the transition and guarantee compatibility with both protocols. I'll mention a few of them at the end of this guide.

## Obtaining an SSL Certificate

There are [several different types of SSL certificates](https://simonecarletti.com/blog/2013/11/ssl-certificate-types/). You can group them by validation level (domain validated, organization validated, extended validation), by coverage (single-name, wildcard, multi-domains, etc.), and authenticity (self-signed vs. publicly-trusted certificate authorities).

In general, _single-name_ or _wildcard_ certificates are the most common choices. They allow you to secure a single hostname (such as `www.example.com`) or an entire subdomain level (such as `*.example.com`). Unless you need some extra level of validation, _domain validated certificates_ are the cheapest and most common solution. The second-most-popular alternative is the _extended validation_ certificate, which is generally recognized by the green bar displayed by the browsers in the address bar.

![image](https://d2mxuefqeaa7sj.cloudfront.net/s_5208C718E585F4ACF18509DB8B9E7D16BE8F660F1FA1C856DE8EB55FF12C12C8_1451482192246_Screen_Shot_2015-12-30_at_14_28_25.png)

For production environments, you are required to purchase an SSL certificate issued by a trusted certificate authority (e.g. [Digicert](https://www.digicert.com/), [Comodo](https://www.comodo.com/), [Let's Encrypt](https://letsencrypt.org/)) or a reseller (e.g. [DNSimple](https://dnsimple.com/ssl-certificates)). To purchase a trusted SSL certificate, follow the instructions provided by the certificate provider.

For non-production applications, you can avoid the costs associated with the SSL certificate by using a self-signed SSL certificate. If you use a self-signed certificate the connection will still be encrypted, however, your browser will likely display a security warning because the certificate is not issued by a trusted certification authority. You can [follow these instructions](https://devcenter.heroku.com/articles/ssl-certificate-self) to generate a self-signed certificate.

Regardless of the type of certificate, at the end of the issuance process you should obtain the following files:

- The public SSL certificate

- The private key

- Optionally, a list of intermediate SSL certificates or an intermediate SSL certificate bundle

These files are required to proceed to the next step and configure the web server to support HTTPS.

## Configuring the Web Server to Support HTTPS

In this section, we'll learn how to configure the most common Ruby on Rails web servers to serve an application under HTTPS. We'll use the public SSL certificate, the private key, and the intermediate SSL chain we obtained in the previous step.

For the purpose of the examples, I'll use the following file names:

- `certificate.crt` - the public SSL certificate

- `private.key` - the private key

Depending on the web server, you may need to supply the SSL intermediate chain in a single file along with the public SSL certificate, or use two separate files:

- `chain.pem` - the intermediate SSL certificate bundle

- `certificate_and_chain.pem` - the SSL certificate and intermediate SSL certificate bundle

### Intermediate and SSL Certificate Bundle

The creation of the intermediate SSL certificate bundle is generally one of the most confusing steps, therefore it deserves a special mention.

The bundle is just a simple text file that contains the concatenation of all the intermediate SSL certificates. The order is generally in reverse order, from the most specific intermediate SSL certificate to the most generic (and/or the root certificate).

The root certificate is generally omitted, as it should be bundled in the browser or in the operating system. If the bundle has to contain the server SSL certificate, then this must appear as the first certificate in the list (as this is the most specific).

```
1SERVER CERTIFICATE
2INTERMEDIATE CERTIFICATE 1
3INTERMEDIATE CERTIFICATE 2
4INTERMEDIATE CERTIFICATE N
5ROOT CERTIFICATE
```

You can use a text editor to concatenate the files together, or the `cat` unix utility.

```bash
1cat certificate.crt interm1.crt intermN.crt root.csr > certificate_and_chain.pem
2cat interm1.crt intermN.crt root.csr > chain.pem
```

bash

### Nginx

Nginx is generally used as a front-end proxy for other Rack web servers, such as Unicorn, Passenger, Puma or Thin.

To configure HTTPS on Nginx, the `ssl` parameter must be enabled on listening sockets in the server block, and you must specify the locations of the SSL certificate bundle and private key files:

```
1server {
2    listen 443 ssl;
3    server_name www.example.com;
4    ssl_certificate /path/to/certificate_and_chain.pem;
5    ssl_certificate_key /path/to/private.key;
6    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
7    ssl_ciphers HIGH:!aNULL:!MD5;
8    ...
9}
```

See also [Configuring HTTPS Servers for Nginx](http://nginx.org/en/docs/http/configuring_https_servers.html). You may want to refer to [cipherli.st](https://cipherli.st/) for the recommended cipher and protocol configurations.

Although you can configure a server block to listen on both `80` and `443`, it's generally a common practice to configure two different server blocks: one for HTTP and one for HTTPS.

Don't forget to restart the web server to apply the configurations.

### Apache

Apache is generally used in combination with Passenger.

To configure HTTPS on Apache, the `VirtualHost` must listen to the port `443`, you must turn on the `SSLEngine`, and then you must specify the locations of the SSL certificate bundle and private key files.

```
1<VirtualHost *:443>
2    ServerName www.example.com
3    SSLEngine on
4    SSLCertificateFile "/path/to/certificate_and_chain.pem"
5    SSLCertificateKeyFile "/path/to/private.key"
6</VirtualHost>
```

If you are using a version of Apache before 2.4.8, please note that [you need to use two separate files for the public SSL certificate and the SSL intermediate chain](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html#sslcertificatechainfile).

```xml
1<VirtualHost *:443>
2    ServerName www.example.com
3    SSLEngine on
4    SSLCertificateFile "/path/to/certificate.crt"
5    SSLCertificateChainFile "/path/to/chain.pem"
6    SSLCertificateKeyFile "/path/to/private.key"
7</VirtualHost>
```

xml

See also [SSL/TLS Strong Encryption for Apache](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html). You may want to refer to [cipherli.st](https://cipherli.st/) for the recommended cipher and protocol configurations.

Don't forget to restart the web server to apply the configurations.

### Heroku

Heroku is a popular platform as a service (PaaS) also used to deploy Rack applications. Heroku is a managed environment, therefore you don't get access to the low-level web server configuration.

In order to configure the SSL certificate on Heroku, you have to use the Heroku CLI to [provision the SSL endpoint](https://devcenter.heroku.com/articles/ssl-endpoint#provision-the-add-on)

```shell
1$ heroku addons:create ssl:endpoint
```

shell

and install the SSL certificate using the private key and the full SSL certificate bundle composed by the certificate and the chain (if any).

```shell
1$ heroku certs:add certificate_and_chain.pem private.key
```

shell

### Amazon AWS

How you configure AWS to serve HTTPS depends on the specific AWS service you use. If you use a single EC2 instance, then you'll have to install a web server to host your Ruby on Rails application and the configuration depends on the specific server you will use. If you use Nginx or Apache along with a specific Rack web server, you can follow the instructions published in the previous sections.

See also [configuring HTTPS for Single-Instance Environments](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/https-singleinstance.html) and [configuring HTTPS on single instances of Ruby](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/https-singleinstance-ruby.html).

## Configuring the Ruby on Rails Application for HTTPS

By default, once you properly configured your web server for HTTPS, you should be able to load your Ruby on Rails application via HTTPS without further configurations.

However, to effectively serve a Ruby on Rails application via HTTPS, it is recommended that you flag the cookies as `secure`, respond with the appropriate HTTPs security headers, and redirect HTTP traffic to HTTPS.

### `force_ssl` Configuration

In the recent Ruby on Rails versions, the framework provides a configuration flag called `force_ssl` that you can enable to force the application to run under HTTPS. This feature is available since Ruby on Rails 3.1.

You can turn this feature on in a specific environment by setting the value to `true` in the environment file.

```ruby
1# config/environments/production.rb
2Rails.application.configure do
3  # ...
4
5  # force HTTPS on production
6  config.force_ssl = true
7end
```

ruby

Please remember to restart your Ruby on Rails web server to apply the changes.

You can also turn on the feature globally in the `application.rb` file and this will apply to all the environments. However, be aware that this will also affect your `test` and `development` environments.

```ruby
1module MyApp
2  class Application < Rails::Application
3
4  # ...
5
6  # force HTTPS on all environments
7  config.force_ssl = true
8end
```

ruby

Once enabled, the framework will perform the following actions for you:

- All cookies set by the application [will be flagged as secure](http://tools.ietf.org/html/rfc6265#section-4.1.2.5)

- The response will contain the [HTTP Strict Transport Security (HSTS) security header](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) that will instruct the browser to perform subsequent requests via HTTPS only

- All HTTP requests will be redirected to HTTPS

It's important to note that this configuration applies to the entire environment: it affects all requests sent to the application and it's not possible to enable/disable it on specific controllers or actions. Furthermore, it's not possible to run the application under HTTP and HTTPS in parallel because of the point.

### The `rack-ssl` Gem

If you use a Ruby on Rails version lower than 3.1, or if you want a little bit of more control, you can use the [`rack-ssl`](https://rubygems.org/gems/rack-ssl) . This gem is a `Rack` middleware that provides exactly the same feature of the `force_ssl` feature. In fact, Ruby on Rails [used to rely on](https://github.com/rails/rails/commit/2c0c4d754e34b13379dfc53121a970c25fab5dae) .

To use this gem in a Ruby on Rails application, add the dependency to the `Gemfile`

```ruby
1gem 'rack-ssl', require: 'rack/ssl'
```

ruby

and install it with `Bundler`

```shell
1$ bundle
```

shell

Then, add the middleware to the application middleware stack:

```ruby
1# config/application.rb
2config.middleware.insert_before ActionDispatch::Cookies, "Rack::SSL"
```

ruby

It is recommended that you add the middleware in a position at least before `ActionDispatch::Cookies` and `ActionDispatch::Static` (if loaded), because the middleware may need to optimize the response from the two middleware for HTTPS.

`Rack::SSL` is a little bit more flexible than the `force_ssl` configuration. You can enable it on a per-request basis by passing the `:exclude` option.

The following example disables the middleware if the request comes from HTTP and it allows to run the application in parallel to HTTP and HTTPS.

```ruby
1config.middleware.insert_before ActionDispatch::Cookies, "Rack::SSL",
2                                exclude: ->(env) { !Rack::Request.new(env).ssl? }
```

ruby

This approach is particularly useful to silently rollout HTTPS in parallel to the HTTP version of the site, to test how your application behaves and fix bugs before forcing the entire site to HTTPS.

You can also configure the HSTS header by setting, for example, a different expiration and to include subdomains.

```ruby
1config.middleware.insert_before ActionDispatch::Cookies, "Rack::SSL",
2                                hsts: { expires: 2.years, subdomains: true }
```

ruby

Or you can disable the HSTS header completely. This is useful if the header is already set at another level, for instance, by a front-end proxy such as HAProxy or Nginx.

```ruby
1config.middleware.insert_before ActionDispatch::Cookies, "Rack::SSL",
2                                hsts: false
```

ruby

Of course, you can combine these options together:

```ruby
1config.middleware.insert_before ActionDispatch::Cookies, "Rack::SSL",
2                                exclude: ->(env) { !Rack::Request.new(env).ssl? },
3                                hsts: false
```

ruby

You can also inject the middleware in a specific environment to enable it only for that specific environment. Alternatively, you can use the `exclude` configuration in combination with an environment check.

Although the `rack-ssl` gem is a little bit more customizable, it still works at the Rack middleware level, therefore, it's a little bit tricky to selectively enable or disable it on a per-controller or per-action basis. You could, in theory, use the `exclude` option to filter by request path, but this approach may quickly become cumbersome if you have a large number of routes.

### HTTPS Configuration per Controller or per Action

There are possible solutions to force HTTPS only on specific actions or controllers. The most common approach is to use a `before_action` to force HTTPS whenever required, however, I personally discourage this approach as it drastically reduces the security of your application, it increases the complexity of your controllers, and it limits the ability to use extra security measures such as the security headers. Please consider serving the entire application via HTTPS, not just a portion of it.

In certain cases, a Ruby on Rails application is a container for more than one application or different components. Let's think for a second about Rails applications that consist of a front-end and an API, or a front-end, where the assets are served from different domains.

There may be cases where you need to enable HTTPS only on certain actions or controllers that represent one or more specific components. In these cases, it is recommended that you map these components to separate hostnames.

In other words, if the APIs are served via HTTPS and the website via HTTP, but they are both powered by the same application, you should use

```
1https://api.example.com/
2http://example.com
```

instead of

```
1https://example.com/api/
2http://example.com
```

as security policies such as the [`HSTS`](https://developer.mozilla.org/en-US/docs/Web/Security/HTTP_strict_transport_security) and the [`HPKP`](https://developer.mozilla.org/en-US/docs/Web/Security/Public_Key_Pinning) [security headers](https://securityheaders.io/) applies to an entire hostname.

It's also quite easy to enable the `rack-ssl` middleware only for a list of hostnames, rather than having to deal with controllers and actions.

## Tips and Tricks

The following is a list of suggestions and best practices you should follow to reduce the effort of migrating an existing Ruby on Rails applications from HTTP to HTTPS. Some of these best practices apply to Ruby on Rails or web development in general, and you can already follow them today regardless of your plans to move the application under HTTPS.

### Use the application router to dynamically generate URLs and links

One of the most important rules to follow to ensure the long-term maintainability of a Ruby on Rails application, aside from HTTPS, is to never hard-code the application routes anywhere in the code. Instead, you should always rely on the router to generate links and URLs.

It's not uncommon to see hard-code links like this one in mailer templates:

```html
1If you have any questions, feel free to <a href="http://example.com/contact">contact us</a>.
```

html

Another very common habit is to hard-code application URLs in the JavaScript files or static assets.

It's very important to always rely on the [Ruby on Rails routing helpers](http://guides.rubyonrails.org/routing.html) whenever you need to generate a route. The most essential routing helper is [`url_for`](http://api.rubyonrails.org/classes/ActionDispatch/Routing/UrlFor.html#method-i-url_for) which is very basic, but extremely powerful.

For named routes, Ruby on Rails provides the `_path` and `_url` helper. For example, if you have a route called `posts` that maps to `/posts`, then you can use:

- `posts_path` to generate `/posts`

- `posts_url` to generate `http://example.com/posts`

The `_url` helper is really powerful because it automatically constructs the full absolute link using the current request protocol and hostname. Therefore, if you use these helpers to generate absolute links, Ruby on Rails will automatically display HTTPS links if the request was for made via HTTPS.

This works very well in templates that are generated at runtime by the server. But what about a static file, such as a JavaScript file, that needs to send a request to the application? I mentioned before that it's not uncommon to see routes hard-coded in JavaScript files or CSS.

For JavaScript files, you can rely on the HTML data attribute to pass information to the script. Let's suppose you have a script that needs to send a POST request when the user clicks a button.

```js
1// somefile.js
2$.post("https://example.com/loader", function(payload) {
3  $("#result").html(payload);
4});
5
6<!-- file.html -->
7<div id="result"></div>
```

js

Instead, generate and render the URLs in the template using the `_url` helper:

```html
1<!-- file.html -->
2<div id="result" data-url="<%= loader_url %>"></div>
```

html

and change the JavaScript to fetch the target URL from the DOM.

```js
1// somefile.js
2$.post($("#result").data("url"), function(payload) {
3  $("#result").html(payload);
4});
```

js

```html
1<!-- file.html -->
2<div id="result"></div>
```

html

### Use Relative Paths Whenever Possible

If you use relative paths from the root, the browser will automatically resolve the links by appending the current request hostname and protocol to the path.

For example, replace

```html
1<link rel="shortcut icon" type="image/x-icon" href="http://example.com/images/favicon.png" />
```

html

with

```html
1<link rel="shortcut icon" type="image/x-icon" href="/images/favicon.png" />
```

html

This is not required if you use the Ruby on Rails routing helpers I mentioned in the previous section.

### Load Third-party Assets via HTTPS Whenever Possible

The most common error you will experience while transitioning from HTTP to HTTPS is the [mixed content warning](https://www.howtogeek.com/181911/htg-explains-what-exactly-is-a-mixed-content-warning/). This happens when your site is using HTTPS but you are loading an external resource (such as an image or a JavaScript file) via HTTP.

To prevent this issue, always link to the HTTPS version of a third party resource whenever you can. That will save you some headaches when you switch your website to HTTPS. In fact, in general there is absolutely no downside of embedding an HTTPS resource in a site loaded via HTTP, whereas the vice-versa is not true.

Most content providers are now distributing their resources both via HTTP and HTTPS. For example, let's suppose you want to embed a font via Google Font or jQuery from the `code.jquery.com` CDN: in both cases, you can link immediately to the HTTPS version of the resource, there is no reason to use the HTTP link.

In the past, it was also quite common to use the [protocol-relative link](http://www.paulirish.com/2010/the-protocol-relative-url/) to delegate to the browser the decision to use HTTP or HTTPS depending on the request.

```html
1<link rel="stylesheet" media="screen" href="//netdna.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.css" />
```

html

However, now that SSL is encouraged for everyone and doesn't have performance concerns, this technique is considered an anti-pattern. If the asset you need is available on SSL, then always use the `https://` link, as I mentioned before.

### Configure the Browser Security Headers

Both `force_ssl` and `rack-ssl` options are designed to quickly set up your application to get started with HTTPS with a default set of options and security configurations.

However, now that your Ruby on Rails application is running via HTTPS, you may want to fully take advantage of HTTPS and properly tweak the security headers returned by your application. Check out the course on this topic, [Introduction to Browser Security Headers](https://www.pluralsight.com/courses/browser-security-headers) from Troy Hunt. This course will help you to better understand what the security headers related to HTTPS are and how to properly use them.