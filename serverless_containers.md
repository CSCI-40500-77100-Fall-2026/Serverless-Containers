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

## Agenda

* Why this matters: microservices and containers.
* What serverless is: BaaS and FaaS.
* Examples.
* Features and limits of FaaS.
* Pros and cons of serverless and of containers.
* How the two have converged.
* When to pick which, with a case study.

## Why Should We Know?

* Serverless and containers let us build applications based on a microservices architecture.

> Q: How is your project deployed (or how will it be)? Is that serverless, containers, or something else?

::: notes
Open-ended. Typical answers: Vercel or Netlify (serverless functions for the backend, static hosting for the frontend); Cloudflare Workers (serverless); Render, Railway, or Fly.io (containers, often built from the project's Dockerfile); Firebase or Supabase (BaaS); a virtual machine running Docker. Point out that many class projects already mix the two, and the rest of the lecture gives criteria for choosing.
:::

## Recall: Microservices

> "A microservice is an independent and self-contained software component that runs in its own process and communicates with other microservices using lightweight protocols."

> "Microservices architecture is an architectural style in which the system is constructed from communicating microservices. It is well-suited to cloud based systems where each microservice can run in its own container."

From Sommerville (2020), Ch. 6.

## Recall: Containers

* Containers are an *operating system* virtualization technology: independent servers share a single operating system.
	* Usually megabytes in size, whereas virtual machines (VMs) are gigabytes.
	* Start and stop in seconds, rather than the minutes a VM needs.
* Docker: a *Dockerfile* defines an *image*, which packages the software and everything it depends on; a *container* is a running image.
* Kubernetes *orchestrates* containers: runs, replicates, and restarts them across many machines.
* You built a Docker image of your own project in *Containerize Your Project*.

Adapted from Sommerville (2020), Ch. 5, and [Müns (2017)][muns].

## What Is Serverless?

> "Serverless architectures are application designs that incorporate third-party 'Backend as a Service' (BaaS) services, and/or that include custom code run in managed, ephemeral containers on a 'Functions as a Service' (FaaS) platform."

From [Roberts (2018)][roberts].

## What Is Serverless? (cont.)

### Backend as a Service (BaaS)

* Applications that rely mostly or entirely on third-party, cloud-hosted services for server-side logic and state.
* E.g., cloud-accessible databases (Firebase, Supabase) and authentication services (Auth0, Amazon Cognito).

### Functions as a Service (FaaS)

* The developer still writes the server-side logic, but it runs in compute containers that are:
	* Stateless.
	* Event-triggered.
	* Ephemeral: may last for only one invocation.
	* Fully managed by a third party.
* E.g., AWS Lambda (launched 2014), Azure Functions, Google Cloud Run functions, Cloudflare Workers, Vercel Functions.
	* Some run functions in lighter sandboxes than containers (e.g., Cloudflare Workers uses V8 isolates).

Adapted from [Roberts (2018)][roberts] and [Müns (2017)][muns].

## Serverless: Three Essential Qualities

1. An abstraction that hides the servers and the complexity of programming and operating them.
1. Pay-as-you-go pricing instead of reserved capacity: no charge for idle resources.
1. Automatic, rapid scaling up and down to match demand, from zero.

* Serverless is more than FaaS: e.g., object storage (Amazon S3) also hides its servers.
* Analogy: *serverful* computing is like renting a car (reserve it for your whole stay, drive and refuel it yourself); *serverless* is like taking a taxi (pay per ride; someone else drives).

From [Schleier-Smith et al. (2021)][cacm].

## Example of FaaS

* A typical e-commerce app: an online pet store with a traditional three-tier, client-oriented architecture.

![A traditional three-tier pet store](graphics/pet-store-traditional.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

## Example of FaaS (cont.)

* A serverless (FaaS) architecture may end up looking more like this:

![A serverless pet store](graphics/pet-store-serverless.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

> Q: What changed at each numbered step? What did the pet store server do before?

::: notes
1. Authentication moved to a third-party BaaS service (e.g., Auth0).
2. The client reads the product database directly (e.g., Firebase), which a third party hosts; the client needs a different security profile than server code.
3. Logic moved into the client, e.g., tracking the user's session and the app's page structure. The client becomes a single-page app.
4. Search, which needs a lot of data, stays on the server side as a FaaS function behind an API gateway (which maps HTTP requests to functions).
5. Purchases become a separate function, kept on the server side for security (e.g., prices and payments can't be trusted to the client).

Tie-back: one server became several independently deployed pieces, i.e., microservices. The cost: more moving parts to monitor, and logic spread across the client, third parties, and functions.
:::

## Example of FaaS (cont.)

* A different example is an online advertisement system:

![Ad click processing, traditional vs. serverless](graphics/ad-clicks.svg){title="Adapted from Roberts (2018)."}

Example adapted from [Roberts (2018)][roberts].

> Q: What happens in each design when clicks spike tenfold?

::: notes
- Traditional: the one click processor falls behind and the queue grows until someone adds capacity, or autoscaling (if configured) catches up.
- Serverless: the platform starts more function instances automatically, up to the account's concurrency limit, and scales back to zero after the spike; you pay only for the invocations.
- Caveats: the database must absorb the extra concurrent writes, new instances may hit cold starts, and parallel instances may process messages out of order.
- The architecture barely changed here, unlike the pet store; Roberts notes this is why asynchronous message processing is a very popular use of serverless.
:::

## Features of FaaS

* Run backend code without managing your own servers *or* your own long-lived server applications.
* Functions are triggered by event types that the provider defines (e.g., HTTP requests, file uploads, schedules, queue messages).
* Upload the function's code; the provider does the rest (provisioning, running, and scaling it).
* The whole application's business logic is implemented as *functions* and *events*.
* Limits on the programming model:
	* A maximum execution time (e.g., 15 minutes on AWS Lambda).
	* No persistent state between invocations.
	* Restricted networking: functions pass data through shared storage (e.g., S3), not directly to each other.

Adapted from [Roberts (2018)][roberts], [Müns (2017)][muns], [Schleier-Smith et al. (2021)][cacm], and [AWS (2026)][lambda-quotas].

> Q: Where does a FaaS application keep its state, e.g., a shopping cart?

::: notes
Outside the function: in a database (e.g., DynamoDB, PostgreSQL), a cache or key-value store (e.g., Redis), object storage (e.g., S3), or on the client (e.g., cookies, local storage). In-memory state may vanish after the invocation, since the next request may reach a different instance. This matches Sommerville's stateless services (Ch. 6): state is stored in a database or kept by the service requestor.
:::

## Serverless: Pros and Cons

:::::::::::::: {.columns}
::: {.column width="50%"}

### Advantages

* Zero administration: much less operations and maintenance work.
* Faster time to market.
* Automatic scaling.
* Pay per execution: no cost when idle.
* Microservice nature: a clear separation of the code.

:::
::: {.column width="50%"}

### Disadvantages

* No standardization: vendor lock-in.
* "Black box" environment: little visibility into, or control over, where the code runs.
* Cold starts: the first request to a new instance is slow.
* Complex applications can be hard to build.

:::
::::::::::::::

From [Müns (2017)][muns].

> *Note*: Since 2017, CloudEvents (a CNCF graduated project since 2024) has standardized event formats, and Knative and OpenFaaS run functions on any Kubernetes cluster. Each provider's APIs and triggers still differ. Cold starts can be reduced, e.g., with AWS Lambda's provisioned concurrency (always-warm instances, at a cost) or SnapStart (snapshots of initialized functions).

## Containers: Pros and Cons

:::::::::::::: {.columns}
::: {.column width="50%"}

### Advantages

* Control and flexibility: any operating system, language, runtime version, or long-running process.
* Easier migration path for existing (legacy) applications.
* Portability: vendor-agnostic; runs anywhere a container runtime does.

:::
::: {.column width="50%"}

### Disadvantages

* Administrative work (e.g., applying security fixes to images).
* More manual intervention.
* Scaling is slower.
* Running costs: something is always running.
* Hard to get started.

:::
::::::::::::::

From [Müns (2017)][muns].

> *Note*: Since 2017, managed container platforms (e.g., Google Cloud Run, AWS Fargate, Azure Container Apps) have taken over much of the administration and scale containers automatically. Some (e.g., Cloud Run, Azure Container Apps) scale to zero when idle, so nothing runs, and nothing is billed, without traffic.

## The Line Has Blurred

* FaaS platforms run container images.
	* E.g., AWS Lambda since 2020 (images up to 10 GB).
* Functions and containers share a platform.
	* E.g., Google's Cloud Functions became Cloud Run functions (2024), running on its container service.
* "Serverless" functions can run on reserved servers.
	* E.g., AWS Lambda Managed Instances (2025) runs functions on EC2 instances in your own account, billed for the instances (plus a management fee and per-request charges) rather than per execution.
* So the question is less "serverless *or* containers?" than *how much of the platform you manage* and *how you pay for it*.

> Q: Your project has a `Dockerfile`. What would it take to run it serverless?

::: notes
Push the image to a registry and deploy it to a serverless container platform (e.g., Google Cloud Run). The app must listen on the port the platform provides (often via a `PORT` environment variable), keep no state in memory or on local disk between requests, and start quickly. Moving it to FaaS instead would usually mean restructuring the code into event handlers.
:::

## When to Pick Which

1. Costs.
	* Serverless: pay only while code runs. Unit prices are higher (a Lambda function can cost about 7.5× a small VM per minute), but they include availability, monitoring, logging, and scaling.
	* Containers: pay for capacity whether it is used or not, so steady, high load can be cheaper.
1. Portability.
	* Containers run anywhere a container runtime does; FaaS code depends on the provider's triggers and services.
1. Convenience.
	* Serverless: no infrastructure to manage, so faster iteration.
	* Containers: full control of the operating system and runtime, at the cost of more setup and maintenance.

> Rule of thumb: containers when you need flexibility or are migrating legacy services; serverless when you need speed of development, automatic scaling, and lower running costs ([Müns, 2017][muns]).

Costs from [Schleier-Smith et al. (2021)][cacm].

> Q: Which would you choose for your project? Why?

::: notes
Other factors worth naming:

- Workload shape: short, event-driven tasks (webhooks, image resizing, scheduled jobs) suit FaaS; long-running processes, WebSockets, and background workers suit containers.
- Latency: user-facing requests that can't tolerate cold starts favor always-on containers (or warm instances).
- Existing code and team skills: an existing app is easier to containerize than to split into functions.
- Mixing is common: e.g., a containerized API plus functions for scheduled jobs.
- For a class project with little traffic, serverless free tiers are often enough; a container image keeps the project portable across hosts.
:::

## Case Study: Prime Video (2023)

* Prime Video built a service that monitors its live streams for defects from serverless components (AWS Step Functions, AWS Lambda), passing video frames between them through Amazon S3.
* It hit a hard scaling limit at about 5% of the expected load:
	* Step Functions made several state transitions for every second of video, reaching account limits and billing per transition.
	* The many S3 calls were expensive.
* The team moved the service into a single process (a monolith) on Amazon EC2 and ECS.
	* Infrastructure costs fell by over 90%, and the service scaled further.
* Their takeaway: whether to use microservices and serverless "has to be made on a case-by-case basis."

From [Kolny (2023)][prime-video].

> Q: Why did serverless cost so much here? Would the same happen to your project?

::: notes
- The load was constant and heavy (every second of thousands of streams), so pay-per-use had no idle time to save; reserved capacity (EC2 Savings Plans) was cheaper.
- The design was very fine-grained: per-transition billing in Step Functions multiplied with every second of video.
- Data passed between functions through S3 (the restricted networking limit); in one process, it passes through memory for free.
- Not an indictment of serverless: the team chose it to build quickly, and the post itself says the choice is case by case. Many commentators read it as a workload that outgrew its first design.
- A class project's traffic is small and spiky, the opposite case, where serverless tends to be cheap.
:::

## Summary

* Serverless covers BaaS (third-party services) and FaaS (developer code run as managed, event-triggered, stateless functions).
	* Its essential qualities: hidden servers, pay-as-you-go pricing, and automatic scaling.
* Serverless trades control for convenience: no servers to manage and no charge when idle, but vendor lock-in, cold starts, and limits on execution time, state, and networking.
* Containers trade convenience for control: portable and flexible, but more to manage and, unless the platform scales them to zero, always running.
* The two have converged: functions run as containers, and containers run serverless.
* Neither is better in general: choose by cost, portability, and convenience for *your* workload.

## References

1. Sommerville, I. (2020). *Engineering Software Products: An Introduction to Modern Software Engineering*. Pearson.
1. Roberts, M. (2018). [Serverless Architectures][roberts]. martinfowler.com.
1. Schleier-Smith, J., Sreekanti, V., Khandelwal, A., Carreira, J., Yadwadkar, N. J., Popa, R. A., Gonzalez, J. E., Stoica, I., & Patterson, D. A. (2021). [What Serverless Computing Is and Should Become: The Next Phase of Cloud Computing][cacm]. *Communications of the ACM*, 64(5), 76–84.
1. Amazon Web Services. (2026). [Lambda Quotas][lambda-quotas]. *AWS Lambda Developer Guide*.
1. Kolny, M. (2023, March 22). [Scaling Up the Prime Video Audio/Video Monitoring Service and Reducing Costs by 90%][prime-video]. Prime Video Tech. Archived by the Internet Archive.
1. Müns, P. (2017). [Serverless (FaaS) vs. Containers—When to Pick Which?][muns] Serverless, Inc.

[svc]: https://s3.amazonaws.com/files.commons.gc.cuny.edu/wp-content/blogs.dir/2880/files/2021/05/Serverless_vs_containers.pdf
[roberts]: https://martinfowler.com/articles/serverless.html
[muns]: https://www.serverless.com/blog/serverless-faas-vs-containers
[cacm]: https://doi.org/10.1145/3406011
[lambda-quotas]: https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html
[prime-video]: https://web.archive.org/web/2023/https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90
