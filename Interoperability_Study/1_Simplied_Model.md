# **Simplied Interoperability Model**
# **Version**
**Draft v.0.2 - Erick Domingues**

[Lucid Chart Used for Diagrams](https://lucid.app/lucidchart/97b76c0c-c368-437c-b326-097dbbedf2af/edit?beaconFlowId=E0A20AFD03F0101D&invitationId=inv_e52166c5-c267-432a-bf10-75bdf5208f7b&page=0_0)
# **Table of Contents**
# **Disclaimer**
Before evaluating this document, please take into account the following critical points:

- This document is intended to function as a point of reference for evaluating a simple strategy to facilitate interoperability between the Open Payments Framework (OPF) and Open Insurance Network (OPIN). It should not be interpreted as constituting the final recommendation from Raidiam.
- Raidiam continues to advocate that the path towards achieving interoperability for both ecosystems should incorporate all the insights gathered since the inception in 2021. As such, this step presents a valuable opportunity to implement FAPI 2.0 and OIDC Federation, enhancing standardization and security measures. Moreover, it enables a more comprehensive level of interoperability, extending beyond just OPF - OPIN.
- It is important to note that the primary objective of this document is to provide an analysis of the security standards changes that might be implemented to establish interoperability. Therefore, it should not be viewed as a comprehensive or exhaustive resource.
- To maintain simplicity, this document outlines proposed updates to the Open Finance Standards to accommodate Open Insurance Data Receivers. It is anticipated that corresponding amendments will need to be made to the Open Insurance Standards to facilitate interoperability.
## **Objective**
Both the Open Finance and Open Insurance ecosystems are mandated to devise a technical proposal that would facilitate Open Insurance participants' requests for customer data from Open Finance participants, and vice versa. This requirement is delineated in [RESOLUÇÃO CONJUNTA Nº 5, DE 20 DE MAIO DE 2022](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=5) , published collaboratively by the two ecosystem regulators, SUSEP and Open Finance.

The objective of this paper is to serve as a guiding framework, by proffering a straightforward solution for these ecosystems to achieve interoperability, in alignment with the requirement specified in the aforementioned document:
> I - Propose and implement technical standards and other operational procedures that ensure the interoperability of the systems constituting Open Finance;

To fulfil this objective, this document will critically analyze and propose amendments to the technical aspects of the three primary pieces of standards utilized by both ecosystems: [FAPI-BR](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82083996/EN+Open+Finance+Brasil+Financial-grade+API+Security+Profile+1.0+Implementers+Draft+3), [DCR-BR](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3) and [Brazil Certificate Standards](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82084176/EN+Padr+o+de+Certificados+Open+Finance+Brasil+2.0). The aim is to devise a solution to the challenge highlighted above.
## **Premises**
To ensure that the proposed solution remains as straightforward as possible, a number of stipulations have been defined to limit the scope of this document:

1. Both Directories should persist as distinct, independent entities. Participants from Open Finance and Open Insurance should not be integrated into any of the new directories.
1. No alterations are anticipated in the Consent Journey or any functional APIs within either Ecosystem.
1. Neither of the directories should undergo major modifications.
1. Only small changes are to be expected in the existing security specification. This indicates that participants from the same ecosystems should maintain their current communication practices.
   1. For the avoidance of doubt, this proposition does not include any changes towards Federation and FAPI 2.0. Despite this, we continue to endorse both standards as effective methods of enhancing ecosystem standardization.
# **Problem Statement**
The challenge of interoperability that we aim to address can be encapsulated in the following statement: Establishing the technical means to enable any Data Receivers registered within either ecosystem to access a well-defined set of User Data APIs from the Data Providers registered within either ecosystem.

For a Data Receiver to gain access to User Data APIs in both ecosystems, initiating a Consent Authorization Journey is required. This journey entails obtaining consent from the user who owns the data linked to those APIs, thereby unlocking access to those APIs. The journey commences with the creation of a ConsentID, achieved by invoking the POST Consents API for each ecosystem. Following this, the journey diverges for Data Providers from each ecosystem, each following a path dictated by ecosystem-specific APIs. Consequently, we can postulate that, given the technical means enabling a Data Receiver from either ecosystem to generate a ConsentID within either ecosystem, we should be capable of enabling interoperability.

To acquire a ConsentID for a Data Provider within either ecosystem, a Data Receiver must have a client\_id capable of issuing an access token with sufficient access scope to invoke the POST Consents API for this Data Provider. Therefore, the keystone for interoperability is the provision of the technical means allowing any Data Receiver to generate client\_ids capable of obtaining tokens with adequate access scope from a Data Provider within either ecosystem.

One approach to providing these technical means involves standardizing the entire journey up until Consent Creation, which should be specific to each ecosystem. In technical terms, this can be represented as follows:

This leads to the ultimate problem statement: "Establish a standardized Registration and Token Grant Journey that does not discriminate between participants from either ecosystem."

Considering the above, it is possible to divide the two main blocks of the journey requiring standardization into smaller components executed by each ecosystem's Data Providers, which may differ between Open Finance and Open Insurance:

The aim of this paper, then, is to evaluate and standardize each validation defined in the above diagram in a way that enables Data Providers (OPs) to authenticate Data Receivers (RPs), irrespective of their originating ecosystem.
# **Standardization Proposal** 
Each validation block outlined in the diagram at the conclusion of the Problem Statement will be analyzed in terms of the divergence between the Open Finance and Open Insurance documentation. A proposal will then be put forward on how this validation can be standardized to realize the desired ecosystem interoperability.
## **A1/B1/C1 - mTLS Certificates to be Trusted**
### **Change 1: Consolidate Trusted Certificate Authorities (CAs)**
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82084176/EN+Padr+o+de+Certificados+Open+Finance+Brasil+2.0#5.2.-ICP-Brasil-Certificates> 

Both ecosystems support the same ICP-Brasil, however, they each have distinct onboarding journeys, which can result in different CAs being accepted for each ecosystem.

**Solution**: Align both ecosystems on the same list of trusted CAs and establish a joint CA onboarding process.. 
### **Change 2: Certificate Standards - Different Subject\_DN attribute - *organizationIdentifier***
The sole difference between Open Finance (OPF) and Open Insurance (OPIN) issued certificates as defined on the [Brazil Certificate Standards](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82084176/EN+Padr+o+de+Certificados+Open+Finance+Brasil+2.0) is the attribute organizationIdentifier. This attribute is structured beginning with OFBBR- for certificates issued for OPF, and OPIBR- for certificates issued for Open Insurance.

**Solution**: Explicitly document this distinction in the certificate standards documentation and enforce the requirement that certificates issued by accredited CAs must be accepted, irrespective of the attribute organizationIdentifier.
## **A2 - DCR request validation**
### **Change 3: Modify Mention Around Sandbox Directory**
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.1.-Authorization-server> 

Claim 7.1.1 stipulates:

Dynamic client registration requests not secured by a mutual TLS connection using certificates issued by Brazil ICP (for production) or the Directory of Participants (for sandbox) shall be rejected.

**Solution**: Propose that for Sandbox, CAs from both ecosystems should be acceptable. Furthermore, include in the documentation the list of CAs for both environment
### **Change 4: DCR - Different Subject\_DN attribute - organizationIdentifier**
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.1.-Authorization-server> 

Claim 7.1.14 defines:

- The *organizationIdentifier* field will be found in the subject\_DN in ASN.1 format and must be decoded respecting the corresponding encoding string. The value of the *organizationIdentifier* field of the certificate which must contain the prefix corresponding to the Registration Reference *OFBBR-* followed by the value of the *org\_id* field of the SSA. You must convert the values ​​of the OID 2.5.4.97 field from ASN.1 format to human-readable text. For certificates issued before August 31, 2022: The value of the *OR* field of the certificate must contain the value of the *org\_id* field of the SSA.

**Solution:** Revise the above claim to specify that both OPFBR- and OPIBR- prefixes can be accepted in the subject\_dn.
### **Change 5: SSA Signature Validation - Expand**
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.1.-Authorization-server> 

and 

<https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#9.2.-Open-Finance-Brasil-SSA-Key-Store-and-Issuer-Details> 

Claim 7.1.14 defines:

- shall validate that the request contains software\_statement JWT signed using the PS256 algorithim issued by the Open Finance Brasil directory of participants;

**Solution:** Permit the SSA JWT to be signed by either the Open Finance Brasil directory or the Open Insurance Brasil directory, as indicated by the 'iss' attribute in the SSA. Additionally, the key stores for both participant directories should be published in the documentation.
## **A3 - Scope Accreditation** 
### **Change 6: Consolidate the Role Table Mapping** 
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.2.-Regulatory-Roles-for-OpenID-and-OAuth-2.0-Mappings>

The [Authorisation Server Requirements](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.1.-Authorization-server) on the DCR specification defines the following for the scope accreditation process:

1. shall validate that the requested scopes are adequate for accredited institutions and their regulatory roles and contained in the software\_statement. The list of regulatory permissions and the corresponding scopes are described in the following sections;

The aforementioned clause necessitates an update to the specifications to incorporate a mapping between roles and scopes for the SSA, originating from both the Open Finance (OPF) and Open Insurance (OPIN) Directories.

This is delineated in the 'software\_statement\_roles' claim in the SSA, which follows the subsequent format:

`  `"software\_statement\_roles": [

`    `{

`      `"role": "",

`      `"authorisation\_domain": "",

`      `"status": "Active"

`    `} ]

**Solution:** The table featured in section 7.2 should be updated to include an additional column titled "Authorisation Domain". This modification would enable the mapping of each ecosystem's Regulatory Role to the scopes that should be assigned based on each individual ecosystem.

Furthermore, a clause could be established in the section that ensures the 'authorisation\_domain' is always associated with a specific Ecosystem, as determined by the 'iss' claim in the SSA.

A sample representation of this table would be as follows:

|**Authorisation Domain**|**Regulatory Role**|**Description**|**Allowed Scopes**|**Target Phase**|
| :-: | :-: | :-: | :-: | :-: |
|Open Insurance|DADOS|Instituição transmissora ou receptora de dados (AISP) from Open Insurance|openid consents ?|Phase 2|
|Open Finance|DADOS|Instituição transmissora ou receptora de dados (AISP)|openid accounts credit-cards-accounts consents customers invoice-financings financings loans unarranged-accounts-overdraft resources|Phase 2|
## **B2 - Subject\_DN Validation:**
Apart from the modifications outlined in Change 2 and Change 4, pertaining to the 'organizationIdentifier' field, there are no further changes required in the documentation to ensure that a server from either ecosystem can apply the validations detailed in <https://datatracker.ietf.org/doc/html/rfc8705>. 
## **C2 - Token Request Assertion Validation:**
Referenced on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#7.1.-Authorization-server> 

The assertion is verified using the keys specified in the 'jwks\_uri' included in the DCR. The only constraint associated with this field is as follows:

- shall, when informed, validate that jwks\_uri matches the software\_jwks\_uri provided in the software\_statement

This suggests that, assuming Change 5 is implemented—which facilitates the use of SSAs from both directories—there should be no restrictions on the JWKS utilized to validate the Token Request Assertion.
## **Additional Changes** 
The change mentioned below is related to payload signing and, as this is not a requirement for Customer Data 
### **Additional Change 1: Explicit Requirement to use JWKS for a specific directory**
Defined on: <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82083996/EN+Open+Finance+Brasil+Financial-grade+API+Security+Profile+1.0+Implementers+Draft+3#6.1.-Message-Content-Signing-Considerations-(JWS)> 

- The receiver shall validate the consistency of the JWS message's digital signature **exclusively based on the information obtained from the directory**, that is, based on the keys published in the institution's JWKS in the directory.
- **iss** (in the *JWT* request and in the *JWT* response): the receiver of the message shall validate if the value of the **iss** field matches the organisationId of the sender

and on <https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/82051199/EN+Open+Finance+Brasil+Financial-grade+API+Dynamic+Client+Registration+1.0+Implementers+Draft+3#9.4.-Signature-certificates-validation> 

- Make sure that the kid present on the message JWT header is present on the directory JWKS.

**Solution:** The specification should be revised to affirm that the JWKS used for validation is the one included in the SSA that is utilized in the DCR, irrespective of the directory it originates from. Moreover, it should re-emphasize that the 'organisationId' should also be the one transmitted in the SSA.
# **Conclusion**
The proposed pathway for achieving interoperability between the Open Finance and Open Insurance Customer Data APIs necessitates only six minor adjustments to the existing security standard. Importantly, these proposed modifications do not entail any substantial changes to the current architecture or functionality of the individual directories, thus preserving their unique integrity within their respective ecosystems.

As a subsequent course of action, institutions must assess the potential impact these updates may have on their systems. While these amendments are deliberately designed to be minimal, it is essential to ensure a smooth transition. Additionally, we reinforce our premise that Open Finance and Open Insurance ecosystems should seize this transition period as an opportunity to adopt FAPI 2.0 and OIDC Federation, leveraging the lessons learned since the inception of these ecosystems in 2021. Such an initiative would not only enhance standardization and security but also open the door to broader interoperability beyond just Open Finance and Open Insurance, hence, marking a significant stride towards a more integrated and secure future in the realm of Open Data in Brazil
