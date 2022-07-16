# EtherPSI - DRAFT

*This is work in progress and may change frequently. Check file history for changes*

## Abstract

The [Seeking Psi at the Casino](https://www.researchgate.net/publication/266879484_Seeking_psi_in_the_casino) experiment by Radin and Rebman (2000) examined the correlation between psi capabilities and lunar phases, geomagnetic field fluctuations and tidal forces. This experiment is difficult to reproduce due to the requirement of a casino needing to share historical player data. A system to cheaply and efficiently recreate the conditions of the experiment is described. Smart contracts executing on a blockchain are used to simulate the playing of games in a secure, verifiable and fraud-proof environment without needing a casino. Cryptocurrencies offer the incentives to recreate the behavioural conditions of wanting to win casino games without needing "real money". The smart contracts and methodology are open-source and verifiable to ensure experiment integrity and reproducability. The implications of such a system are discussed.

## Literature Review

Radin, Rebman (2000) describe the ideal conditions for running psi experiments:

From [Seeking Psi at the Casino](https://www.researchgate.net/publication/266879484_Seeking_psi_in_the_casino)

> When parapsychologists dream, they dream about thousands of highly motivated people participating in psi experiments, 24 hours per day, in dozens of laboratories, world-wide.  They dream that these laboratories will be exquisitely sensitive to human needs and desires, yet maintain stringent controls, fraud-proof testing conditions, and obsessive attention to data collection and verification.  Some parapsychologists even fantasize that people might pay for the opportunity to participate in these experiments.

Radin & Rebman were able to achieve their study by requesting data from a casino. It seems quite difficult to imagine any other kind of organization that would satisfy the requirements, other than a casino.

> This dream is a reality today in gambling casinos.  Other than being profit-oriented, many gambling games are essentially identical to experiments conducted in psi laboratories. Thus, if one accepts that precognition and psychokinesis are widely distributed human abilities, then in principle they may also be present in the casino.  That is, some percentage of gambling winnings may be psi-mediated.

They describe the difficulties of testing this hypothesis, and how the execution of the experiment relied on the kindess of a casino executive.

> For years, this has remained an untestable speculation because casinos, like most businesses, tightly control company information.  It is difficult to gain access to daily profit/loss records from casinos, even for research purposes. This problem is compounded for research on possible psi-mediated effects, as casinos are dubious about promoting study of anything that may affect their profits.  Fortunately, an executive at one Las Vegas casino was interested in parapsychology and was kind enough to allow us to examine daily gaming data that allowed us to test the hypothesis of “psi in the casino.”

- Journal of the Society for Psychical Research Printed: 7/19/00 Radin & Rebman Page 2/28

## Objectives

- Improve reproducibility of psi experiments by several orders of magnitude, making it easier, cheaper and efficient while still retaining characteristics of being fraud-proof, verifiable and auditable 
- Develop open-source software to facilitate psi experiments, including necessary smart contract, infrastructure services, data collection/analysis and front-ends as needed. Documentation and code included so experiments can be remixed, modified and reproduced.
- Deliver a proof-of-concept application which implements the system design and allows for runing a psi experiment and associated data collection

## Methodology - Remote Viewing

The methodology defined here describes a way to execute remote viewing experiments while still retaining the characteristics of a secure, verifiable environment that can be audited.
### The Need for Secrets

A system that executes code as a smart contract on a public blockchain would typically enable any and all parties to view the data of those transactions. All validating nodes in the network need to validate that submitted transactions execute correctly according to the rules of the smart contract. Therefore the input data needs to be public in order to perform this validation.

In the example of a remote viewing game, there is a secret `Target` which players must guess the composition of. As this game is played as a smart contract running on a blockchain, there needs to be some way to validate the guesses against the Target. There are a few approaches to implement this. Note: The Target will be assumed to be a series of characters (words, etc) that describe an object/place/thing. Using images or other targets is outside of the scope.

The incorrect approach would be to store the value of the secret in plain text as a smart-contract storage variable. Everyone would be able to see it, even with a basic understanding of how blockchains work (i.e checking Etherscan and reading contract variable). This design is flawed as it allows for "cheating" the game.

### Word Modularization

Before describing implementations, it's important to reduce the scope of the problem a bit. Let's take this series of words as an example target:

> A metal, cyclindrical screw driver with a yellow handle

Any English sentence can be broken down into discrete structures of nouns, adjectives, and verbs.

Nouns:
- screw driver

Adjectives:
- metal
- cyclindrical
- yellow

Verbs:
- *none*

So instead of using the raw sentence above, we can use a series of discrete words that represent the target: [`metal`, `screw driver`, `cyclinder`, `yellow`]. This modularization of the sentence gives a rough approximation of the meaning behind the target. This does impose certain limitations on the kinds of targets selected (targets cannot be photos, they must be words). However, this modularization also grants important characteristics:

- **Scoring system**. By modularizing the Target words, we can easily develop a scoring system for matching against individual words (nouns, adjectives, verbs). We can even compose partial hits and incorporate that into scoring. Ex. 2 partial hits is better than 1, but a full hit is the best. This way, the need for a third-party to evaluate scoring subjectively is eliminated. In fact, the scoring can be done as part of the smart contract logic. Without this modularization, scoring becomes subjective, less strict and must occur off-chain.

- **Language constraints**. Characterizing a target can take on different syntaxes, forms and tenses. `Cyclinder` and `Cylindrical` are equal approximations, with one being an adjective and the other being a noun. When attempting to assign scores to target guesses, it's valuable to normalize these language quirks into a standardized library of words. This normalization forms a sort of guard rail system where guess words can have a large design space but can still allow guesses to stay on track.
### Implementation: Hash commit-reveal scheme
The simplest approach would be to store a hash of the value of the secret. "A one-way hash function is a mathematical function that generates a fingerprint of the input, but there is no way to get back to the original input." [Technipages](https://www.technipages.com/definition/one-way-hash-function).

The MD5 hash with the words (input) `metal screw driver cyclinder yellow` is computed as `8d58ff5c71c00aa59eb4bc32186236de` (fingerprint).

The hash would be enough to conceal the secret from the public, and allow for a time window where guesses are submitted. The person who creates the Target, the `TargetProducer`, must reveal the secret input of the hash after the guessing window is completed. That way, it can be verified that the TargetProducer did not change the target half-way through the test. In other words, the hash commit-reveal is a way to "lock-in" the target.

In this implementation, `Viewers` publish their guess as a blockchain transaction in order to register their guess.

The downsides of this approach are:
1. The TargetProducer must reveal the secret input. If they do not, there can be no verification of guesses against the target.
2. By modularizing the words of the target, someone could iterate through all of the words, hash the results and compare against the `hash(secret)` which is stored on the blockchain. Then they would know what the words were and be able to cheat the system.
3. By publishing a blockchain transaction, the identity of the Viewer is revealed. Wallet addresses are typically thought to be anonymous but in fact they could be de-anonymized by linking to an identity an exchange account, for example. This limitation can possibly be removed altogether by utilizing a zero-knowledge proof submitted to a verifier which verifies the submission without revealing the input address.

To mitigate against 2, the TargetProducer is asked to generate another secret, called the [`salt`](https://en.wikipedia.org/wiki/Salt_(cryptography)). This would prevent a cheater from iterating through all of the words to match the published target, because they wouldn't know the salt. As part of the the reveal stage, the TargetProducer will reveal the secret words as well as the salt. Once the secret and the salt is revealed, scores can be calculated and the target verified as being the same that was initially chosen.

#### Mechanics 
Once the hash is submitted an epoch begins. An epoch is the time period in which a Target has been decided on and viewers can guess what the Target is. For example, an epoch can be 1 day long. Once the epoch is over, the TargetProducer has a limited time to broadcast the secret Target to the network.

### Assessing Target Accuracy
So far, the system is capable of keeping a secret subset of a list of words. When the epoch is concluded and the secret is revealed, published guesses can be matched with a score:
- None: none of the guess words were part of the Target
- Partial: some of the guess words were part of the Target
- Full: all of the words matched exactly

This is possible because of the word modularization of the Target. A guess for `yellow` and `metal` would be counted as a partial score. Once the word list is known, all permutations of the words are automatically checked by the software so that Partial matches can be attributed.

### The Statement Tree
A form of structuring targets is used at `Problems > Solutions > Innovations`, which is Lyn Buchanan's controlled remote viewing program. See [How do you score session results?](https://www.crviewer.com/faqs/training/faq001.php) Lyn also identified the problem of scoring and devised a heirarchical system. In terms of being able to deploy a similar technique that is automatically evaluated in the smart contract, a formal definition is required: divide the target sentence into a sub-tree of statements, where the root of the tree is the most significant description, leaves are descriptive statements, and branches can form which categorize further descriptive statements.
```
The viewer's statement is changed from:
There is a red moving vehicle against an unmarked, green background.
to
- 1. There is a vehicle
    - 1.a which is red
    - 1.b and moving
    - 1.c against a background
        - 1.c.1 which is unmarked
        - 1.c.2 and green
```

*Note* Annotations were added to label each line. In this Statement Tree, the root word is `vehicle` with label `1`, labels `1.a` to `1.c` are direct sub-statements of the root, and `1.c` forms a branch that is further described in statements `1.c.1 ` and `1.c.2`.

Using this `Statement Tree`, it is possible to embed Word Modularization within a heirarchy that describes the meaning and associations between the words, allowing for more precise scoring and a larger design space for the target statement.

From a programmatic perspective, this statement tree is also traversible and verifiable. In the same way that `hash(target)` allows the verification of the integrity of the target, a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) structure allows to verify the integrity of the Statement Tree.

### Reducing coordination
There exists a problem where a TargetProducer can coordinate via private communication channels to Viewers what the target was, allowing the Viewer to cheat and score higher. In this way the integrity of the experiment falls apart from this cheating. Some ways to mitigate this are:

1. Randomly choose `TargetProducer` for a given epoch `E` from a pool of participants. This decreases the chance of coordination on aggregate across multiple.
2. Split the TargetProducer into multiple shards, so that the target words only can describe a partial part of the target: Ex. Shard 1: `yellow handle`, Shard 2: `cyclindrical screwdriver`. However this would then require multiple reveal stages, which can become cumbersome and risks a full reveal not occuring.

There exists the possibility for a so-called "Sybil-attack", where fake participants are programmatically created (similar to bots) and increase the chances for selection as TargetProducer. This can be mitigated with protocols that attest to [Proof-of-humanity](https://www.proofofhumanity.id).

Further reading:
- https://zkasino.medium.com/how-zkasino-will-handle-randomness-on-starknet-9bf1531b1236
https://twitter.com/zkasino_io/status/1499092681590378496?s=21
- https://vitalik.ca/general/2022/06/15/using_snarks.html


### Keeping Secrets (WIP)
- Hash pre-commitments
- Zero-knowledge proofs

### Fraud Proof (WIP)
- Anonymity sets reduce the likelihood of coordination

## Implications

### Reproducability
Psi researchers desire experiments which are verifiable, fraud-proof and easy to reproduce. As *Seeking Psi at the Casino* demonstrates, it is difficult to design an experiment with a large enough sample size to generate significant results.

An `EtherPSI`-like system would allow facilitation of such psi experiments with the click of a button. This system would be significantly less complex from multiple perspectives including logistics, security, planning and more. By reducing the difficulty of running these kinds of experiments, they become easier to reproduce.

### Scalability
The casino used in *Seeking Psi at the Casino* took four years of daily data generated by players. Casinos are one of the only data sources that are fraud-proof, operate in a controlled and secure environment and generate enough data to analyze. However not everyone plays at casinos, data is limited to casinos that are willing to share 

An `EtherPSI`-like system would allow scaling the population size of a psi experiment up to Internet-scale. The scalability of the experiments would only be limited to the incentizes provided to players. This allows experiments to be as small as tens of people all the way up to millions of players.

### Design space
Experiments using casino data are limited to casino games. In *Seeking Psi at the Casino*, these games were: Roulette, Keno, Blackjack, Craps & Slots. A system which replaces the casino with blockchain smart contracts would allow for a larger design space, limited only by the Turing-complete execution environment in which the games run.

This would provide a larger design space in terms of what games can be used for testing. Remote viewing target verification using hash pre-commitments is one such area for further research.

### Decentralized Science (`DeSci`)
The execution of this experiment would demonstrate and further validate there does indeed exist a subset of relatively cheap, achievable science experiments which would likely be denied by mainstream academia or traditional funding mechanisms yet still have profound implications for science.

## Proposal

Human psi capabalities are not yet accepted in the mainstream. An experiment such as this, upon execution and publication of results, would generate media coverage which would increase attention to any sponsoring entities.
