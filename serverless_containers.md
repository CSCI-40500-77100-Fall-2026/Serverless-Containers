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

* A microservice is an independent and self-contained software component that runs in its own process and communicates with other microservices using lightweight protocols.
* A microservices architecture is an architectural style in which the system is constructed from communicating microservices. It is well suited to cloud-based systems where each microservice can run in its own container.

## What Is Serverless?

* Serverless computing is an application design that incorporates third-party "Backend as a Service" (BaaS) services, and/or that includes custom code run in managed, ephemeral containers on a "Functions as a Service" (FaaS) platform.

## What Is Serverless? (cont.)

1. Serverless was first used to describe applications that significantly or fully incorporate third-party, cloud-hosted applications and services, to manage server-side logic and state. These types of services have been previously described as BaaS, for example, cloud-accessible databases (e.g., Parse, Firebase), authentication services (e.g., Auth0, AWS Cognito), and so on.
1. Serverless can also mean applications where server-side logic is still written by the application developer, but, unlike traditional architectures, it runs in stateless compute containers that are event-triggered, ephemeral (may only last for one invocation), and fully managed by a third party. One way to think of this is "Functions as a Service" (FaaS). AWS Lambda is one of the most popular implementations of a Functions-as-a-Service platform at present.

## Example of FaaS

* A typical e-commerce app: an online pet store with a traditional three-tier, client-oriented architecture.

![A traditional three-tier pet store](graphics/pet-store-traditional.svg)

## Example of FaaS (cont.)

* A serverless (FaaS) architecture may end up looking more like this:

![A serverless pet store](graphics/pet-store-serverless.svg)

## Example of FaaS (cont.)

* A different example is an online advertisement system:

![Ad click processing, traditional vs. serverless](graphics/ad-clicks.svg)

## Features of FaaS

* FaaS is about running backend code without managing your own server systems or your own long-lived server applications.
* Functions in FaaS are typically triggered by event types defined by the provider.
* In a FaaS environment we upload the code for our function to the FaaS provider, and the provider does everything else necessary.
* The most basic premise of a serverless setup is that the whole application—all its business logic—is implemented as *functions* and *events*.

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

## When to Pick Which

1. Costs
1. Portability
1. Convenience

## References

1. Sommerville, I. (2020). *Engineering Software Products: An Introduction to Modern Software Engineering*. Pearson.
1. Serverless Architectures. <https://martinfowler.com/articles/serverless.html>
1. Serverless (FaaS) vs. Containers—When to Pick Which? <https://www.serverless.com/blog/serverless-faas-vs-containers>

[svc]: https://s3.amazonaws.com/files.commons.gc.cuny.edu/wp-content/blogs.dir/2880/files/2021/05/Serverless_vs_containers.pdf
