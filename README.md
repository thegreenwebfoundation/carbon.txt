# carbon.txt

## What is carbon.txt?

A proposed convention for website owners and digital service providers to demonstrate that their digital infrastructure runs on green electricity. This is achieved by reusing existing governance structures, already published data, and existing industry standards as much possible.

_Note: this is a draft version and we are still ironing out how this might work. If you would like to contribute or have questions, please [open a GitHub issue](https://github.com/thegreenwebfoundation/carbon.txt/issues) or email [fershad@thegreenwebfoundation.org](mailto:fershad@thegreenwebfoundation.org)._

## What are the goals?

Having a convention around how website owners and service providers disclose information about the energy they use allows for two things:

1. It brings this information into the public domain in a way that is consistent, and easily accessible.
2. It lays the groundwork for a future where checking if a website/service uses green energy is democratised, and not locked to a single database or organisation.

In order to achieve this, we believe that any proposed convention should:

- use as much existing infrastructure as possible, especially as we already have structures and governance for making the internet work;
- be released with permissive licensing around use, so it can easily be built into modern tooling for continuous delivery and existing platforms/software;
- be human readable, as well as machine readable;

## Why do we need this?

We need a fossil free internet by 2030, but there's still not much transparency around how we source power at the moment. The Green Web Foundation has been building a [Green Domains database](https://www.thegreenwebfoundation.org/green-web-datasets/), open sources [the code for tracking this](https://github.com/thegreenwebfoundation/admin-portal) and is publishing [open datasets about the state of the green web](https://datasets.thegreenwebfoundation.org/).

Creating this dataset has been an ongoing effort for years. It has comprised of associating IP address space like IP ranges or Autonomous System Numbers with specific organisations - and then accompanying these links to manually verified written reports and certificates provided by said organisations to back up their green claims.

This has worked for the first few million domains, and first few billion checks against the database, but there are more tools that can help now, and the internet is a much bigger, more dynamic place than it was when this endeavour was started.

Tools like the domain name system (DNS) allow for a variety of ways to establish a link between a domain and the party that is hosting or operating it. Dedicated registries already exist to make it possible to trust that electricity has been generated from clean and renewable energy sources. And, if you know where to look, it's possible to tell when efforts to decarbonise energy are credible.

Carbon.txt serves to make links between these systems easier to follow, so when there is a claim that a website is running on green energy, you can trace the provenance down to the supporting evidence more easily.

## What are the benefits?

### For digital service providers

If you provide hosted digital services to others, carbon.txt lets you:

- Receive recognition in a human readable and machine readable way that the infrastructure you manage or use to provide your service runs on green energy.
- Earn trust from customers by helping creating an evidence base of action being taken by providers to help the world transition to a fossil free internet.
- Allow any downstream services or websites using your services to make the same claims, with a clear chain of attribution.
- Demonstrate leadership if you are moving faster in terms of a climate repsonse than the organisations in your supply chain by linking to your own work.

### For individual website owners

If you operate a website, or run an application yourself, you are able to:

- Demonstrate in human and machine-readable ways, the care you have taken in responsibly sourcing the providers that make up the supply chain in your own sites or applications.
- Help build the evidence base for tracking the transition of to a fossil free internet. Projects like the [HTTP Archive’s Web Almanac](https://almanac.httparchive.org/en/2022/) use datasets like these to provide a detailed breakdown of the overall "greenness" of the web every year. Your efforts provide the data needed for create the insights in these reports.

## Getting started

### For Digital Service Providers & Self-hosted Websites

1. **Register with The Green Web Foundation**

   As a digital service provider, you should first [register yourself with The Green Web Foundation](https://www.thegreenwebfoundation.org/green-web-check/register/), and provide evidence of your green claims.

2. **Create a carbon.txt file for your organisation**

   Create a carbon.txt file for your organisation. There is [a guide](#carbontxt-file-syntax-for-digital-service-providers) to the expected syntax below.

   For example: _<https://www.my-org.com/carbon.txt>_

   We default to checking at for a file located at the root of your domain (e.g. `/carbon.txt`), but if you need to store the file at a different path, we support setting your own path, like `.well-known/carbon.txt`. We [outline how](#how-to-link-two-domains-using-dns-txt-records-and-domain-hashes) further below.

3. **Share the URL of the carbon.txt file with the Green Web Foundation**

   The Green Web Foundation has an API for registering where to check for carbon.txt file for a given domain. Once this is listed, and the link established, your site shows as green.

   ```curl
   curl --location 'https://api.thegreenwebfoundation.org/api/v3/carbontxt' \
   --header 'Content-Type: application/json' \
   --data '{ "url": "https://my-org.com/carbon.txt" }'
   ```

4. **(Optional) Link other domains to your green claims if they are using infrastructure you control**

   If you offer managed hosted services to other organisations, once your first link is established there is an automated process for listing future domains so they show up as green too, with attribution to you. See [_domain hashes_](#linking-green-claims-to-multiple-domains-with-domain-hashes) below for more.

### Carbon.txt file syntax for digital service providers

The carbon.txt syntax is written in TOML. Below is an example of what a carbon.txt file might look like for a digital service provider.

```toml
[upstream]
providers = [
    # An array of providers my-org.com is using to deliver our service
    { domain='cloud.google.com', service = 'infrastructure' },
    { domain='aws.amazon.com', service = 'infrastructure' }
]

[org]
credentials = [
    # Optional.
    # An array of documents that point to evidence of green claims made by my-org.com.
    { domain='my-org.com', doctype = 'sustainability-page', url = 'https://my-org.com/our-climate-record'}
]
```

_An example of a carbon.txt file for a digital service provider._

### For Individual Website Owners

For individual website owners, you can set your own carbon.txt in order to provide supplementary information to what may be provided by any services you use. It also provides an avenue through which organisations can be very explicit about putting sustainability information into the public domain.

An example of what a carbon.txt file an individual website would look like is below:

```toml
[upstream]
providers = [
    # These providers include my hosting (Cloudflare Pages),
    # as well as all the third-party services I use on this site.
    # I feel it makes sense to keep these all together.
    { domain = "cloudflare.com", service = "hosting" },
    { domain = "usefathom.com", service = "analytics" },
    { domain = "cloudinary.com", service = "media" }
]

[org]
credentials = [
    # Optional.
    # These are evidence of things I do to account for the carbon emissions of this website.
    { domain = "my-website.com", doctype = "webpage", url = "https://my-website.com/climate/"}
]
```

_An example of a carbon.txt file that highlights the different providers used by a website._

## Linking green claims to multiple domains with domain hashes

It's fairly common that a digital service provider might own and provide services through multiple domains. They might also have multiple products, or provide hosted services for a number of users all who have their own domains.

In this case, you can maintain a single carbon.txt file at one domain (e.g. _<https://my-org.com/carbon.txt>_) and refer other domains to that single source of truth.

There are two supported ways to do this:

1. Using DNS TXT records, which contain the specific URL pointing to the carbon.txt file to read.
2. Using a dedicated `Via` HTTP Header, again containing a URL for the carbon.txt file to read.

The first of these is intended for organisations who are able to add DNS records to both their own domain, as well as the domain they want to show up as green. If you own both domains, this option is for you.

The second is intended for organisations who are not able to add DNS records for the domain they want to show up as green, but do accept HTTP requests for the domain, and serve responses for it. If you operate a CDN, a managed Wordpress service, or a general Platform-As-A-Service offering, this usually better suited for your use case.

### How to link two domains using DNS TXT records and domain hashes

DNS text records are frequently used to help organisations demonstrate they have control over a domains. The DNS TXT record approach follows similar principles.

To do this:

1. **Complete the steps above to register as a provider**

   See the steps above about registering with the Green Web Foundation

2. **Create a carbon.txt file.**

   As above.

3. **Create a shared secret for your provider**

   You created a shared secret by sending an authenticated request to the Green Web CarbonTxt Shared Secret API endpoint.

   ```curl
   curl --location --request POST 'https://api.thegreenwebfoundation.org/api/v3/carbontxt_shared_secret' \
   --header 'Authorization: <Base64 encoded username and password>'
   ```

4. **Create a domain hash for the domain you want to show as green**

   You create a domain hash. This is a SHA256 hash of your shared secret and the domain you want to establish a link to. Various online tools demonstrate how to make SHA 256 hashes( [see this example](https://codebeautify.org/sha256-hash-generator)). To make it easier, you can do all this in [our own observable notebook](https://observablehq.com/d/21dbe07b6d399868).

5. **Set a DNS record for the domains you want to link back to your main carbon.txt, containing the domain hash**

   You add the generated domain hash as a final, optional part of the DNS TXT record defining the domain you want to link back to your main carbon.txt file, along with the location of the carbon.txt file and the generated hash.

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, they would create a TXT record that looks something like:

   ```DNS
   TXT "carbon-txt=https://my-org.com/carbon.txt <generated_domain_hash>"
   ```

   **Note:** you can see an example DNS TXT record for the domain [delegating-with-txt-record.carbontxt.org](https://delegating-with-txt-record.carbontxt.org) using online tools like [nslookup.io](https://www.nslookup.io/domains/delegating-with-txt-record.carbontxt.org/dns-records/txt/)

#### How to link two domains using HTTP Via headers domain hashes

HTTP requests and responses can contain a number of extra headers, which you can use to send along extra metadata about the server serving the request.

Carbon.txt supports using the [HTTP Via header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/via), to declare that a HTTP response has been sent along by one or more intermediate entities - in our case usually a managed hosting provider.

1. **Complete the steps above to register as a provider**

   See the steps above about registering with the Green Web Foundation

2. **Create a carbon.txt file.**

   As above.

3. **Create a shared secret for your provider**

   You created a shared secret by sending an authenticated request to the Green Web CarbonTxt Shared Secret API endpoint.

   ```curl
   curl --location --request POST 'https://api.thegreenwebfoundation.org/api/v3/carbontxt_shared_secret' \
   --header 'Authorization: <Base64 encoded username and password>'
   ```

4. **Create a domain hash for the domain you want to show as green**

   You create a domain hash. This is a SHA256 hash of your shared secret and the domain you want to establish a link to. Various online tools demonstrate how to make SHA 256 hashes( [see this example](https://codebeautify.org/sha256-hash-generator)). To make it easier, you can do all this in [our own observable notebook](https://observablehq.com/d/21dbe07b6d399868).

5. **Set the `via` header on HTTP responses to requests for the domain you want to show as green**

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, when a request comes in for _me.my-org.com_, you would configure the server serving the request to add the following Via header.

   ```HTTP
    Via: 1.1 https://my-org.com/carbon.txt <generated_domain_hash>
   ```

   **Note**: the domain hash would be a 64 character hash of me.my-org.com and your shared secret.

We maintain a set of [server config setup examples folder on github](https://github.com/thegreenwebfoundation/carbon.txt/tree/master/examples), for popular open source servers like Nginx, Caddy, Apache, and so on (pull requests gratefully accepted).

## FAQs

While this concept is still new, we expect that some edge cases will be encountered. Here are some frequently asked questions we have internally.

### I am self-hosting my website

Self-hosting a website requires some degree of technical knowledge to setup and configure one/multiple servers to handle requests. Because of that, we assume that anyone who is self-hosting would be technically capable of setting up the require DNS/HTTP Header configurations of their site themselves.

In order for a self-hosted site to be considered green, we would also require the owner to register their site and their green claims with The Green Web Foundation. This would allow for the site to be added to the Green Web Dataset.

### Why this approach?

There are a few reasons for taking the approach we’ve described above.

Primarily, we are adopting an approach which leans heavily on existing web standards, and technologies. This allows for familiarity, and we hope will lead to these ideas being easier to adopt/implement.

We believe that providing a way for hosting providers/managed services to implement the carbon.txt specification for their product/s is key to broader adoption. It allows us, as a small not-for-profit driving this idea, to have a much larger reach & impact when compared to the alternative of relying on individuals to upload carbon.txt files to their own domains.

In the long run, we think that by demonstrating that you can use existing internet and web standards to link to make sustainability claims easily discoverable, as well as human and machine readable, conventions like carbon.txt will result in a web where it's easier to trust green claims, as well as follow them all the way back to the supporting evidence used to back them up.

### How can you trust this? What would stop me lying in my carbon.txt file about my site?

You could indeed list a green provider in your supply chain, and claim it was hosting your site, so your site showed up as green through association. Your site would not show up as green until you had been able to submit some supporting evidence for manual review that you _really were_ using that provider.

After manual review by our support staff, you would have managed to mark _one_ domain as green.

### What would stop me using someone else's carbon.txt file instead?

There are two mechanisms designed to mitigate against lying.

**1. Manual review for new domains** - just like with your own domain, we have a manual review step for any new domain delegating to one already trusted. New domains don't show as green until they have been added to an allow list for a given provider, or until a domain hash is made available when performing a lookup. More on domain hashes below.

**2. Domain hashes for newly seen domains** - domains are only automatically recognised as green if there is a matching domain hash available when they are submitted. Domain hashes rely on established, reassuringly boring technology that has been in use for years.

### How do you handle globally distributed providers?

The internet is a distributed network by its very nature. And, many hosting providers - especially CDNs and edge compute services - will serve content from locations around the world. Some of these locations could be in regions that run entirely (or almost entirely) on green energy.

At present, carbon.txt is designed to work at an organisational level, for organisations aiming to demonstrate that 100% of the carbon emissions associated with operating digital infrastructure have beenb accounted for.

We are looking into ways to surface greater geographic resolution for location-based carbon intensity, to reflect the physical reality. If you're interested in collaborating, please [get in touch](mailto:fershad@thegreenwebfoundation.org).
