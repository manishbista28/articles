## Background
Verifiable Credentials are cryptographic proofs of user data issued by some responsible authority. They can be presented to any party who can verify the validity of this credential and attributes in it. A frequent example is that of a birth certificate, issued by some government authority, which can be submitted to a liquor store such that only the age or proof of it meeting the age threshold is submitted to the store owner. Few more examples, it could be gym and store memberships, conference tickets or a KYC credential sought by some decentralized application; there may be more but all of these examples have the same taste to them. For a technology that proves a user attribute and is also practical, the possibilities should have been much more diverse. So what do these examples fail to demonstrate ?

These examples lack an appreciation of the overall system where credential presentation is only but a small part. The store membership you use will get you to buy from the same place again for discounts, possibility for rewards, suggestion for stuffs you are likely to buy; it could also be used by the store to run some analytics and figure out how it can optimize its operation. A conference ticket could give you exclusive access to some meetings and presentations, help the organizers conduct surveys and find you prospects you are likely to have a business relation with. So when you think about integrating verifiable credential, consider how it will fit into an already optimized system and if it can squeeze some more value out of it. Seems a bit vague, so let's break down the requirements for integrating verifiable credential and analyze possible difficulties in achieving them.

### Problem Definition
| Requirements | Problems |
| ------------ | ------------ |
| Onboarding users: <ul></ul> To onboard issuers, holders and any other parties, you need to have a strong enough reason to pull them to a market and technology they're not already familiar with. | Chicken and Egg Problem: <ul></ul> The task is difficult because the current pool of users and issuers is small, the infrastructure is costly and each requires the other to be present first. Additionally, some usecases may require credential from disparate sources, not all of which may be available. |
| Fulfills minimum requirement: <ul></ul> The users must be able to use the credential where it's needed. | Credentials may not be interoperable: <ul></ul> Protocol behind credentials can be different, in which case the same information present in a credential will also have to be embedded in another credential of different type to get past compatibility issues.  |
| Fulfills larger requirement: <ul></ul> As discussed previously, using verifiable credential should not deprive the whole system of its current capabilties and the credential itself also should not lose its own features during the process. | Complex Requirements: <ul></ul> Businesses use data in a diverse set of ways, not all of which may be compliant or want to keep up with privacy & security requirements sought by verifiable credential. |
| Incentive to stay: <ul></ul> One of the main draws of verifiable credential is that multiple credentials can be composed together to achieve higher form of use cases. Users should be able to reap benefit of the overall ecosystem of credentials, not just get locked to a minor specific use case. | Mechanism Undefined <ul></ul> How users can discover new credentials or usecases their existing credentials can fulfill, how much value they can gain from such interaction and similar problems have not been tackled. |

The currently deployed solutions are susceptible to the above mentioned difficulties too and the primary impact has been on scale of adoption. If the requirements mentioned above are essential, the difficulties therein inherent and its effects apparent, how do we go about addressing this situation ? The first step might be to acknowledge that some of these problems may not be perfecty solvable straight away. For example, the difference in protocol standard is something all new technologies encounter or the chicken and egg problem is faced by all two-sided platforms yet successful products have been getting past them. In concrete terms, following are the suggested solutions to the above problems.

### Possible Solutions
| Problems | Possible Solutions |
| -------- | ------------------ |
| Usescases without necessary credentials | We require a looser definition of verifiable credential. Credential from authorized source has its special features but we shouldn't fail to notice that much of the internet is powered by user supplied data too e.g. user profile information in social media. Using an interim solution helps demonstrate the value of onboarding authorized sources and enrich the value of interim solution provider. |
| Interoperability | IETF Working Group for Verifiable Credential has suggested BBS+ Signatures with JSON-LD scheme as a standard. |
| Complex Requirements | Advances in private aggregation have demonstrated ways computation can be done all the while maintaining strong security guarantees. These approaches realize the differences in nature of data and businesses and have tools to cover a wide range of use cases |
| Credential and usecase discovery | Finding useful credential and usecases requires known repository and an efficient search engine. Recent advances in zero knowledge have enabled running such searches to be done privately on one's local device with low computational cost. As such you can find and check for potential matches without leaking any private information. This ability keeps in with the privacy guarantees. |
| Incentives and Rewards | Though mechanism design and tokenomics have dealt with the problem, a concrete answer requires evaluating the situation in depth. |
| Onboarding | Find ways an issuer can estimate the potential user base he could tap into. This has commonly been done as part of market research and value realization. For example, running a targeted survey could help draw up some answers. |


## The Product

We finally introduce a product that serves as a platform to help users discover and realize the value of verifiable credentials. It does so by wrapping the above solutions to provide the following set of features.
1. A search-engine like portal where users can discover how they can use their credentials and data requesters can clearly express what they seek from user data.
2. A core set of ready-to-use libraries to help you get started with integrating verifiable credential, whether you're a provider, a holder or a requester of user data.
3. Incentivization tools to ensure involved parties can exchange value conveniently.
All of this is done while maintaining strong privacy requirements throughout the process. 

### What it offers
We begin with a set of examples that help demonstrate capabilities of the product. Following are some of them.

1. Ticketing
    
    Premise:

    A user is given a credential that serves as a ticket for a conference. In the credential is a "job" attribute with value "developer". The user has enabled queries from conference organizers to access his credential and notifications if they pass. He finds out that developer meeting is to be held in a different room.

    Usecase:

    This serves as a commonly given example of ticketing system where it's used for role based access control, but what's different here is an illustration of how the ability to make queries can help organizers manage the event better. 

2. Advertisement

    Premise:

    A social media application (e.g. Lens Protocol) keeps track of how active you've been on the platform and issues a credential each week that includes your activity score. Or, a web browser extension or a system application generates a credential mentioning how much you've been using different types of applications. 
    Secondly, an application, that wants to help detox users from social media, creates a query that checks if activity attributes in the user credential is within harmful range. A user's web application will pull the query, run checks privately to see if the query matches his credential and suggest the detox app and its benefits.

    Usecase:

    This serves as a privacy friendly approach to advertisement, yet it does not stray far from the initiatives already being taken in the Advertisement ecosystem. Google Topics API, an ongoing project to eliminate use of third party tokens for personalized ads, also uses a similar approach where it parses browing history to create a set of topics the user might be interested in.

3. Surveys

    Premise:
    
    An HBR Survey wants to check if minority groups living in a certain location are susceptible to discrimination in salary or loan applications. It creates a query that matches if users are from the target demography and requests the platform to provide it an aggregate value of salary or credit worthiness. It can use this aggregate value to compare against the same metric gathered from a different demography. To ensure participation, it has offered certain amount of tokens for willing participants.

    Usecase:

    First, it demonstrates why aggregation is necessary in a platform. A surveyor that can directly access data values per user can later use/sell it on a different market. Providing the surveyor with aggregates alone limits its reach over user's private data. Secondly, the example shows that a surveyor in need of a more complex metric (e.g. credit worthiness derived from a set of user attributes) will benefit from verifiable computation offered by Zero-Knowledge technology embedded in the platform. Finally, the use and value of tokens as an incentive for participation shows why the platform can work alongside blockchain technologies.


### Market Opportunity

1. To the best of our knowledge, there isn't another product which offers similar benefits and also fulfills privacy requirements. 
2. There has been push towards adopting more privacy friendly approaches to handling user data e.g for advertisements.
3. Blockchain products have been advocating for verifiable credentials and decentralized identity.
4. Verifiable Credentials are cryptographic proofs and can be valid on any blockchain. This provides a larger shared ground for lucrative integrations.

## Roadmap

| Milestone | Dates |
| --------- | ----- |
| Research and develop a working proof of concept | March-June |
| Integrate applications | June+ |

For a more detailed list, see [activity log](https://github.com/manishbista28/articles/tree/main/credentialportal/activity.md).

### Screenshot of PoC application:

<img src="./overview_page_screenshot.png" alt="otr-simplified" width=800 height=400/>

Fig 1: Overview page showing queries that a user can interact with

<img src="./credential_issuance.png" alt="otr-simplified" width=800 height=400/>

Fig 2: Credential page showing how one can request for a credential