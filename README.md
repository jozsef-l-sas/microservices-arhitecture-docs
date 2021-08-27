# Microservices Arhitecture

## Definition
"The microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and
communicating with lightweight mechanisms."
James Lewis and Martin Fowler, Thoughtworks

Microservices are small, autonomous services that work together with other microservices. They are independently deployable and scalable, represent a small
bounded context and are autonomously developed.

## Comparison with monolithic arhitecture
- pros
	- much simpler to scale
	- faster onboarding of new team members
	- higher autonomy per microservice
	- code easier to understand
	- more emerging technologies available
	- much more easier to adopt new technologies
	- can use multiple programming languages
	- can adopt and use multiple data storage solutions
- cons
	- harder to build, test and deploy

## Design Patterns
- individually deployable to be considered microservices (containers preferred)
- service discovery (registry) to find where another microservice is
- service discovery alternative - built-in DNS in PaaS (Kubernetes), auto-configured LB in front of each microservice instances group
- API Gateway (facade) to provide a public interface to the client app (more flexible)
- API Gateway solutions (Kong, Spring Cloud Gateway, Netflix Zuul, NGINX)
- microservice template (unified logging, config, secrets, etc.)
- 4 pillars of reactive architectures https://www.reactivemanifesto.org/ (responsive, resilient, elastic, message driven)
- centralized logging and distributed tracing
- event sourcing and global event store (pub/sub) that gives ability to replay events due to failure (elasticity)
- distributed transactions (choreographed or orchestrated SAGAs)
- when horizontal scalability is not possible (costs, cloud provider limitations, etc.) to avoid downtime you can implement throttling at API Gateway
(better to serve a max number of clients than none) but keep in mind a fair usage policy per client or user
- Security Proxy Service (Sidecar Pattern or Service Mesh), early days though - see Istio for Kubernetes environments
- BFF, backends for frontends, separate API contracts or gateways for each type of consumer (web, mobile)

## Communication
- synchronous (HTTP)
- asynchronous (event bus, preferred, decoupling, better scaling options, more resilient)

## DevOps
- the 5 essentials of continuous deployment
  - build phase (compile + unit tests for each branch/commit)
  - test phase (automated, integration, smoke, post-deployment tests)
  - deployment phase (automated)
  - provisioning (infrastructure as code)
  - monitoring (identify and respond quickly to issues related to code or infrastructure)
- deployment patterns
	 - multiple instances per host (fast deployment and efficient resource utilization but no service isolation, resource contention, software conflicts, 
	difficult monitoring)
	 - instance per VM (isolation, dedicated resources, easier monitoring and scaling but less efficient resource utilization, building images takes a lot of time)
	 - instance per container (very fast to build and run, isolation, highly scalable, infrastructure as code but lack of mature infrastructure, not as secure,
	less efficient resource utilization)
	 - serverless deployment/functions (no infrastructure, highly scalable, easy to deploy and monitor but technical constrains, slow start up time, higher latency)

## Resilience
- retry with back off, circuit breaker, bulkhead (separation by criticality), caching, etc. for synchronous communication
- message retries and idempotent operations (same end state) for asynchronous communication

## Monitoring
- health endpoint pattern (monitor by submitting a request to a configurable set of endpoints, and evaluating the results against a set of configurable rules)
- reliable audit (when, where, who, what) with irregular pattern recognition/alerts in logs/events
- check OWASP logging cheat sheet https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
- reasons for monitoring (auto scaling, exception tracing, security, health checks)
- be selective about what to monitor
  - app metrics (req/s, latency, duration, business specific)
  - platform metrics (CPU, memory usage, network latency, DB query execution time)
  - system metrics (deployments, infrastructure changes, other system events)
- centralized logging is critical for easy monitoring (AWS Cloud Watch, Application Insights, ELK, Graylog)
- log tracing for multiple microservices calls

## Security
- encryption in transit and rest, private network, identity server for authentication and authorization, IP restriction, do pen tests
- zero trust, defense in depth (multiple layers of security), principle of least privilege in mind
- solution should NOT
  - be for a monolith
  - prevent easy scalability and deployment
  - downgrade performance, stifle productivity, restrict technology stack
- microservices inside a private network that has a single point of entry (the API Gateway) but also secure the communication between microservices
- internal microservices security solutions
  - mutual TLS with internal CA and short lived certificates, see Metatron/Spire and Netflix Lemur
  - token exchange (exchange received token for a new token necessary for accessing downstream services) - higher overhead
- do not forget about authorization, make it as a service if possible (centralized, easier to maintain and control, better decoupling)
- do not enable global CORS for all domains, be specific
- secret management and bootstrapping, short lived, rotated, encrypted, least privilege
- token revokation techniques
- do not store token/secrets client side (token should be opaque, encrypted or contain minimal info)
- keep token lifespan as short as possible, especially in the case of offline token validation (signature), this way revocation or outdated claims are better
handled
- use refresh tokens on server-side to handle short lived tokens
- robust security test suite for regressions
- static code analysers for security checks
- frequent patches/updates
- keep in mind container security (see Clair for static analysis and docker-bench-security)
- identify the actors/roles, do risk assessment (impact and likelihood) and prioritize

## Transition from a monolith:
- facade microservices (contracts/interfaces) to actual microservices
- strangler application pattern
  - systematic and incremental steps
  - identify interaction points (endpoints, 3rd party APIs/vendors, external queues, etc.), group them in capabilities and transform them in microservices
  - overall stages
    - transform
    - coexist (route to new microservice with ability to rollback)
    - release strategy (incrementaly release and deploy when ready)
    - eliminate (on successful evaluation)
    - rinse and repeat (until monolith is at the desired size)
- feature toggle pattern for rolling back to monolith functionality during coexist stage
- identify bounded contexts (groups of core domain concepts and coresponding sub-concepts and interaction points, API contracts, UI sections)
- bounded contexts can be moved to a microservice while keeping the same interaction points or contract
- prioritize bounded contexts (based on low usage, simple to refactor, minimal risk, frequently changed to reduce monolith releases, good test coverage, smaller to reduce work,
larger to simplify integration)
- for missing dependencies/logic from the monolith use 'branch by abstraction' pattern that provides a hollow implementation (interface) for code that's missing
- for data use 'shared database by abstraction' pattern via views and SPs in which the monolith data source is shared via abstract contracts with the microservice 
only while coexisting for easy rollback/switch
- easier to decompose the logic first and data after that
- sometimes you need to modify the monolith to expose hidden functionality needed in the new microservice
- service to service calls introduce temporal coupling which can be fixed by async calls, or message queues or caching when sync call is needed
- apply the same 'branch by abstraction' pattern for using the new microservice from monolith logic
- check canary deployments for gradually switching to the new microservice, minimizing the risk
- compare with monolith functionality by comparing logs, monitoring, reports
- decoupling logic into microservices will needs extra error handling and can introduce latency, timeout and network load issues
- most complicated and challenging step would be separating the database
  - may break relationships and JOINs
  - may break existing reports
  - may have NoSQL on the new microservice
  - may need to migrate triggers to application logic instead
  - may needs retraining staff for distributed data
  - may need to rewrite or move DB business logic (SPs, functions)
  - data consistency, transactions, security
  - impact on performance once data is not in one place
- alternative solutions to database separation (partial separation of new tables, sync or write to both instead, DB access via a separate service used in both 
places but requires refactoring)
