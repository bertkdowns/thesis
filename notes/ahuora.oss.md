---
id: wz9jb5o3tztmz3br577dy43
title: Oss
desc: ''
updated: 1782163710120
created: 1779148327322
---

# Where does the business model of ahuora come from?

Potential options:

- Consulting with companies (Using the platform internally to help make our consulting better) to decarbonise/increase efficiency.
- Working with suppliers to make their models accessible in the platform (advertising/trust in their brand and expected performance)
- Selling/licensing the software to other companies
- Selling/licensing real-time systems or integrations to other companies (e.g real-time data access, additional models/capabilities)
- Hosting the platform as a SaaS


Considering these options:

Consulting with companies is the most achievable and realistic. We could do that pretty soon.

Working with suppliers would only happen (and only be profitable) if lots of people are using the platform already, otherwise they wouldn't spend money on it.

Selling/licensing is an option, but it is hard to figure out good terms. One company may get millions of dollars of value out of it by optimising their process. Another company may operate on a much smaller scale and only get 50 thousand dollars worth of value from it. Either way, the value depends on how well they can use the software and what gains are possible, so it feels like a gamble for them. It's very hard to point to a number that the platform is "worth" for a license. Besides, if you only need it for design time, what's stopping people from getting a 3 month access and then stopping using it?

Selling/licensing real-time systems or integrations becomes a bit easier, as that's an ongoing thing and so you can be contracted by the company to maintain a digital twin of their site and all the real-time automation stuff that goes with it. Some sort of subscription model may make sense here.

Hosting the platform as a SaaS links to some of the others. What if people only need access for a few months?

# Open source licenses:

## GNU Affero General Public License (AGPL)

Can be used commercially or noncomercially. All derivatives must share their code back, including if they only use their code internally.

### How might it limit our business?

Others could use it without paying us as it is open source.

### How might it give new opportunities?

Any derivative works on it must come back to us. This means we have more visibility into what people are adding and using it for.

We could license out under more permissive terms to those who don't want to make their changes publiclly accessible (for a royalty.)


## Mozilla Public License 2.0 (MPL 2.0)

Modified files must stay open source.

This lets companies use it commercially a bit easier than the AGPL, as they are allowed to have some "industry secrets". So it might help with adoption.

### How might it limit our business?

Others could use it "for free" including other consultant companies. Others could make business models based on it relatively easily. 


### How might it give new opportunities?

This would encourage more people to use it, and their contributions would come back and improve the core platform. If we were using it for consulting, it would give us many more tools to do better work.

We would still be the experts in it and that would still give us a pretty big competitive advantage.

Those who have custom propietary technology can keep their models private easier, increasing adoption. 

With more people using it, other companies would want to model their equipment in the platform. We could include their equipment in standard libraries or the standard online platform for a fee.


## Business Source License (BSL)

The source code is avaliable, but any commercial competitors or uses require a license. This gives us a lot more control over how other people use it, but it also means people are going to be much less willing to contribute to it. It drastically limits what they can do with their contributions. It feels less like "contributing to an open source community" and more like "giving free labour to another company".



## Contributor License Agreement

We should probably also get those who contribute to sign a CLA in case we want to relicense in the future. Otherwise we will get stuck in whatever license we release under.

## Moving licenses

It is easy to go BSL -> MPL -> AGPL, but hard to go the other way around. 

BSL is "you can see it, but you can only use it under our terms".

MPL is "you can use it for commercial or non-commercial purposes"

AGPL is "you can use it for whatever, but must contribute back anything you use it for".

# My personal opinion

I think the ahuora platform alone is very hard to monetise. I think we have a much better chance making money from consulting, and from the ecosystem around the ahuora platform.

While there is a danger that other consultants could compete with us by using our own software, I think that the worldwide market is big enough that most of them would be working in different markets that we don't service anyway. Their improvements to the system however, could give us the capability for new types of modelling and types of processes that we do not currently have, allowing us to expand the market we can reach. It would also give us a big reputation boost if other consulting companies were relying on our software, which would win us more business on its own.

I think our real value comes in the ecosystem, and being the person that controls the core of the ecosystem is very powerful. It's hard to get others to invest in an ecosystem without a relatively open license so they trust that they will be able to use the software for years to come. While others could use the software freely to create models, we would provide them with (all at a cost):

- Tools and hosting solutions for digital twin infrastructure, and model versioning and sharing,
- model libraries from other companies (either the suppliers pay us to include their equipment, or others purchase our pre-built models)
- closed sourced licenses if they have commercially sensitive changes that they cannot share,
- a cloud platform to use the tool in without having to maintain their own infrastructure,
- integrations with other third party tools and platforms
- additional features locked behind a paywall (e.g automatic HEN design from a flowsheet)
- support contracts and custom solution services,

all while maintaining our consulting businesses on top of that. Just because the platform is open source does not mean everything else we do is open source.

I personally think we either start with MPL and then transition to AGPL when we have adoption, or start with AGPL immediately. The best chance we have for survival long term is to become something that people rely on.


# Proposal


Ahuora Unit Operations & Property Packages: MPL/MIT. Allows others to use/modify our unit operations. Companies can make their own unit models and use them with the platform, and do what they want with them - including they could sell their custom unit model libraries. (We expect companies making their own unit ops will be pretty common and allow most companies to us the software for free.)

Main Ahuora Platform - AGPL3. If companies make changes, they need to make them public. We require contributors to our main repository to sign a CLA. If companies want to make a modified version that they keep closed source, they need to get a license with us.

## Example use cases:

A salt factory uses our platform and existing models, they self host in AWS. They can do so without any legal issues as they are not modifying it.

A pharmaceutical company uses our platform and creates new/modified unit operations and property packages for their proprietary compounds. They can do so perfectly fine as the operations and property packages are licensed under MIT, without any involvement from us. 

The pharmaceutical company later wants to make a custom GUI to analyse medical properties. They contact us to recieve a closed-source license.

A University Researcher creates an Exergy Analysis GUI module. They contribute it back and sign the CLA. It is then part of our main ahuora repository.

Another research institution creates an Energy Modelling GUI module. They comply with the terms of the AGPL3 and publish it, but they do not sign the CLA to contribute it back to our main repository. We cannot relicence it, and companies would have to seek permission from them (as well as us) to receive a closed source licence to their Energy Modelling version.



# Related Content

[Pyomo: Accidentally Outrunning the Bear](https://www.sciencedirect.com/science/article/pii/S266638992500159X)