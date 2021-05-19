# Release v0.1 

## Capabilities
* Sign-up - provide email and handle (only used internally) to generate publisher account
* Confirm email - sent a unique URL to a credential-offer QR specific to that account - email confirmed and DID controlled by receiving wallet associated to account
* Sign-in - nonce-signing Verifiable Presentation of VC from email confirmation 
* Account recovery - original email URL can be used to re-issue proof-of-email-control VC again if wallet needs to be recovered from seed phrase (see Credible docs)
* Publisher profile - self-attested, not published, internal use only for now 
* Creating arbitrary royalty structures (i.e. verifiable statement of revenue-sharing obligations encoded in authenticity mechanism of published PDFs)
* Verifiable authenticity record embedded in each publication:
   - includes DID associated with account (control of which is freshly re-proved by each sign-in event)
   - includes WMS payment endpoint for server-side renderers that support WMS
   - includes signature of publication by SourceCheck (did:web:sourcecheck.org as issuer)
* Server-side renderer with WMS enabled to allow "pay by the minute" previews of paywalled content and/or statically-hosted content
   - Renderer could also verify and/or validate authenticity record, extract information about it for revenue sharing, etc. (choose your own adventure)

# Possible improvements / Feature Wishlist

Backend / API
* Setup permissions automatically on first startup
* Extract SourceCheck plugin in a separate Strapi / npm module
* Handle unpublishing of publication
* Handle inclusion of page(s) on signed PDF file (Revenue sharing data / provenance data / QR code for honorbox)
* Configure mail sender to use custom SMPT server
* Implement did-web
* Enhance VC/VP schema

Front end / UI
* Edit Publication page: automatically suggest slug for publication
* Edit Publication page: handle changes in the slug for publication
* Edit Publication page: allow unpublishing publication
* Creation of a dashboard with number of publications and number (and total time) of visualizations

Preview App
* Improve general layout
* Implement optional blocking of visualization in case Coil extension is not active
* Render Provenance data
* Render Revenue Sharing data (as signed in Royalty Structure)
* Allow custom layout by publishers

Authentication:
- Refresh of authentication tokens
