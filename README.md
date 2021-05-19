# wms-ssi-prototype

Our live demo is up now at [wms.sourcecheck.org](https://wms.sourcecheck.org/).  This work was supported by a Mozilla Grant for the Web in support of the [Web Monetization Standard](https://webmonetization.org/docs/receiving/) working its way through the [W3C](https://w3.org/). We stitched together open-source components (next.JS, Strapi, Spruce Systems' didkit and credible) to create a backend for publishers to embed authenticity provenance, tamper-proofing, and royalty-obligation transparency commitments into PDFs in the form of W3C [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) (another draft standard a little further along in W3C). These PDFs can then be view in a WMS-enabled server-side renderer extracting and verifying.

To understand how this functionality is composed of various open-source components, extensions, and modifications, see our [architecture document](architecture.md).  Also see our [authentication and signing flows](flows.md) document and our [changelog](changelog.md) for the project to round out the documentation.

The components, organized as separate repositories for maximum reusability and forking,  are:
* [Publisher front-end](https://github.com/SourceCheckOrg/wms-ssi-ui/): Publisher interface with SSI-integrated access control and user management
* [Publisher backend API](https://github.com/SourceCheckOrg/wms-ssi-api/): Customized Strapi API to handle provenance, VC, and re-wrapping of tamper-proofed and signed PDFs
* [Publisher backend dB](https://github.com/SourceCheckOrg/wms-ssi-db/): MySQL and Redis dockerized to support Strapi & customizations
* [Reader PDF-reader](https://github.com/SourceCheckOrg/wms-ssi-preview/): Proof of concept of VC-extracting and -verifying PDF server-side renderer with WMS support
 
## Updates

Sign up for updates on our website, [SourceCheck.org](https://sourcecheck.org/).
