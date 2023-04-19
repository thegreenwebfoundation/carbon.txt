# carbon.txt

## What is carbon.txt?

A proposed convention for website owners and digital service providers to demonstrate that their digital infrastructure runs on green power. This is achieved by reusing existing governance structures, already published data, and existing industry standards.

_Note: this is a draft version and we are still ironing out how this might work. If you would like to contribute or have questions, please [open a GitHub issue](https://github.com/thegreenwebfoundation/carbon.txt/issues) or email [fershad@thegreenwebfoundation.org](mailto:fershad@thegreenwebfoundation.org).

## What are the goals?

Having a convention around how website owners and service providers disclose this kind of information allows for two things:

1. It brings this information into the public domain in a way that is consistent, and easily accessible.
2. It lays the groundwork for a future where checking if a website/service uses green energy is democratised, and not locked to a single database or organisation.

In order to achieve this, we believe that any proposed convention, such as carbon.txt, should:

- use as much existing infrastructure as possible, especially as we already have structures and governance for making the internet work;
- be open source, so it can easily be built into modern tooling for continuous delivery and existing platforms/software;
- be human readable, as well as machine readable.

## Why do we need this?

There's almost no transparency around how we source power at the moment. Green Web Foundation has been building a [Green Domains database](https://www.thegreenwebfoundation.org/green-web-datasets/), open sources [the code for tracking this](https://github.com/thegreenwebfoundation/admin-portal) and is publishing [open datasets about the state of the green web](https://datasets.thegreenwebfoundation.org/).

But we need something more. Something that can endure beyond a single organisation. **We need a convention like carbon.txt**.

## What are the benefits?

The carbon.txt convention has two main user groups:

- Digital service providers (CDNs, hosting providers, managed services)
- Individual website owners

### For digital service providers

Providers are able to:

- Broadcast that the infrastructure they manage or use to provide their service runs on green power, in an open, transparent and publicly accessible way.
- Acknowledge and justify any green or carbon neutral credentials they are claiming. 
- Identify themselves as an upstream provider in digital supply chains through [Green Web Checks](https://www.thegreenwebfoundation.org/green-web-check/). Take the example of _The Really Green CDN Company_ who offers a CDN service that runs on Google’s infrastructure. Currently, when a check is run against a site hosted on _The Really Green CDN Company’s_ network, the results returned would say the site is hosted by Google. After _The Really Green CDN Company_ implement carbon.txt on their services, results from a GreenCheck would return showing the site is hosted by _The Really Green CDN Company_ as well as any evidence linking to their use of Google’s services.
- “Pass on” their green credentials to websites or other downsteam services that use them.

### For individual website owners

Website owners are able to:

- Create their own carbon.txt file which can be used to indicate which provider they are using. This is helpful if they do not want to wait for their upstream provider/s to make their own DNS or HTTP Header changes. 
- Share their own, more detailed information. For example, they might be using multiple providers for different parts of their website. They may have a web host, another provider for images, another for advertising, another for video etc. Such information could then be used as a reference by other projects, like the [HTTP Archive’s Web Almanac](https://almanac.httparchive.org/en/2022/), or by other services that aim to capture the overall "greenness" of the web.

## How it works

When checking a website, the following checks are performed:

1. Check the domain name is a valid one.
2. Check there if there is `carbon-txt` DNS TXT record for the given domain.
3. Perform an HTTP request at [https://domain.com/carbon.txt](https://domain.com/carbon.txt), OR the override URL given as the value in the DNS TXT lookup.
4. If there is valid 200 response and a parseable file, parse the file.
5. If there is a no valid 200/OK response at domain.com/carbon.txt (i.e. a 404, or 403), check the HTTP for a `Via` header with a new domain, as a new domain to check.
6. Repeat steps 1 through 5 until we end up with a 200 response with a parsable carbon.txt payload, or bad request (i.e. 40x, 50x).

Once there is a parseable carbon.txt file, the domain the carbon.txt belongs to is used as a lookup key against a known list of domains associated with a provider. If there is a match, the site is assumed to be running at the provider, and shows as green.

At this stage, the Green Web Dataset will still be the source of truth for verifying green services. This means that providers will still be required to register with The Green Web Foundation, and provide evidence of their green claims.

In the long run, we want to be able to use publicly available data to link green energy usage or procurement. Some example of data sources we might explore are:

- Data provided by the [EnergyTag API](https://energytag.org/fea/)
- Open data provided by government, such as this dataset of [Renewable Energy Credit (REC) purchases in Taiwan](https://www.trec.org.tw/en/transaction_history_direct?year=2023).

## Getting started

### For Digital Service Providers & Self-hosted Websites

As a digital service provider, you should first [register yourself with The Green Web Foundation](https://www.thegreenwebfoundation.org/green-web-check/register/), and provide evidence of your green claims.

1. **Register with The Green Web Foundation** \
   As a digital service provider, you should first [register yourself with The Green Web Foundation](https://www.thegreenwebfoundation.org/green-web-check/register/), and provide evidence of your green claims.
2. **Create a carbon.txt file for your organisation** \
   Create a carbon.txt file for your organisation. There is a guide to the expected syntax below. We recommend that you host this file either at the root of your domain, or in a `.well-known` folder. \
   For example: _<https://www.my-org.com/carbon.txt>_ or \
   _<https://www.my-org.com/.well-known/carbon.txt>_

#### Carbon.txt file syntax for digital service providers

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

#### For digital service providers with multiple domains

It is to be expected that one digital service provider might own and provide services through multiple domains. In this case, you can maintain a single carbon.txt at one domain (e.g. _<https://my-org.com/.well-known/carbon.txt>_) and refer other domains to that single source of truth. To do this:

1. Complete the steps above to register as a provider, and create a carbon.txt file.
2. **Generate a domain hash for your account** \
   To do this, use the Observable Notebook linked below: \
   [https://observablehq.com/d/17c922a33a5d6867](https://observablehq.com/d/17c922a33a5d6867) \
   (Note: We understand this is solution is not ideal & are working on a nicer way to generate secrets)
3. **Set DNS records for the domains you want to link back to your main carbon.txt** \
   You can now create DNS TXT records for all the domains you want to link back to your main carbon.txt file. \
   For example: _my-org.com_ also owns _me.my-org.com_. In order to link _me.my-org.com_ to the main carbon.txt file, they would create a TXT record that looks something like:

   ```DNS
   TXT me.my-org.com carbon-txt=<https://my-org.com/.well-known/carbon.txt> generated_domain_hash
   ```

#### For digital service providers who want to pass on their green claims to users of their service

Providers may want to broadcast that all the websites using their service are also running on green energy. To do this, they can use the [HTTP Via header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/via).

1. Complete the steps above to register as a provider, and create a carbon.txt file.
2. **Generate a domain hash for your account** \
   To do this, use the Observable Notebook linked below: \
   [https://observablehq.com/d/17c922a33a5d6867](https://observablehq.com/d/17c922a33a5d6867) \
   (Note: We understand this is solution is not ideal & are working on a nicer way to generate secrets)
3. **Set via headers for on requests generated/processed by your service** \
   You can now set via headers for requests that go through your service. This allows you to broadcast that the request has come from your service, and point to your carbon.txt file. A valid via header would look like:

   ```HTTP
    Via: 1.1 <https://my-org.com/.well-known/carbon.txt> generated_domain_hash
   ```

### For Individual Website Owners

For individual website owners, you can set your own carbon.txt in order to provide supplementary information to what may be provided by any services you use. It also provides an avenue through which organisations can be very explicit about putting sustainability information into the public domain.

An example of a carbon.txt file for an individual website would look like:

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
    { domain = "fershad.com", doctype = "webpage", url = "https://fershad.com/for-good/"}
]
```

_An example of a carbon.txt file that highlights the different providers used by a website._

## Why this approach?

There are a few reasons for taking the approach we’ve described above.

Primarily, we are adopting an approach which leans heavily on existing web standards, and technologies. This allows for familiarity, and we hope will lead to these ideas being easier to adopt/implement.

We believe that providing a way for hosting providers/managed services to implement the carbon.txt specification for their product/s is key to broader adoption. It allows us, as a small not-for-profit driving this idea, to have a much larger reach & impact when compared to the alternative of relying on individuals to upload carbon.txt files to their own domains.

## FAQs

While this concept is still new, we expect that some edge cases will be encountered. Here are some frequently asked questions we have internally.

### I am self-hosting my website

Self-hosting a website requires some degree of technical knowledge to setup and configure one/multiple servers to handle requests. Because of that, we assume that anyone who is self-hosting would be technically capable of setting up the require DNS/HTTP Header configurations of their site themselves.

In order for a self-hosted site to be considered green, we would also require the owner to register their site and their green claims with The Green Web Foundation. This would allow for the site to be added to the Green Web Dataset.

### How can you trust this? What would stop me lying in my carbon.txt file about my site?

You could indeed list a green provider in your supply chain, and claim it was hosting your site, so your site showed up as green through association. Your site would not show up as green until you had been able to submit some supporting evidence for manual review that you _really were_ using that provider.

After manual review by our support staff, you would have managed to mark one domain as green.

### What would stop me using someone else's carbon.txt file instead?

There are two mechanisms designed to mitigate against lying.

**1. Manual review for new domains** - just like with your own domain, we have a manual review step for any new domain delegating to one already trusted. New domains don't show as green until they have been added to an allow list for a given provider, or until a domain hash is made available when performing a lookup. More on domain hashes below.

**2. Domain hashes for newly seen domains** -

Domain hashes are SHA256 hashes based on:

1. the domain a lookup is being delegated to
2. a secret shared that only the green web platform and the provider associated with the domain above has access to

They are an optional part of either a TXT record or HTTP header, when delegating a lookup to carbon.txt file at a different domain.

### How do you handle globally distributed providers?

This is a particularly tough question, but one that needs to be considered carefully. The internet is a distributed network by its very nature. And, many hosting providers - especially CDNs and edge compute services - will serve content from locations around the world. Some of these locations could be in regions that run entirely (or almost entirely) on green energy.

Take the example of a small-scale datacentre operator with two locations - one in Iceland (100% renewable energy) and one in Germany (~50% renewable energy). One approach that could be taken here is that a hosting provider who uses this datacentre operator would add HTTP Headers _only_ to those requests which are served from the Iceland datacentre.

We don’t yet have a genuinely good answer for how to handle this kind of situation, and are open to suggestions.
