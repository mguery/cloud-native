# Cloud Native

**Learning how to construct a CI/CD pipeline that will containerize an application and deployed it to a Kubernetes cluster.**

Notes from [Cloud Native Foundations](https://www.udacity.com/scholarships/suse-cloud-native-foundations-scholarship)

Hands-on: Python and Flask, Docker, Kubernetes, the CLI, Git 

# Lesson 1: Intro to Cloud-Native

**Cloud native** a set of practices that empowers an organization to build and manage applications at scale. 

**Containers** Small units of an app. Easy to manage, deploy, and fast to recover. Fits well with a microservice-based architecture

**Microservices** are used to manage and configure a collection of small, independent services that can be easily packaged and executed within a container.

Resources
[What's Cloud Native](https://www.ibm.com/cloud/learn/cloud-native)

---

## CNCF and CN tooling

K8s - Google open-source container orchestrator with the integration of the following:
- runtime for app execution env
- ntwg for app connectivity
- storage for app resources
- service mesh for granular control of traffic w/in cluster
- logs and metrics
- tracing for building full req journey

Cloud native computing foundation (CNCF) - vendor neutral home to open-source projects such as K8s, Prometheus, and more.

---

## Stakeholders

Use cloud-native tooling to enable quick delivery of value to customers and easily extend to new features and tech. Main perspectives:
- business (agility - perform transformations, growth - quickly iterate based on feedback, and service avail. - 24/7)
- technical (automation, orchestration - to mng 1000s of svcs w/ minimal effort, observability - independently and debug each component)

# Lesson 2: Architecture Consideration for CN Apps

## Intro 

Go through a design phase to identify the main requirements and structure of an application. Then team chooses the most suitable project arch.


Learn about:
- Monoliths and Microservices
- Trade-offs for Monoliths and Microservices
- Practices for Application Development

---

## Design Considerations

**Step 1**: List functional reqs or what the app should deliver to end users. Starting pt - who are the stakeholders, functionalities needed, who are the end users, input and output process (notify auto. or need customer input), engineering teams (skilled and avail engineers needed)

**Step 2**: List avail. resources for implementation. Starting pt - engineering res (# of engs or contractors avl), fin. res (budget), timeframes, internal knowledge

Knowing the reqs and res can help you choose btwn monolithic and microservices. 

---

## Monoliths and microservices

Main goal is to have a well-designed app the can provide value to customers for satisfactory UX and allows engs easily adjust and extend existing functionalities.

App tiers - UI (handle HTTP reqs from users and return a response), biz logic (collection of funcs that provides a svc to users), data layer (access and storage of data objects)

Ex. - mobile app that lets users read the latest articles. UI - mobile app, BL - func to get the articles, DL - func to store user preferences for articles

Monoliths - all 3 tiers in a single unit, sharing the same resources - cpu and mem, same programming lang/single codebase

Microservices - all 3 tiers are mgd as small independent units, sep svc (allocated res, well-defined api for cnnx to other units), every unit is written in the lang of choice, sep repo

---

## Tradeoffs

Know the tradeoffs for ml and ms and how the app will be maintained. This provides a clear project roadmap.

tradeoff | monoliths | microservices | considerations 
--- | --- | --- | --- 
**development complexity** - effort reqd to deploy and mg an app | 1 lang, 1 repo, sequential dev'mt | 2+ langs and repos, concurrent dev | continuous project maintainance based on user feedback
**scalability** - how app can scale up or down based on traffic (more replicas) | replicate entire stack | replicate single unit |  scale down when requests are low
 **time to deploy** - build of a delivery pipeline that is used to ship features | 1 pipeline deploys entire stack (more risk) | multiple delivery pipelines deploying sep units (less risk, higher ftr dev'mt rate) |  pipeline to ensure successful product release
**flexibility** - adapt to new tech and introduce new functionalities  | restructuring entire stack to add new functs = low rate | changes made to a single unit = high rate | adopt new functs and tooling 
 **operational cost** - reps the cost of resources to release product | low cost but cost increases when app needs to operate at scale | high cost but the costs based on consumed resources | resources necessary throughout prj dev'mt and their cost
**reliability** - how app quickly recovers from failure and tools to monitor an app | entire needs to be recovered, logs and metrics are mixed together | only failed unit is recovered, with logs an metrics for that unit | respond to failure based on logs and metrics

---

## Best practices for app deployment

After you figure out the architecture (ml or ms), next step is to build. Whichever you choose, apply the best practices to improve the app lifecycle. This increases resiliency, lowers time to recover, provides transparency of how a service handles incoming requests.

These practices are focused on health checks, metrics, logs, tracing, and resource consumption.

**health checks**
- showcases the status of an app - status report
- report if an application is running and meets the expected behavior to serve incoming traffic
- hc are represented by an http endpoint - '/healthz' or '/status'
- returns response - healthy or error

**metrics**
- needed to understand a behavior of an app
- stats should be gathered for individual service (cpu, thruput, memory)
- used and handled resources (# of requests, active users, # of logins) 
- http endpoint - '/metrics'
- return response - `{"UserCount":140, "UserCountActive":23"}`
- read about [Best practices: Metric and label naming](https://prometheus.io/docs/practices/naming/)

**log aggregation**
- logs are used to record ops performed by a svc
- return response `{"date":"2021-01-01 02:10:12", "severity":"ERROR", "msg":"Login failed for user FOO"}`
- logs are collected from STDOUT & STDERR thru passive logging (sends output and errors to shell)
- active logging - direct interaction (logs sent to backend storage without a logging tool reqd - like Splunk)
- some logging levels - DEBUG, INFO, WARN, ERROR (app runnin but notified of an error), FATAL (app not operational) 
- read [Logging levels](https://www.tutorialspoint.com/log4j/log4j_logging_levels.htm) and [How to Log a Log: Application Logging](https://logz.io/blog/logging-best-practices/)

**tracing**
- shows how difft svcs are invoked to fulfill a request
- recreate the request lifecycle and ID key metrics within an app
- tracing is implemented in the app layer where a dev can record when a function is invoked/executed
- these records for single services are spans
- collection of spans define a trace that recreates the entire lifecycle of a request
- read about [distributed tracing](https://containerjournal.com/topics/container-ecosystems/enabling-distributed-tracing-for-microservices-with-jaeger-in-kubernetes/)

**resource consumption**
- refers to the amount of CPU and memory an app requires to be fully operational (return response - CPU: 1, memory: 256mb)
- network throughput - making sure the app has enough bandwidth to handle mulitple requests (return response - 100Mb/s)

Resources
[Microservice Architecture](https://microservices.io/index.html)

---

## Edge Case: Amorphous Applications

After product is released, next step in the app lifecycle is maintenance. Both monolith and microservice-based applications transition in the maintenance phase after the release. it is always beneficial to focus on extensibility rather than flexibility. 

### Applications ops during maintenance phase

Any of these operations increases the longevity and continuity of a project. End goal is to provide value to cust and the app is easy to manage by the eng team. Not static but amorphous, evolving based on new reqs and cust fdbk.

- **split operation** - is applied if a service covers too many functionalities and it's complex to manage. Having smaller, manageable units is preferred in this context.
- **merge operation**- is applied if units are too granular or perform closely interlinked operations, and it provides a development advantage to merge these together. For example, merging 2 separate services for log output and log format in a single service.
- **replace operation** - is adopted when a more efficient implementation is identified for a service. For example, rewriting a Java service in Go, to optimize the overall execution time.
- **stale operation** - is performed for services that are no longer providing any business value, and should be archived or deprecated. For example, services that were used to perform a one-off migration process.

# Lesson 3: Container Orchestration with Kubernetes

## Docker for Application Packaging

### Dockerfile
A set of instructions used to create a Docker image

FROM - to set the base image 
LABEL - `LABEL maintainer=Marjy G`
RUN - to execute a command 
COPY & ADD  - to copy files from host to the container 
CMD - to set the default command to execute when the container starts
EXPOSE - to expose an application port 


### Docker Image
A read-only template that enables the creation of a runnable instance of an application. In a nutshell, a Docker image provides the execution environment for an application

To build an image `docker build`

To build an image using the Dockerfile from the current directory
`docker build -t python-helloworld .`

To build an image using the Dockerfile from the 'lesson1/python-app' directory
`docker build -t python-helloworld lesson1/python-app`

To run the `python-helloworld` image, in detached mode and expose it on port `5111`
`docker run -d -p 5111:5000 python-helloworld`

To retrieve the Docker container logs use the docker logs {{ CONTAINER_ID }} command. For example: `docker logs 95173091eb5e`

### Docker Registry

A central mechanism to store and distribute Docker images. The image needs to be pushed to a public Docker image registry, such as DockerHub (so other engineers have access to it).

Before pushing an image to a Docker registry, it is highly recommended to tag it first. During the build stage, if a tag is not provided (via the -t or --tag flag)

format - `docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`
example - `docker tag python-helloworld pixelpotato/python-helloworld:v1.0.0`

Once the image is tagged, the final step is to push the image to a registry.
`docker push pixelpotato/python-helloworld:v1.0.0`

Resources 
- [Best practies for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) 
- Docker notes - <https://github.com/mguery/docker>



# Lesson 4: Open Source PaaS



# Lesson 5: CI/CD with Cloud Native Tooling



