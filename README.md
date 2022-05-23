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
- Deliver a proof-of-concept application, code-named `EtherPSI` which implements the system design and allows for runing a psi experiment and associated data collection

## Methodology (WIP)
Crypto gaming is one of the burgeoning categories of so-called "web 3" applications that are in development. Many of these use smart contracts as an execution environment for game logic. However, significant challenges remain. In an adversarial environment where "real money" is involved, on-chain mechanism designs need to be verifiable, fraud-proof and legitimate in order for crypto games to thrive.

Fortunately for `EtherPSI`, many of the same innovations that are being used in crypto games are applicable for the psi experiment simulation system described in this document.

### Achieving Randomness (WIP)
- VRF (verifiable random functions)
- RanDAO's

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

Human psi capabalities are not yet accepted in the mainstream. An experiment such as this, upon execution and publication of results, would generate media coverage which would increase attention, reputation and further the legitimacy of any sponsoring entities.
