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

## Kubernetes Resources
Kubernetes cluster is composed of a collection of distributed physical or virtual servers. These are called nodes. Nodes are categorized into 2 main types: master and worker nodes. 

The suite of master nodes, represents the control plane, while the collection of worker nodes constructs the data plane.

The control plane consists of components that make global decisions about the cluster. These components are the:

- **kube-apiserver** - is used to exposes the Kubernetes API, and handles and triggers any operations within the cluster
- **kube-scheduler** -  is the mechanism that will place the new workloads on a node with sufficient capacity
- **kube-controller-manager** -  is the component that handles controller processes and ensures that the desired configuration is propagated to resources
- **etcd** - is the key-value store that is utilized to backs-up the cluster

Two additional components on the control plane: 
- **kubelet** - the agent that runs on every node and notifies the kube- apiserver that this node is part of the cluster
- **kube-proxy** - a network proxy that ensures the reachability and accessibility of workloads places on this specific node

The data plane is used to host workloads onto the cluster. Those 2 components are installed on a node to mark it as part of the data plane in a Kubernetes cluster.

These components are installed on all the nodes in the cluster (master and worker nodes). These components keep the kube-apiserver up-to-date with a list of nodes in the cluster and manages the connectivity and reachability of the workloads.


New terms
- **CRD** - Custom Resource Definition provides the ability to extend Kubernetes API and create new resources 
- **Node** - a physical or virtual server
- **Cluster** - a collection of distributed nodes that are used to manage and host workloads
- **Master node** - a node from the Kubernetes control plane, that has installed components to make global, cluster-level decisions
- **Worker node** - a node from the Kubernetes data plane, that has installed components to host workloads
- **Bootstrap** - the process of provisioning a Kubernetes cluster, by ensuring that each node has the necessary components to be fully operational


Bootstrap tools can be used to provision production-grade clusters - kubeadm, Kubespray, Kops, K3s
Developmnet-grade clusters - kind, minikube, k3d


To access a Kubernetes cluster a **kubeconfig** file is required. A kubeconfig file has all the necessary cluster metadata and authentication details, that grants the user permission to query the cluster objects.

A Kubeconfig file has 3 main distinct sections: 
* **Cluster** encapsulates the metadata for a cluster, such as the name of the cluster, API server endpoint, and certificate authority used to check the identity of the user.
* **User** contains the user details that want access to the cluster, including the user name, and any authentication metadata, such as username, password, token or client, and key certificates.
* **Context** links a user to a cluster. If the user credentials are valid and the cluster is up, access to resources is granted. Also, a current-context can be specified, which instructs which context (cluster and user) should be used to query the cluster.

Kubernetes resources
Application Deployment
**Pods** 
- the anatomic element within a cluster to manage an application. Pods are the smallest manageable units in a Kubernetes cluster. Every pod has a container within it, that executes an application from a Docker image (or any OCI-compliant image). 
**Deployments & ReplicaSets** 
- oversees a set of pods for the same application. To deploy an application to a Kubernetes cluster, a Deployment resource is necessary. 
The Deployment resource comes with a very powerful rolling out strategy, which ensures that no downtime is encountered when a new version of the application is released. There are 2 rolling out strategies:
  - **RollingUpdate** - updates the pods in a rolling out fashion (e.g. 1-by-1)
  - **Recreate** - kills all existing pods before new ones are created
- A Deployment contains the specifications that describe the desired state of the application. Deployment resource manages pods by using a ReplicaSet. 
- A ReplicaSet resource ensures that the desired amount of replicas for an application are up and running at all times. To create a deployment, use the `kubectl create deploy NAME --image=image [FLAGS] -- [COMMAND] [args]` command. Example: `kubectl create deploy go-helloworld --image=pixelpotato/go-helloworld:v1.0.0 -n test`

Application Reachability
**Services & Ingress** 
- ensures connectivity and reachability to pods. A Service resource provides an abstraction layer over a collection of pods running an application. A Service is allocated a cluster IP, that can be used to transfer the traffic to any available pods for an application. 
- There are 3 widely used Service types: 
  - ClusterIP - exposes the service using an internal cluster IP, 
  - NodePort - expose the service using a port exposed on all nodes in the cluster, 
  - LoadBalancer - exposes the service through a load balancer from a public cloud provider and allows external traffic to reach the services within the cluster securely. To create a service for an existing deployment, use the `kubectl expose deploy NAME --port=port [--target-port=port] [FLAGS]` command. Example: `kubectl expose deploy go-helloworld --port=8111 --target-port=6112`
- To enable the external user to access services within the cluster an Ingress resource is necessary. An Ingress exposes HTTP and HTTPS routes to services within the cluster, using a load balancer provisioned by a cloud provider. To keep the Ingress rules and load balancer up-to-date an Ingress Controller is introduced.

Application Configuration And Context
**Configmaps & Secrets** 
- pass configuration to pods. objects that store non-confidential data in key-value pairs. A Configmap can be consumed by a pod as an environmental variable, configuration files through a volume mount, or as command-line arguments to the container. 
- To create a Configmap use the `kubectl create configmap` command. Flags: --from-file - set path to file with key-value pairs and --from-literal - set key-value pair from command-line. Example: `kubectl create configmap test-cm --from-literal=color=yellow`
- Secrets are used to store and distribute sensitive data to the pods, such as passwords or tokens. Pods can consume secrets as environment variables or as files from the volume mounts to the pod. K8s will encode the secret values using base64. To create a Secret use the `kubectl create secret generic` command. Example: `kubectl create secret generic test-secret --from-literal=color=blue`
**Namespaces** 
- provides a logical separation between multiple applications and their resources
- To create a Namespace, use the `kubectl create ns` command. Example: `kubectl create ns test-udacity`

Kubectl provides a rich set of actions that can be used to interact, manage, and configure Kubernetes resources. Below is a list of handy kubectl commands used in practice.
- To create resources, use the following command: `kubectl create RESOURCE NAME [FLAGS]`
- To describe resources, use the following command: `kubectl describe RESOURCE NAME`
- To get resources, use the following command, where -o yaml instructs that the result should be YAML formatted. `kubectl get RESOURCE NAME [-o yaml]`
- To edit resources, use the following command, where -o yaml instructs that the edit should be YAML formated. `kubectl edit RESOURCE NAME [-o yaml]`
- Label Resources `kubectl label RESOURCE NAME [PARAMS]`
- Port-forward to Resources `kubectl port-forward RESOURCE/NAME [PARAMS]`
- To access logs from a resource `kubectl logs RESOURCE/NAME [FLAGS]`
- To delete resources `kubectl delete RESOURCE NAME`


---

## Declarative Kubernetes Manifests
Management techniques:
- imperative configuration - that operates and interacts directly with the live objects within the cluster, development, low learning curve
- declarative configuration - that operates and manages resources using YAML manifests stored locally, recommended for production, high learning curve 

YAML Manifest structure
- apiversion, kind (object type), metadata (info about object - name, namespace, labels), spec (defines desired config state of resource)
- use `kubectl get -o yaml` to get the YAML manifest of any resource within the cluster
- Kubernetes YAML manifests can be created using the `kubectl apply -f manifest.yaml` command
- To delete a resource `kubectl delete -f manifest.yaml`

Kubernetes provides methods to handle some of the low-level failures and ensure the application is healthy and accessible:
- ReplicaSets - make sure # replicas are up and running at all times
- Liveness probes - checks if pod is running, and restarts if it's errored state
- Readiness probes - makes sure traffic is routed to pods ready to handle reqs
- Services - provides 1 entry point to all the available pods of an app

Resources
- [K8s kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)



# Lesson 4: Open Source PaaS

- **Platform as a Service** - where the infrastructure is fully managed by a provider, and the team is focused on application deployment (Beanstalk for example)
- A platform consists of multiple services that need to be configured, wired, and maintained together: includes - ntwg, storage, servers, virtualization, O/S, middleware, runtime, data, apps / In PaaS, you manage apps and data
- Advantages - save time + focus on dev, scal + HA (on demand), rich app catalog (integrate external svc with minimal effort)
- Disadvantages - Vendor lock-in, Data security (when dealing with 3rd party svc, need an extra layer of sec for data confidentiality), Operational limitations (svc catalog limited to svcs from your cloud provider)

**Function as a Service**
- FaaS is an event-driven cloud-computing service that allows the execution of code without any management of the infrastructure and configuration files. Example - Lambda, Azure Functions, Cloud Functions
- This only requires the application code that is built and executed immediately. And has quicker usability rate, as no data management or configuration files are necessary



# Lesson 5: CI/CD with Cloud Native Tooling

A delivery pipeline includes stages that can test, validate, package, and push new features to a production environment. Is triggered when a new commit is available. In CI phase - Stages: build, test, package.

It is common practice to push an application through multiple environments before it reaches the end-users. In CD phase - Sandbox (dev env), Staging (identical to production w/o affecting the UX), Production (customer-facing env)

Advantages of a CI/CD pipeline - ship new code asap and receive customer feedback to make improvements, less risk since automation eliminates manual configuration, developer productivity - structured release process allows every product to be released independently of other components

New terms
**Continuous Integration**- includes the build, test, and package stages.
**Continuous Delivery** - handles the deploy stage.
**Continuous Deployment** - a procedure that contains the Continuous Integration and Continuous Delivery of a product.

## CI Fundamentals 
- Build - installs app dependencies `RUN pip install -r requirements.txt`
- Test - run test usng frameworks. Ex. testing for Python app - pytest, pylint
- Package - build image and pushing code to store in registry like DockerHub `docker build` `docker push`

GitHub Actions - build, test, and package an application. event-driven workflows that can be executed when a new commit is available, on external or scheduled events.

> A GitHub Action consists of one or more jobs. A job contains a sequence of steps that execute standalone commands, known as actions. When an event occurs, the GitHub Action is triggered and executes the sequence of commands to perform an operation, such as code build or test.
[Learn GH Actions](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions)


## CD Fundamentals

After automated the packaging of an app, next is to release to customers. You push the code through at least 3 environments: sandbox, staging, and production. Ex. ArgoCD, CircleCI, Jenkins

## Config Managers

When managing multiple manifests, it's necessary to introduce a mechanism to store and manage manifests in a reliable, scalable, and flexible way. 

**Helm** -  package manager that templates exiting manifests, and uses input files to tailor configuration for each environment / manages Kubernetes manifests through charts. 

A Helm chart is a collection of YAML files that describe the state of multiple Kubernetes resources. Chart.yaml - details of the chart, such as version, description, and maintainer list / templates/ folder containing templated YAML manifests / values.yaml - contains default input parameters for a Helm chart

