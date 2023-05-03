## Abstract

Users are issued verifiable credential by identity providers like idemix, U-prove, polygonID issuers. This verifiable credential is issued to the user such that the issuer itself does not know what value the user has asked to be included in the credential. This is called blind issuance. The server can only know that the value satisfies certain predicate stated by the user with zero knowledge proofs. As such the user is able to embed his private details on a credential, which he can present to verifiers later. Note that this method of specifying one's own attribute values differs from the typical credential issuance process where an authorized entity issues these values to the user. Though our form of verifiable credential also supports such central authority specified attributes, we think user specified values have potential as many prominent technology do indeed use user specified values for user profile information. So it's a tried and tested convenient and scalable model of issuing softer versions of verifiable credential.

So far user has in possession a verifiable credential with private attributes. This can now be used in three different forms. First, a user can selectively disclose some attribute of his credential. Second, a user can prove a statement about the attribute values in zero knowledge proof instead of directly revealing the attribute. Third, a set of users can provide attribute values to aggregator servers such that no single server can understand the user's private data yet the aggregator servers working together can compute some function on the total data provided by all users. For example, it will be possible to find average (or standard deviation) salary of people within a certain age range without ever knowing how much each individual earns. Note that with second and third approach, data requesters have little to no information about user's actual private data but have a way to extract the information they need. Paired with how the verifiable credential is issued as mentioned above, a user's private information has been used to calculate results yet the actual values have remained hidden throughout the pipeline.

Data requesters can request data that satisfy some predicate present in a verifiable credential. In return, the data providers (users) can be rewarded for the participation. A user can create multiple different types of credentials which he can use in different ways as mentioned above. This work is different from existing ones for combining verifiable credential and private aggregation. So far works on these subjects have existed on their respective silos. Additionally, the proposition to specify one's attributes oneself to get issued a less strict version of credential is, to our knowledge, new more practical and scalable take on credential issuance.


## 1. Introduction

### 1.1  State of User Privacy

User information like credentials, preferences and other details shared online have shaped prominent techs like social media, advertisement and marketing tools, recommendation systems, etc. Because the same information of a user is valuable to a large number of use cases, it's no surprise that this value has given rise to a market where personal data is collectively aggregated, refined and sold. For example, the use of third party cookies to collect and share user data across sites, use of targeted ads to shape user behavior and shady ways to uniquely identify (fingerprint) a user despite not sharing any personal details deliberately have led to a much reduced user trust in these systems. Because of how deeply ingrained these methods are, getting rid of them would lead to an incovenient user experience or the need for a different economic model altogether. Fortunately, recent advances in cryptography and increasing demands from the general public have made it possible to design and implement systems focused on maintaining user privacy. Few of such systems are as given below. 

- browser telemetry systems collect and operate on aggregated user data to extract relevant metrics all the while ensuring user privacy, 
- ENPA notifies user of potential exposure to COVID without accessing individual user details,
- Google Privacy Sandbox is working on technologies like Topics API, Fledge, Reporting API which will replace use of third party cookies,
- advances in zero knowledge proofs make it possible to prove a statement while hiding private details, 
- multi-party computation ensures no single data handling server has autonomous access to user data and so on. 

It is safe to assume that these privacy friendly techs will find more applications in near future, starting with those where the demand for privacy is relatively higher.


### 1.2 Representing User Info

Different people have differing notions of privacy; what one considers sensitive info could be something publicly shared by the other. As such, it's best to let the user decide what info he is willing to share. However, it gets really inconvenient really quick if a user has to specify his choices repeatedly. For example, GDPR enforced cookie settings and how inconvenient it is to manage these preferences on every site.

As a remedy, there can be tools to help automate incovenient interactions. In the above cookie settings example, a user could specify his preference once and a browser extension could have probably handled most of these settings on his behalf. A different example is the use of CAPTCHA to solve puzzles, which have been getting increasingly difficult. Instead of having to squint one's eyes and identify pictures of a laughing zebra, one could submit some other user metric that captures user's interactions on the internet and uses it to prove humanness. This is exactly what's done by Privacy Pass, which uses user's reputation score calculated by cloudflare to get past CAPTCHAs. 

Secondly, not every user info has to be treated the same; some need more protective measures than others.

### 1.3 Collection

How do you fill a form such that only you know the filled values, yet the surveyor can rest assured that certain constraints ( like values being of specific data types or range) were satisfied ? With Zero Knowledge Proof, we can prove that the submitted values satisfy the supplied constraints and are also the same values whose encrypted form has been submitted. This method ensures that your private information is not leaked when you're not doing anything valuable using it. 

### 1.4 Processing

Depending upon what is requested, a user's data can be presented in various ways. For example, requester may ask to be disclosed a set of attribute values either in clear text or in an encrypted form, requester may ask the user to prove a statement on his data using zero knowledge proof instead of revealing directly, requester may ask for only the result of an aggregation function applied to data from multiple users without having to know individual values. 

### 1.5 Sharing Incentives

Rewarding users for sharing information can encourage privacy friendly approach where user would want to retain full custody of their data and use it wisely. 

## 2 Related Work

Private Aggregation Schemes like Prio enable computation of aggregate statistic such that individual values are not revealed to any parties, not even the servers that perform the aggregation. This technology has been used by Mozilla Firefox Browser, ENPA, DivviUP
To ensure these user submitted values are reliable, some authority issues verifiable credential to these users. This verifiable credential includes in them the attribute values whose correctness is verified later during credential presentation. 

## 3. Preliminaries

### 3.1 Verifiable Credential

### 3.2 Zero Knowledge Proofs

### 3.3 Private Aggregation

## Technical Details


## Application

