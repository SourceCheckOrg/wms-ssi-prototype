## Introduction 

We are creating a kind of publishing "platform" with WMS and SSI capabilities.
Architecting and planning took considerable research and experimentation, but we
are approaching the minimum viable prototype.


![](https://sourcecheck.org/content/images/size/w600/2021/02/SourceCheck---StakeholderMap-V2.png)

[Src: Original Whimsical](https://whimsical.com/sourcecheck-stakeholdermap-v2-FNM15d5rMR7tARo46F3ASa)

## API & Publisher Back-end 

[REPO](/SourceCheckOrg/wms-ssi-api/)

![](https://community.webmonetization.org/remoteimages/uploads/articles/iql4a8e1lwjdb1svwmjn.png)

[Src: Original Whimsical](https://whimsical.com/sourcecheck-stakeholdermap-v2-FNM15d5rMR7tARo46F3ASa)

To build a full-featured content API would beyond of the scope of this project;
our goal instead was to identify the best open-source components that could be
combined to meet all our functional requirements:

* REST API to be consumed by client app
* Content type customization to support our specific needs
* Content ownership and ACL to allow simultaneous utilization of the app by
  different publishers 
* Extensible through plugin architecture to support authentication using SSI
  (more on that bellow) We chose Strapi as our base for the backend development
  and have been finding it quite straightforward and full-featured.

## UI / Front End app

[REPO](/SourceCheckOrg/wms-ssi-ui/)

![](https://community.webmonetization.org/remoteimages/uploads/articles/nurgiaaadh42bm4rmie4.png)
[Src: Original Whimsical](https://whimsical.com/sourcecheck-stakeholdermap-v2-FNM15d5rMR7tARo46F3ASa)

The front-end component for publishers will be of utmost importance in launching
this project as a hosted platform some day, but for now, in the spirit of the
Grant for the Web grant, we are trying to prototype a minimum viable project,
maximizing for reusable open-source functionality. As such, we are focusing
first on the main goal of our intended future front-end is to allow publishers
to upload their publications as PDF files, define royalty structures for each
piece of content, and digitally sign both in a portable, binding way: verifiable
credentials. These can be downloaded by publishers as receipts, are embedded in
the final PDF publication, and can be consumed and processed by customized PDF
readers. Our front-end currently includes one such custom PDF reader, but our
modular architecture allows upgrading or swapping out any given element.

Our frontend requirements for now are:

* Server Side Rendering (SSR): custom metadata for each page to allow
  content-level web monetization hooks and, some day, content-level royalty
  structures for monetization
* Single Page Application functionality whenever possible
* Convenience of modern web application libraries and frameworks
* Possibility to render PDFs without allowing download and printing, to allow
  for a "WMS Paywall" in-browser without having to establish a direct payment or
  subscription. We have decided to develop the frontend app using Next.js.

As "stretch goals", more likely to be addressed after the grant period, we want
to support multiple payment systems and publication channels for these signed,
authenticated publications. Some day, we want to make these royalty schemes
codified in the publications consumable by payment systems to allow
micropayments and/or automated/smart-contract payments.

## Authentication and Infrastructure for portable signatures

The main feature of our project is to add a provenance layer to the publications
in a both machine-readable and human-readable way; in other words, encoding into
each publication verifiable information about who who created the content and
who published it. This is a deep-rooted problem in all web publication, but it
is also a structural problem particularly felt in the web monetization
community, where fraud is a real concern and disputed originality could even
open the door to liability action for lost income! We also consider our work a
"down payment" on the long-term goal of not just transparent but verifiable
distribution of direct payments between the various parties contributing to
content. While most WM use-cases focus on small-scale productions and individual
creators, we see real potential for bringing WM to non-profit newsrooms,
chapbook presses, academic journals, and many other places where large teams of
creators and collaborators do collective, rather than individual, work. These
contexts require a more complex licensing and payment model than simple direct
payments!

![](https://community.webmonetization.org/remoteimages/uploads/articles/1tqqxo8ao1wooj5rs00l.png)
[Src: Original Whimsical](https://whimsical.com/sourcecheck-stakeholdermap-v2-FNM15d5rMR7tARo46F3ASa)

We have advocated for the maximally standards-driven, open-source implementation
we could find of Self-Sovereign Identity technology as the infrastructure for
handling these identities and high-value signatures. We are using a backend
library called DIDkit and the mobile wallet Credible, both developed by Spruce
Systems (USA).

The user can sign up for an "account" with our app using the identity wallet
rather than a username/password, and they receive a verifiable credential as
"receipt" after confirming her/his email and login to the application using a
so-called "verifiable presentation" (i.e., a proof of possession of unique
credential). In addition to authenticating with this app, content and royalty
structures are both signed with cryptographic material stored securely in the
app (in most cases, in the phone's OS-layer keystore).  We have developed a
Strapi plugin that handles SSI authentication in the backend and integrates with
Strapi's built-in ACL system. This plug-in also renders QR codes in the frontend
to be scanned by the identity wallet. 

The triangular synchronisation between the backend, the frontend, and the wallet
is achieved using [Socket.io](https://socket.io) messages and a custom database
to extend the user account model of [strapi.io](https://strapi.io), which powers
our publisher backend. The custom  dB for our Publisher backend can be found
[here](/SourceCheckOrg/wms-ssi-db/).

## Custom PDF Renderer for WMS

[REPO](/SourceCheckOrg/wms-ssi-preview/)

One crucial aspect of any paywall or web monetization scheme is that content
should not be downloadable (or "sideloadable") in its raw form, because this
makes it trivial and far too tempting not to pay at all. The case of PDFs makes
this quite obvious and salient! For the purposes of demonstration, we have made
a WMS-enabled PDF reader with download functionality disabled by customizing
open-source libraries for server-side rendering of PDFs. This can be configured
to do various kinds of validation, such as only displaying a page if connected
to a WMS account with a positive balance, or checking the authenticity of the
document before displaying it, as determined by the given use-case. 

## Next Steps

Our plan after the grant is to refine this schema in alignment with ongoing
digital content provenance structures such as the Adobe-led [Content
Authenticity Initiative](https://contentauthenticity.org/) and/or the Open
Content Certification Protocol (OCCP)(https://posth.me/occp/). Aligning with
such protocols would greatly extend the reach of our experiments to date,
combining markets, tooling, business models, and large corpuses of authentic
content.