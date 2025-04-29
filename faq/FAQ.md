# FAQs

While this concept is still new, we expect that some edge cases will be encountered. Here are some frequently asked questions we have internally.

## I am self-hosting my website, what can I do?

For now, sit tight. We are currently working with larger organisations to pilot the carbon.txt specification. This will help us uncover use cases and implementation details that will be applicable to smaller providers and website operators.

Expanding the carbon.txt specification to cater for self-hosted sites is on our roadmap. You can follow [this Github issue](https://github.com/thegreenwebfoundation/carbon.txt/issues/9) to keep updated with progress.

## I run a website for my company/myself, what can I do?

For now, sit tight. We are currently working with larger organisations to pilot the carbon.txt specification. This will help us uncover use cases and implementation details that will be applicable to smaller providers and website operators.

Expanding the carbon.txt specification to cater for individual websites is on our roadmap. You can follow [this Github issue](https://github.com/thegreenwebfoundation/carbon.txt/issues/17) to keep updated with progress.

## What upstream provider services are accepted?

A list of accepted upstream provider services can be found on [this FAQ page](/faq/FAQ-SERVICES.md).

If there's a particular service you'd like listed, but which isn't on that page then please contribute to [this issue](https://github.com/thegreenwebfoundation/carbon.txt/issues/16).

## What are the accepted `doctype` values that can be used?

Currently, we accept the doctypes below:

- `annual-report`: An organisations annual sustainability reporting.
- `webpage`: A web page outlining steps taken to avoid, reduce, or offset greenhouse gas emissions from service operations.
- `certificate`: Certificates showing the steps taken to purchase green energy, or offset greenhouse gas emissions from service operations.
- `other`: A catch-all for everything else.

## Why this approach?

There are a few reasons for taking the approach we’ve described above.

Primarily, we are adopting an approach which leans heavily on existing web standards, and technologies. This allows for familiarity, and we hope will lead to these ideas being easier to adopt/implement.

We believe that providing a way for hosting providers/managed services to implement the carbon.txt specification for their product/s is key to broader adoption. It allows us, as [a small not-for-profit](https://www.thegreenwebfoundation.org/) driving this idea, to have a much larger reach & impact when compared to the alternative of relying on individuals to upload carbon.txt files to their own domains.

In the long run, we think that demonstrating how we can use existing internet and web standards to make sustainability claims easily discoverable, as well as human and machine readable, will result in a web where it's easier to trust green claims. Not only that, conventions like carbon.txt allows us follow green claims to the supporting evidence used to back them up.

## How does this work for globally distributed providers?

The internet is a distributed network by its very nature. And, many hosting providers - especially CDNs and edge compute services - will serve content from locations around the world. Some of these locations could be in regions that run entirely (or almost entirely) on green energy.

At present, carbon.txt is designed to work at an organisational level, for organisations aiming to demonstrate that 100% of the carbon emissions associated with operating digital infrastructure have been accounted for.

We're looking into ways and funding to surface greater geographic resolution for location-based carbon intensity, to reflect the physical reality. If you're interested in collaborating or funding our work, please [get in touch](mailto:fershad@thegreenwebfoundation.org).
