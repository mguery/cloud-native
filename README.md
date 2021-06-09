# Cloud Native

**Learning how to construct a CI/CD pipeline that will containerize an application and deployed it to a Kubernetes cluster.**

Notes from [Cloud Native Foundations](https://www.udacity.com/scholarships/suse-cloud-native-foundations-scholarship)

Projects include Python, Docker, Kubernetes, the CLI, Git 

# Lesson 1

## Intro to Cloud-Native



**Cloud native** a set of practices that empowers an organization to build and manage applications at scale. 

**Containers** Small units of an app. Easy to manage, deploy, and fast to recover. Fits well with a microservice-based architecture

**Microservices** are used to manage and configure a collection of small, independent services that can be easily packaged and executed within a container.

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

# Lesson 2

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
 **operational cost** - reps the cost of resources to release product | low cost but cost increases when app needs to operate at scale | high cost but the costs based on consumed resources |  resources necessary throughout prj dev'mt and their cost
**reliability** - how app quickly recovers from failure and tools to monitor an app | entire needs to be recovered, logs and metrics are mixed together | only failed unit is recovered, with logs an metrics for that unit |  respond to failure based on logs and metrics

# Lesson 3

# Lesson 4

# Lesson 5

