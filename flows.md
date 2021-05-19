# Main Flows

## SSI-embedded Account Generation (a.k.a. "Sign Up")

1. have developed a Strapi plugin to handle Authentication using SSI which adds additional fields to the Strapi user data model (to store a DID control over which is proven at signup and reproven at each signin).
1. When the user signs up, it receives a temporary link by email where he/she can retrieve a Verifiable Credential signed by SourceCheck. 
2. This credential proves that the publisher has control on the email account provided, and includes both the email and the associated DID (signed by SourceCheck)
3. Proving control of the associated did that the credential was issued to is required by the VP nonce-signing process; see the [VP section of the VC spec](https://www.w3.org/TR/vc-data-model/#presentations-0).

![swimlanes-7d5a21f4f770394eeaa992583bf6cf77](https://user-images.githubusercontent.com/37127325/118764222-01045680-b82e-11eb-9c74-2f7092595af8.png)
Src: https://swimlanes.io/d/PwWrVYjSN

## SSI-embedded Authentication per session

1. To login into the app, the user is asked to present a Verifiable Presentation based on that Verifiable Credential.
1. The connection between the frontend app and the Credible wallet is done using WebSockets (socket.io). A Redis database is used to store ids of sockets and facilitate session management.
1.fter the connection is established, a JWT token is generated in the backend to authorize subsequent requests.

![swimlanes-fbc7b74443ff232b8df283ec083ad9c3](https://user-images.githubusercontent.com/37127325/118764219-ffd32980-b82d-11eb-8671-7dd6757a84e8.png)
Src: https://swimlanes.io/d/Z2Xt071XI

## PDF Hashing and Verification Flows

We handle PDF files to make possible the verification of digital signatures (of SourceCheck and publishers).

The process starts when the publisher uploads a PDF file. The file is processed in the backend and a hash of the content is generated.

This hash, together with the Revenue Sharing infomed by the publisher, is signed by SourceCheck and offered to the publisher.

Finally he/she co-signs the publication, attesting accordance.

The result of this process is a Verifiable Presentation signed by the publisher that is then attached to the PDF file as a jsonld file.

In complement to this, we have developed a service that verifies the PDF file:
https://wms.sourcecheck.org/verify

Basically we are able to extract the Verifiable Presentation (and additional information that future versions of the SourceCheck platform can embed as well in the form of additional or expanded VCs) and deterministically reconstruct the PDF document originally co-signed by the publisher by stripping layers of the PDF/A additive versioning/data model.

The main challenge of implementing the Verification Service was to reconstruct the file with the exact original content hash. Given the binary nature of PDF and absolute precision of hash algorithms, changing a single bit in the file can generate a completely different hash, signalling that the content was tampered with.

We have tested many JavaScript PDF creation and manipulation libraries and none of the libraries were able to simply remove the elements that we were added to the file. But we have found a solution to solve this issue: instead of just trying to remove elements from the signed PDF to return to the original one back, we decided to recreate the PDF starting from a blank file and copying only the relevant elements.
