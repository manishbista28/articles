## Abstract

This article proposes a privacy friendly means of collecting and aggregating statistics from a group of users through the use of verifiable credential. Each user holds a set of private data values (e.g. age, salary, health records, preferences) as attributes in a credential. Credential held by each of the users is first split and the splits are distributed amongst a small set of trustless aggregation servers. This way no single server learns the complete value and as long as at least one of these aggregation servers is honest, the user's private data is not exposed. The aggregation servers jointly compute aggregate statistic (e.g. mean, frequency count, variation) over attribute(s) of the credential. To ensure any party can verify correctness of the computation, a zero knowledge proof is created by each of the servers.

Here, user's private data is represented in the form a verifiable credential. Having the data in this form provides multiple benefits. First, the aggregation servers can verify the credential to ensure that the values contained in it satisfy the necessary criterion and aren't malicious. This is useful for spam protection, avoid duplicate submissions, maintain blacklisting through credential revocation list, targeting specific demograpic and more. 

Alongside this, verifiable credential can be presented individually (i.e. without aggregation) either by revealing its attribute values or by providing a zero knowledge proof of apredicate on the attribute. These are well known ways in which verifiable credentials are used. Credentials can be issued such that even the issuer does not know the exact values contained in it, thus providing privacy from issuers. However, to ensure that the supplied values agree to some constraints (e.g. falls in some range), the users provide zero knowledge proof to issuer proving that the request is valid.

With combined benefits of verifiable credential and private aggregation, user's private data is never revealed to any of the parties (unless deliberate) yet verifiable aggregate statistic can be computed.

We remark here that use of verifiable credential to hold private values makes it difficult to work on large volumes of data per user since each of these data values seemingly needs to first go through issuance process and other related heavy computations. However, in this context, we can use credential as the initial condition to establish trust, while successive points can chain of trust starting from the initial credential. This eliminates the requirement for a separately issued credential for each data points. For this design, we take references from the technology behind expensive key-agreement protocols (e.g. X3DH) followed by cheap message exchange protocols (e.g. Double Ratchet alogrithm).


## 1. Introduction

### 1.1 Private Aggregate Statistic

A user's private data (e.g. credentials, web browser activity, gps location, health records) can be used together with readings of same type from other users to draw aggregate statistic in a way readings from any single user is not known to anyone else and as such privacy is preserved. Some real world usage of such tech are the following.

- Mozilla web browser collects telemetry data and generates aggregate statistics. This is helpful to know prevalance of errors, performance metrics, top sites visited and user preference, ad categories viewed or clicked and so on.
- ENPA notifies user of potential exposure to COVID by collecting proximity data.

### 1.2 Verifiable Credential

A different way for a user to report private data is through the presentation of a verifiable credential. A credential (e.g. birth credential) is a combination of attributes (e.g. DOB, gender, location) which is issued by an issuer (e.g. government authority) and can later by presented to a verifier (e.g. liquor store) as a proof of having met some necessary criteria (e.g. above age 18). Following are some of its noteworthy properties.

- Blind Issuance: Issuer does not have to know attribute values requested to be included in a credential. Instead it might require a proof that the attribute value meet certain criteria (e.g. value is from one of the possible choices) which can be done with zero-knowledge proofs.
- Composition: Different credential (e.g. age credential and university credential) can be composed to create a new one that includes selected attribute values from the source. This is done without having to consult with the issuer again.
- Multi-show unlinkability: Multiple presentation of the same credential can not be linked together. As such, same credential can be presented on different places without having to worry about the chances of usage being correlated.
- Selective Disclosure: Only selected attributes of a credential can be disclosed to the verifier while the rest will remain hidden. Despite having some of these values hidden, the verifier can still verify that the credential is well formed with respective to these attribute combination.
- Predicates on attributes: Instead of revealing any attribute, the verifier can prove that some attribute meets some criteria.

### 1.3 Mixing credentials and aggregation

Verifiable credential and private aggregation are similar in that they both work on user private data and provide ways to present it or some computation on it. However they each have their shortcomings or are incomplete individually. Some of the reasons are as follows.

- Repeated credential issuance or for large number of data points is computationally costly and is not what verifiable credentials are designed for. So if a use case requires working on frequent data and relatively fast computation, the flow of credential issuance and presentation will not sustain.
- Private Aggregation requires the input data points to be from credible source but does not itself have ways to ensure it.

Use of verifiable credential provides authentication, integrity of data, spam protection, avoiding duplicates and maintaining blacklisted requests. These features are required and helpful for private aggregation.


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

