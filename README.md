# carbon.txt

## What is carbon.txt?

A proposed convention for website owners and digital service providers to demonstrate that their digital infrastructure runs on green electricity. This is achieved by reusing existing governance structures, already published data, and existing industry standards as much possible.

_Note: This project is currently in its very early stages. We are currently conducting a closed pilot to flesh out the specification, and identify use cases. If you would like to contribute or have questions, please [open a GitHub issue](https://github.com/thegreenwebfoundation/carbon.txt/issues) or [contact us by email](mailto:fershad@thegreenwebfoundation.org)._

## What are the goals?

Having a convention around how website owners and service providers disclose information about the energy they use allows for two things:

1. It brings this information into the public domain in a way that is consistent, and easily accessible.
2. It lays the groundwork for a future where open data is available to anyone wanting to check if a website/service uses green energy.

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

## Getting started

### For Digital Service Providers

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

1. **Follow the steps above to create your carbon.txt file**

Follow Steps 1 to 3 of the [Getting Started guide](#getting-started) above to create a carbon.txt file for your organisation.

2. **Create a domain hash for the domain you want to show as green**

   You create a domain hash. This is a SHA256 hash of your shared secret and the domain you want to establish a link to. Various online tools demonstrate how to make SHA 256 hashes( [see this example](https://codebeautify.org/sha256-hash-generator)). To make it easier, you can do all this in [our own observable notebook](https://observablehq.com/d/21dbe07b6d399868).

3. **Set a DNS record for the domains you want to link back to your main carbon.txt, containing the domain hash**

   You add the generated domain hash as a final, optional part of the DNS TXT record defining the domain you want to link back to your main carbon.txt file, along with the location of the carbon.txt file and the generated hash.

   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, they would create a TXT record that looks something like:

   ```DNS
   TXT "carbon-txt=https://my-org.com/carbon.txt <generated_domain_hash>"
   ```

   **Note:** you can see an example DNS TXT record for the domain [delegating-with-txt-record.carbontxt.org](https://delegating-with-txt-record.carbontxt.org) using online tools like [nslookup.io](https://www.nslookup.io/domains/delegating-with-txt-record.carbontxt.org/dns-records/txt/)

#### How to link two domains using HTTP Via headers domain hashes

HTTP requests and responses can contain a number of extra headers, which you can use to send along extra metadata about the server serving the request.

Carbon.txt supports using the [HTTP Via header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/via), to declare that a HTTP response has been sent along by one or more intermediate entities - in our case usually a managed hosting provider.

1. **Follow the steps above to create your carbon.txt file**

Follow Steps 1 to 3 of the [Getting Started guide](#getting-started) above to create a carbon.txt file for your organisation.

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

### I am self-hosting my website, what can I do?

For now, sit tight. We are currently working with larger organisations to pilot the carbon.txt specification. This will help us uncover use cases and implementation details that will be applicable to smaller providers and website operators.

Expanding the carbon.txt specification to cater for self-hosted sites is on our roadmap. You can follow [this Github issue](https://github.com/thegreenwebfoundation/carbon.txt/issues/9) to keep updated with progress.

### I run a website for my company/myself, what can I do?

For now, sit tight. We are currently working with larger organisations to pilot the carbon.txt specification. This will help us uncover use cases and implementation details that will be applicable to smaller providers and website operators.

Expanding the carbon.txt specification to cater for individual websites is on our roadmap. You can follow [this Github issue](https://github.com/thegreenwebfoundation/carbon.txt/issues/17) to keep updated with progress.

### Why this approach?

There are a few reasons for taking the approach we’ve described above.

Primarily, we are adopting an approach which leans heavily on existing web standards, and technologies. This allows for familiarity, and we hope will lead to these ideas being easier to adopt/implement.

We believe that providing a way for hosting providers/managed services to implement the carbon.txt specification for their product/s is key to broader adoption. It allows us, as a small not-for-profit driving this idea, to have a much larger reach & impact when compared to the alternative of relying on individuals to upload carbon.txt files to their own domains.

In the long run, we think that by demonstrating that you can use existing internet and web standards to link to make sustainability claims easily discoverable, as well as human and machine readable, conventions like carbon.txt will result in a web where it's easier to trust green claims, as well as follow them all the way back to the supporting evidence used to back them up.

### What would stop me using someone else's carbon.txt file instead?

Domains must be associated to an organisation that is a verified green hosting provider with the Green Web Foundation before they show up as being green. This is to mitigate against bad actors taking credit for the green claims made in another organisation's carbon.txt file.

There are two ways to associate a domain with a verified green hosting provider:

**1. Using domain hashes** - New domains are only automatically recognised as green, and associated to a verified green provider, if there is a matching domain hash available when they are first submitted/checked through the Greencheck API.

**2. Manually** - Green Web Foundation staff can manually create a record for a new domain, and associate with an existing verified green hosting provider.

### How does this work for globally distributed providers?

The internet is a distributed network by its very nature. And, many hosting providers - especially CDNs and edge compute services - will serve content from locations around the world. Some of these locations could be in regions that run entirely (or almost entirely) on green energy.

At present, carbon.txt is designed to work at an organisational level, for organisations aiming to demonstrate that 100% of the carbon emissions associated with operating digital infrastructure have beenb accounted for.

We are looking into ways to surface greater geographic resolution for location-based carbon intensity, to reflect the physical reality. If you're interested in collaborating, please [get in touch](mailto:fershad@thegreenwebfoundation.org).
