---
title: "Serverless (FaaS) vs. Containers: When to Pick Which?"
author: Raffi Khatchadourian (based on a "[Serverless (FaaS) vs. Containers][svc]" student presentation from CSCI 40500/77100, City University of New York (CUNY) Hunter College, Spring 2021)
date: October 15, 2026
semester: Fall 2026
lang: en
footer: Based on a "[Serverless (FaaS) vs. Containers][svc]" student presentation from Software Engineering (CSCI 40500/77100), City University of New York (CUNY) Hunter College, Spring 2021
license: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
---

# Serverless (FaaS) vs. Containers: When to Pick Which?

## Why Should We Know?

* Serverless and containers let us build applications based on a microservices architecture.

## Recall: Microservices

> "A microservice is an independent and self-contained software component that runs in its own process and communicates with other microservices using lightweight protocols."

> "Microservices architecture is an architectural style in which the system is constructed from communicating microservices. It is well-suited to cloud based systems where each microservice can run in its own container."

From Sommerville (2020), Ch. 6.

## What Is Serverless?

> "Serverless architectures are application designs that incorporate third-party 'Backend as a Service' (BaaS) services, and/or that include custom code run in managed, ephemeral containers on a 'Functions as a Service' (FaaS) platform."

From [Roberts (2018)][roberts].

## What Is Serverless? (cont.)

### Backend as a Service (BaaS)

* Applications that rely mostly or entirely on third-party, cloud-hosted services for server-side logic and state.
* E.g., cloud-accessible databases (Parse, Firebase) and authentication services (Auth0, AWS Cognito).

### Functions as a Service (FaaS)

* The developer still writes the server-side logic, but it runs in compute containers that are:
	* Stateless.
	* Event-triggered.
	* Ephemeral: may last for only one invocation.
	* Fully managed by a third party.
* AWS Lambda is one of the most popular FaaS platforms.

Adapted from [Roberts (2018)][roberts].

## Example of FaaS

* A typical e-commerce app: an online pet store with a traditional three-tier, client-oriented architecture.

![A traditional three-tier pet store](graphics/pet-store-traditional.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

## Example of FaaS (cont.)

* A serverless (FaaS) architecture may end up looking more like this:

![A serverless pet store](graphics/pet-store-serverless.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

## Example of FaaS (cont.)

* A different example is an online advertisement system:

![Ad click processing, traditional vs. serverless](graphics/ad-clicks.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

## Features of FaaS

* Run backend code without managing your own servers *or* your own long-lived server applications.
* Functions are triggered by event types that the provider defines (e.g., HTTP requests, file uploads, schedules, queue messages).
* Upload the function's code; the provider does the rest (provisioning, running, and scaling it).
* The whole application's business logic is implemented as *functions* and *events*.

Adapted from [Roberts (2018)][roberts] and [Müns (2017)][muns].

## Comparing FaaS with Containers

:::::::::::::: {.columns}
::: {.column width="50%"}

* Advantages of serverless
	* Zero administration
	* Significantly reduced administration and maintenance burden
	* Faster time to market
	* Auto-scaling
	* Pay-per-execution
	* Zero cost for idle time

:::
::: {.column width="50%"}

* Disadvantages of serverless
	* No standardization
	* Vendor lock-in
	* "Black box" environment
	* Cold starts

:::
::::::::::::::

From [Müns (2017)][muns].

## Comparing FaaS with Containers (cont.)

:::::::::::::: {.columns}
::: {.column width="50%"}

* Advantages of containers
	* Control and flexibility
	* Easier migration path
	* Portability

:::
::: {.column width="50%"}

* Disadvantages of containers
	* Administrative work
	* More manual intervention
	* Scaling is slower
	* Hard to get started

:::
::::::::::::::

From [Müns (2017)][muns].

## When to Pick Which

1. Costs
1. Portability
1. Convenience

## References

1. Sommerville, I. (2020). *Engineering Software Products: An Introduction to Modern Software Engineering*. Pearson.
1. Roberts, M. (2018). [Serverless Architectures][roberts]. martinfowler.com.
1. Müns, P. (2017). [Serverless (FaaS) vs. Containers—When to Pick Which?][muns] Serverless, Inc.

[svc]: https://s3.amazonaws.com/files.commons.gc.cuny.edu/wp-content/blogs.dir/2880/files/2021/05/Serverless_vs_containers.pdf
[roberts]: https://martinfowler.com/articles/serverless.html
[muns]: https://www.serverless.com/blog/serverless-faas-vs-containers
