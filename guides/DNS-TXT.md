# How to link two domains using DNS TXT records and domain hashes

DNS text records are frequently used to help organisations demonstrate they have control over a domain. The DNS TXT record approach follows similar principles.

To do this:

1. **Follow the steps above to create your carbon.txt file**

   Follow Steps 1 to 4 of the [Getting Started guide](/README.md#getting-started) to create a carbon.txt file for your organisation.

2. **Create a domain hash for the domain you want to show as green**

   Create a domain hash. This is a SHA256 hash of your shared secret and the domain you want to establish a link to.

   You can complete this step on [our website](https://carbontxt.org/how/digital-services/link-multiple-domains/dns#create-domain-hash).

3. **Set a DNS record for the domains you want to link back to your main carbon.txt, containing the domain hash**

   Add the generated domain hash as a final, optional part of the DNS TXT record defining the domain you want to link back to your main carbon.txt file, along with the location of the carbon.txt file and the generated hash.

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, they would create a TXT record that looks something like:

   ```DNS
   TXT "carbon-txt=https://my-org.com/carbon.txt <generated_domain_hash>"
   ```

   **Note:** you can see an example DNS TXT record for the domain [delegating-with-txt-record.carbontxt.org](https://delegating-with-txt-record.carbontxt.org) using online tools like [nslookup.io](https://www.nslookup.io/domains/delegating-with-txt-record.carbontxt.org/dns-records/txt/)

## Got more questions?

Read the [FAQ](/faq/FAQ.md), or [raise an issue](https://github.com/thegreenwebfoundation/carbon.txt/issues).
