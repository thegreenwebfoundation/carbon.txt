# How to link two domains using HTTP Via headers domain hashes

HTTP requests and responses can contain a number of extra headers, which you can use to send along extra metadata about the server serving the request.

Carbon.txt supports using the [HTTP Via header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/via), to declare that a HTTP response has been sent along by one or more intermediate entities. In our case usually a managed hosting provider.

1. **Follow the steps above to create your carbon.txt file**

   Follow Steps 1 to 4 of the [Getting Started guide](/README.md#getting-started) to create a carbon.txt file for your organisation.

2. **Create a domain hash for the domain you want to show as green**

   Create a domain hash. This is a SHA256 hash of your shared secret and the domain you want to establish a link to. Various online tools demonstrate how to make SHA 256 hashes( [see this example](https://codebeautify.org/sha256-hash-generator)). To make it easier, you can do all this in [our own observable notebook](https://observablehq.com/d/21dbe07b6d399868).

3. **Set the `via` header on HTTP responses to requests for the domain you want to show as green**

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, when a request comes in for _me.my-org.com_, you would configure the server serving the request to add the following Via header.

   ```HTTP
    Via: 1.1 https://my-org.com/carbon.txt <generated_domain_hash>
   ```

   **Note**: the domain hash would be a 64 character hash of me.my-org.com and your shared secret.

We maintain a set of [server config setup examples folder on github](https://github.com/thegreenwebfoundation/carbon.txt/tree/master/examples), for popular open source servers like Nginx, Caddy, Apache, and so on (pull requests gratefully accepted).

## Got more questions?

Read the [FAQ](/faq/FAQ.md), or [raise an issue](/issues).
