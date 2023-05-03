## TLDR;

## 1. Preface

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

