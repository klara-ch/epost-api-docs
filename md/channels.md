# Channels in detail

Part of the ePost API documentation. Canonical page: https://developer.klara.ch/epost-preview/#channels

## Channels in detail

One delivery call reaches six destinations. Each has its own requirements, and ignoring them is the usual reason a delivery is accepted and then fails.

> **Note: PDF/A, not just PDF**
>
> Documents have to be PDF/A, and PDF/A-3 for eBill. A plain PDF is accepted by the upload and then cannot be processed downstream.

### ePost app and ePost online

The digital letterbox of a recipient who is on ePost. Covered under [identity matching](https://developer.klara.ch/epost-preview/#matching) and [digital letterbox](https://developer.klara.ch/epost-preview/#letterbox).

### eBill

An invoice that lands in the recipient's e-banking through their financial institution. Format: invoice as PDF/A-3, prepared according to the eBill Six specification.

eBill has its own mandatory metadata on top of the normal delivery fields.

| Field | Required | Note |
|---|---|---|
| `email` or `companyUid` or `billRecipientId` | one of them | `email` for consumers, `companyUid` for businesses, `billRecipientId` for either |
| `firstName` and `lastName`, or `companyName`, or `universalName` | one of them |  |
| `ebillAdditionalInfo.paymentAmount` | yes |  |
| `ebillAdditionalInfo.qrReference` | yes |  |
| `ebillAdditionalInfo.dueDate` | no | Left out, ePost sets 30 days |
| `documentTypes` | yes | Must be `invoice`. Any other value and the eBill channel is not considered |

Reminders, credit notes and notifications are possible but not enabled by default. Ask [enterprise support](mailto:support.enterprise@epostservice.ch).

### SMS

A short message to a mobile number. One part carries 160 characters. Above that the message is split, and each part then carries 157 characters because the rest is needed for the join.

| Length | Parts |
|---|---|
| 160 characters | 1 |
| 161 characters | 2 |
| 320 characters | 3 |

Parts are billed individually, so a message of 161 characters costs twice a message of 160.

### Email

Plain email through the platform. Two sender domains are available today, `noreply@klara.ch` and `noreply@epost.ch`. You cannot send from your own domain. The service is being reworked, so check with [enterprise support](mailto:support.enterprise@epostservice.ch) before you build on it.

### Physical letter

Printed and posted through a print partner. Three rules decide whether this works.

> **Warning: The address in the PDF must equal the address in the metadata**
>
> A deviation leads to delivery problems or to wrong franking. The print partner reads the PDF, the platform bills the metadata.

**A4 portrait only.** Every page of the document. A page in another format or in landscape orientation cannot be processed for physical delivery.

**The address has to sit in the envelope window.** Either you move the address block in your templates, or you add a cover sheet, which is the cheaper route if you have many templates.

Two window positions are in use and the address field sits in a different place for each. Both are shown, because mirroring one in your head is a good way to print an unusable batch.

##### Envelope with the window on the left

Diagram: Address position on an A4 page for a C5 or C4 envelope with the window on the left: address field 64 by 30 millimetres, 20 millimetres from the left edge and 52 millimetres from the top edge

*Address field 64 x 30 mm, 20 mm from the left edge and 52 mm from the top edge of the sheet. Values from the Swiss Post address position specification for A4 with a C5 or C4 window envelope.*

##### Envelope with the window on the right

Diagram: Address position on an A4 page for a C5 or C4 envelope with the window on the right: address field 64 by 30 millimetres, 117 millimetres from the left edge and 52 millimetres from the top edge

*Same field, same distance from the top, 117 mm from the left edge. Everything else is identical, including the C5 fold line at 148.5 mm, which does not apply to C4.*

The specification dimensions the address field and nothing else. The franking line and the barcode that a print partner adds are not part of it; agree those with [enterprise support](mailto:support.enterprise@epostservice.ch) if you print yourself.

Onboarding for the physical channel needs a test round together with ePost. Get in touch through [enterprise support](mailto:support.enterprise@epostservice.ch).

#### Post office box addresses

A PO box is addressed through its own fields, not through the street.

| Field | Required | Note |
|---|---|---|
| `firstName` and `lastName`, or `companyName`, or `universalName` | one of them |  |
| `postOfficeBoxNumber` | yes | If you do not know the number, put the words for «PO box» in the local language |
| `postOfficeBoxZip` | yes |  |
| `postOfficeBoxTownName` | yes | Including the delivery office number where there is one, for example `Vevey 1` |
| `street`, `streetNumber`, `city` | no | The domicile address, when you know it as well |

Three worked examples

| Address | Fields |
|---|---|
| Peter Muster P.O. Box 123 6340 Baar | `firstName: Peter` `lastName: Muster` `postOfficeBoxNumber: P.O. Box 123` `postOfficeBoxZip: 6340` `postOfficeBoxTownName: Baar` |
| Peter Muster Neuhofstrasse 3a P.O. Box 6340 Baar | `universalName: Peter Muster` `postOfficeBoxNumber: P.O. Box` `postOfficeBoxZip: 6340` `postOfficeBoxTownName: Baar` `street: Neuhofstrasse` `streetNumber: 3 a` |
| Peter Muster Muehlegasse 15, Bolligen P.O. Box 3000 Berne 8 | `firstName: Peter` `lastName: Muster` `postOfficeBoxNumber: P.O. Box` `postOfficeBoxZip: 3000` `postOfficeBoxTownName: Berne 8` `street: Muehlegasse` `streetNumber: 15` `city: Bolligen` |
