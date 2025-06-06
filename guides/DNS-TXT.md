# How to link two domains using DNS TXT records

DNS text records are frequently used to help organisations demonstrate they have control over a domain. The DNS TXT record approach follows similar principles.

To do this:

1. **Create your carbon.txt file**

   Follow Steps 1 to 3 of the [Getting Started guide](https://carbontxt.org/quickstart) to create a carbon.txt file for your organisation.

2. **Set a DNS record for the domains you want to link back to your main carbon.txt**

   Add the DNS TXT record for the domain you want to link back to your main carbon.txt file, with the location of the carbon.txt file.

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, they would create a TXT record that looks something like:

   ```DNS
   TXT "carbon-txt-location=https://my-org.com/carbon.txt"
   ```

   **Note:** you can see an example DNS TXT record for the domain [delegating-with-txt-record.carbontxt.org](https://delegating-with-txt-record.carbontxt.org) using online tools like [nslookup.io](https://www.nslookup.io/domains/delegating-with-txt-record.carbontxt.org/dns-records/txt/)

## Got more questions?

Read the [FAQ](/faq/FAQ.md), or [raise an issue](https://github.com/thegreenwebfoundation/carbon.txt/issues).
