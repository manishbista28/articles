## Abstract

This article proposes a privacy friendly means of collecting and aggregating statistics from a group of users through the use of verifiable credential. Each user holds a set of private data values (e.g. age, salary, health records, preferences) as attributes in a credential. Credential held by each of the users is first split and the splits are distributed amongst a small set of trustless aggregation servers. This way no single server learns the complete value and as long as at least one of these aggregation servers is honest, the user's private data is not exposed. The aggregation servers jointly compute aggregate statistic (e.g. mean, frequency count, variation) over attribute(s) of the credential. To ensure any party can verify correctness of the computation, a zero knowledge proof is created by each of the servers.

Here, user's private data is represented in the form a verifiable credential. Having the data in this form provides multiple benefits. First, the aggregation servers can verify the credential to ensure that the values contained in it satisfy the necessary criterion and aren't malicious. This is useful for spam protection, avoid duplicate submissions, maintain blacklisting through credential revocation list, targeting specific demograpic and more. 

Alongside this, verifiable credential can be presented individually (i.e. without aggregation) either by revealing its attribute values or by providing a zero knowledge proof of a predicate on the attribute. These are well known ways in which verifiable credentials are used. Credentials can be issued such that even the issuer does not know the exact values contained in it, thus providing privacy from issuers. However, to ensure that the supplied values agree to some constraints (e.g. falls in some range), the users provide zero knowledge proof to issuer proving that the request is valid.

With combined benefits of verifiable credential and private aggregation, user's private data is never revealed to any of the parties (unless deliberate) yet verifiable aggregate statistic can be computed.

We remark here that use of verifiable credential to hold private values makes it difficult to work on large volumes of data per user since each of these data values seemingly needs to first go through issuance process and other related heavy computations. However, in this context, we can use credential as the initial condition to establish trust, while successive points can form a chain of trust starting from the initial credential. This eliminates the requirement for a separately issued credential for each data points. For this design, we take references from the technology behind expensive key-agreement protocols (e.g. X3DH, asymmetric encryption) followed by cheap message exchange protocols (e.g. Double Ratchet alogrithm, symmetric encryption).


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

## 2. Related Work

- ENPA
- DivviUP
- Prio
- AdScale
- Hyperledger's Ursa, Aries
- Evernym

## 3. Technical Details

### 3.1. Credential Issuance
- A schema which specifies the attributes present in a credential is published by issuer or any third party. If the values for the attributes need to satisfy some predicate, then zero-knowledge proof statement is also published for users to use.
- Users find the credential(s) which they want to get issued. They create appropriate request based on the schema published. Additional criterion like having to satisfy some predicate to be able to apply for certain type of credential depends upon the usecase.
- Issuer issues verifiable credential based upon the issuance request and the user should be able to verify that the provided credential is correctly formed.

### 3.2. Credential Presentation without aggregation
- A data requester will create a request object which includes the attribute he wants to obtain from the users and optionally, the criteria that defines who is eligible to make submissions.
- A user, if he wants to, will present the necessary credential. 

### 3.3. Credential Presentation with aggregation
- A data requester will create a request object as mentioned above.
- User will split the attribute value(s) using additive secret sharing and distribute shares of the credential to each of the aggregation servers.
- An aggregation server will recieve his secret share of the attribute values and commitment to secret shares of the same attribute values of other users. This much info is sufficient for each of the aggregation servers to individually verify that the credential was well formed.
- The aggregation servers receive such credentials from multiple users to compute aggregate statistic.
- During aggregation, the servers require Beaver multiplication triple from a trusted source. These values are usually submitted by the data requester himself.
- The aggregation servers now use recursive zk-SNARK to compute aggregate statistic and a proof of correct computation. Such proof is verified by the other aggregation servers.
- After each aggregation result is verified, then they are all published and the addition of these values is the actual aggregate statistic. The data requester will receive this value.

## 4. Application

### 4.1 Advertisement without third party cookies
### 4.2 Data Market
### 4.3 User profile integrations
### 4.4 Machine Learning
