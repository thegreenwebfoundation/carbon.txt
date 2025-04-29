# How to link two domains using HTTP headers

HTTP requests and responses can contain a number of extra headers, which you can use to send along extra metadata about the server serving the request.

Carbon.txt supports using the `CarbonTxt-Location`, to declare that a HTTP response has been sent along by one or more intermediate entities. In our case usually a managed hosting provider.

1. **Create your carbon.txt file**

   Follow Steps 1 to 3 of the [Getting Started guide](https://carbontxt.org/quickstart) to create a carbon.txt file for your organisation.

2. **Set the `CarbonTxt-Location` header on HTTP responses to requests for the domain you want to show as green**

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, when a request comes in for _me.my-org.com_, you would configure the server serving the request to add the following header.

   ```HTTP
    CarbonTxt-Location: https://my-org.com/carbon.txt
   ```

We maintain a set of [server config setup examples folder on github](https://github.com/thegreenwebfoundation/carbon.txt/tree/master/examples), for popular open source servers like Nginx, Caddy, Apache, and so on (pull requests gratefully accepted).

## Got more questions?

Read the [FAQ](/faq/FAQ.md), or [raise an issue](https://github.com/thegreenwebfoundation/carbon.txt/issues).
