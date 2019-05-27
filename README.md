# carbon.txt

A proposed convention for making it possible to verify that your infrastucture uses green power, by re-using existing governance structures, and already published data.

_Note: this is all a draft version. We are still working out how this might work, so please either leave an issue, or get in touch at hello@thegreenwebfoundation.org, if you would like to contribute._

## Goals

Make it possible to verify claims about carbon neutral energy:

- using as much existing infrastructure as possible (we already have structures and governance for making the internet work)
- open source, so it can be built into modern tooling for continuous delivery , or existing platforms and software
- human readable, as well as machine readable


# The general principle - _walk the graph til you reach carbon_

We know that generating power to run our infrastructure either:

- results in carbon being emitted, which needs to be accounted for with some credit or compensation system, _or_
- if sold, results in carbon credits being _generated_, which are registered with regulators

If we know this, _until we end up at carbon registered somewhere_, we must assume claims are unreliable - and sadly, when we use any service, the carbon is ending up in the atmosphere.

### Making it more concrete

If I have a website here: https://blog.chrisadams.me.uk

Which is hosted here: https://krystal.co.uk/

I know it's _really _running here, because the CEO of Krystal mentioned it in the youtube video on that page: https://www.netwisehosting.co.uk/data-centres/green-data-centres.html

And I know they get their power from here: https://www.ecotricity.co.uk too, as they mention it.

I think there is a way to confirm this independently.

In Europe, renewable power that is generated is registered with this regulator, like this example here in the UK https://www.renewablesandchp.ofgem.gov.uk/

I can look up this up because the regulator publishes it, and because the internet is wonderful, I can make it searchable here, to see that yes, Ecotricity really do generate green power, because I can see the wind turbines the've registered.

https://greenweb-regos.herokuapp.com/regos-a486ca7/CertificatesExternalPublicDataWarehouse-csv?textbox39__contains=Ecotricity

### Walking the graph

If you have a way to identify your upstream provider, and they can update their upstream and so on, there should a way to 'walk the graph', from _all the way to the energy used to power your website_.

While all we're verifying is that a claim is being made, we live in a world where we rely on these same structures to trust that money will go where we want it to, or that a website will point where we think it does.

And in the real world, if we've narrowed down the claim from a cloudy statement like "we're sustainable!" to "we're run your service using this energy from this company", it's much easier to tell when companies are telling porkies, and hoping no one checks.

### What might this look like in practice?


If I host all the bits I need with one provider, like Krystal, might have a `carbon.txt` file at the root of my page, and it might look like this.

_This is all a draft - please, please file an issue to outline what you would need to see to make this something you could support_

```
[upstream]
krystal.co.uk


```

If a hosting company like Krystal didn't run their own datacentres, their carbon.txt file might look like this:

```
[upstream]
www.netwisehosting.co.uk
```

If Netwise ran _all_ their datacentres on renewable power, all they might need to do is list their one provider like so:

```
Ecotricity Group Ltd
```

Under the hood, that would be enough to provide a verifiable check against a [register of credits](https://greenweb-regos.herokuapp.com/regos-a486ca7/CertificatesExternalPublicDataWarehouse-csv?textbox39__contains=Ecotricity+Group+Ltd), because just filtering the register by the supplier, and taking into account the date we're checking might provide enough information - (if we know what the date is when we check, we can implicitly filter out ones that have already expired).

_Anything that can follow links, and check a name against a register can confirm these claims are being made too_

## What if a company isn't running _entirely_ on green power?

It's currently rare for companies _fully_ run all their infrastructure on green power. But maybe it's worth recognising when they do, to give credit when they do.

Let's take our Netwise example. They have a green data centre in London, but they _don't_ make claims about the rest of their datacentres, so we assume the default grey power there.

It seems reasonable to expect Krystal to ask Netwise, and ask which datacentre they use for green power, and them to respond with name for it like 'london'. 

Let's update these.


```
[upstream]
www.netwisehosting.co.uk london
```

And Netwise might have:

```
[london]
Ecotricity Group Ltd
```

This might work at the datacentre level, or the region level, like Amazon - it's deliberately vague about the organisational subgrouping, to stay flexible enough to capture a 'green' claim being made.


## Getting to carbon if you can't change your infrastructure

Not every place in the world has control over who they get power from. And there may be reasons you can't switch to green power immediately. If you can't do that, you might choose to account for the carbon emitted with an offset/compensation scheme.

Luckily, most of these providers either have websites, and issue numbers for the offsets/compensation/credits, or they exist on an exchange too.

```
https://www.carboncompensation.com 432353-234423-2432323
```

Some providers, like Climate Partner have an actual checking system on their site, you maybe you might be able to check at something like

```
https://www.climatepartner.com/credit/432353-234423-2432323

```



## Competing on transparency

You can go further with this.

If rather than just listing a provider, you list how much energy you're using, or how much carbon is offset, and then it becomes possible to verify if the energy providers named in carbon.txt files could _possibly_ have been able to support the claims made further the chain.

Even without it, because just naming a provider is essentially narrowing the search space, even that helps, because if loads of companies are all naming one provider in one place that they get power from, we can _start_ to make assumptions about how plausible claims about using it for green power.

## Why do we need this?

There's almost no transparency around how we source power at the moment. The Green Web Foundation has been building a database, open sources the code for tracking this and is publishing open datasets about the state of the green web, but we need something that can go beyond a single organisation.

So this is the idea. We'd like to work out how to support linking to other supporting documents that companies doing the right thing also produce, like:

- SASB filings, [like ETSY's have done when filing to the SEC - see page 25](https://investors.etsy.com/financials/sec-filings/sec-filings-details/default.aspx?FilingId=13261228)
- [CSR Reports, when they have actual, useful, numbers](https://storage.googleapis.com/gweb-sustainability.appspot.com/pdf/Google_2018-Environmental-Report.pdf)
- Providing useful detailed documentation from [cloud providers about how they account for the carbon they emit](https://storage.googleapis.com/gweb-sustainability.appspot.com/pdf/24x7-carbon-free-energy-data-centers.pdf) whilst making huge hoards of cash
- Or even [publishing responses to NGOs working in this field, like the CDP](https://storage.googleapis.com/gweb-environment.appspot.com/pdf/alphabet-2017-cdp-climate-change-response.pdf)




