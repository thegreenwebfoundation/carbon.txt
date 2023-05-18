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

We need a fossil free internet by 2030. But right now there's still not much transparency around how we source power.

To address this, Green Web Foundation has been building a [Green Domains database](https://www.thegreenwebfoundation.org/green-web-datasets/), open sources [the code for tracking this](https://github.com/thegreenwebfoundation/admin-portal) and is publishing [open datasets about the state of the green web](https://datasets.thegreenwebfoundation.org/). Creating this dataset has been an ongoing effort for years. It has involved associating IP address space like IP ranges or Autonomous System Numbers with specific organisations, then manually verifying written reports and certificates provided by said organisations to back up their green claims.

This has worked for the first few million domains, and first few billion checks against the database. But the internet is a much bigger, more dynamic place than it was when this endeavour was started. Our approach needs to evolve with the times.

Luckily, today there are more tools that can help. Tools like the Domain Name System (DNS). DNS provides a variety of ways to establish a link between a domain and the party that is hosting or operating it. There are also dedicated registries that make it possible to trust that electricity has been generated from clean and renewable energy sources. And, if you know where to look, it's possible to tell when efforts to decarbonise energy are credible.

Carbon.txt serves to make links between these systems easier to follow. So when there is a claim that a website is running on green energy, you can trace the provenance of supporting evidence more easily.

## What are the benefits?

### For digital service providers

If you provide hosted digital services to others, carbon.txt lets you:

- Receive recognition, in a human and machine readable way, that the infrastructure you manage or use to provide your service runs on green energy.
- Earn trust from customers by helping create an evidence base of action being taken by providers to help the world transition to a fossil free internet.
- Allow any downstream services or websites using your services to make the same claims, with a clear chain of attribution.
- Demonstrate leadership if you are moving faster in terms of a climate response than the organisations in your supply chain by linking to your own work.
- When customers check your service using the [Green Web Check](https://www.thegreenwebfoundation.org/green-web-check/), your organisation name will be shown rather than any CDNs or infrastructure providers you use.

## Getting started

**Note: The carbon.txt specification is currently being trialled with digital service providers only. If you self-host, or have your own website, please see the [FAQ](/FAQ.md) for more information.**

### For Digital Service Providers

1. **Register with Green Web Foundation**

   As a digital service provider, you should first [register with The Green Web Foundation](https://www.thegreenwebfoundation.org/green-web-check/register/), and provide evidence of your green claims.

2. **Create a carbon.txt file for your organisation**

   Create a carbon.txt file for your organisation. There is [a guide](#carbontxt-file-syntax-for-digital-service-providers) to the expected syntax below.

   **Note: You only need one carbon.txt file for your organisation.**

   To link multiple domains to a single organisation, refer to the [linking green claims to multiple domains](#linking-green-claims-to-multiple-domains) section below.

3. **Upload your carbon.txt file to your servers**

   For example: _<https://www.my-org.com/carbon.txt>_

   We default to checking for a file located at the root of your domain `/carbon.txt` or `/.well-known/carbon.txt`.

4. **Share the URL of the carbon.txt file with Green Web Foundation**

   The Green Web Foundation has an API for registering where to check for carbon.txt file for a given domain. Once this is listed, and the link established, your site shows as green.

   ```curl
   curl --request POST --location 'https://api.thegreenwebfoundation.org/api/v3/carbontxt' \
   --header 'Content-Type: application/json' \
   --data '{ "url": "https://my-org.com/carbon.txt" }'
   ```

5. **(Optional) Link other domains to your green claims if they are using infrastructure you control**

   If you offer managed hosted services to other organisations, once your first link is established there is an automated process for listing future domains so they show up as green too, with attribution to you. See [_domain hashes_](#linking-green-claims-to-multiple-domains) below for more.

### Carbon.txt file syntax for digital service providers

The carbon.txt syntax is written in TOML. An example of what a carbon.txt file might look like for a digital service provider.

```toml
[upstream]
providers = [
    # An array of providers my-org.com is using to deliver our service
    { domain='cloud.google.com', service = 'shared-hosting' },
    { domain='aws.amazon.com', service = 'cdn' }
]

[org]
credentials = [
    # Optional.
    # An array of documents that point to evidence of green claims made by my-org.com.
    { domain='my-org.com', doctype = 'sustainability-page', url = 'https://my-org.com/our-climate-record'}
]
```

_An example of a carbon.txt file for a digital service provider._

## Linking green claims to multiple domains

It's fairly common that a digital service provider might own and provide services through multiple domains. They might also have multiple products, or provide hosted services for a number of users all who have their own domains.

In this case, you can maintain a single carbon.txt file at one domain (e.g. _<https://my-org.com/carbon.txt>_) and refer other domains to that single source of truth.

There are two supported ways to do this:

1. Using DNS TXT records, which contain the specific URL pointing to the carbon.txt file to read.
2. Using a dedicated HTTP Via Header, again containing a URL for the carbon.txt file to read.

### Choosing the right option for your organisation

#### Using DNS TXT records

Using DNS TXT records is intended for organisations who are able to add DNS records to both their own domain, as well as the domain they want to show up as green. If you own both domains, this option is for you.

**[Guide: How to link multiple domains using DNS TXT records](/DNS-TXT.md)**

#### Using HTTP Via Header

Using a HTTP Via Header is intended for organisations who are not able to add DNS records for the domain they want to show up as green, but do accept HTTP requests for the domain, and serve responses for it. If you operate a CDN, a managed Wordpress service, or a general Platform-As-A-Service (PaaS) offering, this is usually better suited for your use case.

This option also allows you to "pass down" green claims to customer who use your service.

**[Guide: How to link multiple domains using HTTP VIA HEADER](/VIA_HEADER.md)**

## Unsure about something or have more questions?

Read the [FAQ](/FAQ.md), or [raise an issue](/issues).
