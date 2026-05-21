# spring-config-app

Use this skill when the user needs to generate, review, refactor, validate, or explain Spring Boot configuration files (`application.yaml` or `application.properties`) for modern, production-grade applications.

## Metadata
- Skill name: `spring-config-app`
- Category: `backend`, `spring`, `configuration`, `devops`
- Target stack: Spring Boot `3+`, Java `17+`
- Primary goal: Help generate secure, maintainable, scalable, and environment-aware Spring Boot configuration structures using modern best practices.

## Triggers
Activate this skill when user requests include any of:

- spring config
- spring boot configuration
- application yaml
- application properties
- spring profiles
- spring environment variables
- spring secrets
- configure spring boot
- production spring config
- secure spring configuration
- docker spring config
- kubernetes spring config
- spring cloud config
- spring datasource config
- spring logging config
- spring actuator config
- spring boot best practices
- env variables spring
- spring boot profiles
- spring externalized configuration

Also activate when user asks about:
- Spring Boot configuration structure
- environment setup (dev/test/staging/prod)
- profile separation or overrides
- secrets management and secure config patterns
- YAML/properties organization
- production readiness of configuration
- deployment-oriented configuration (containers/cloud/CI)

## Responsibilities
When active, this skill must enforce and teach best practices for:
1. Configuration organization
2. Profile separation
3. Environment-specific overrides
4. Secure secret management
5. Environment variable usage
6. Production-safe defaults
7. Cloud/container compatibility
8. Observability and logging configuration
9. Database configuration
10. Externalized configuration
11. Configuration validation
12. Maintainability and readability

## Operating Principles
- Default to `application.yaml` unless there is a clear reason to use `.properties`.
- Explain trade-offs when recommending `.properties` (legacy tooling compatibility, flat/simple configs, strict key-value diff ergonomics).
- Prefer clear hierarchical YAML and avoid duplicated blocks.
- Keep examples practical, environment-aware, and aligned with Spring Boot 3 conventions.
- Use explicit, safe defaults; never embed real secrets.
- Treat config as deployable architecture, not just key-value snippets.

## Required Outputs
For generation/refactor tasks, normally produce:
- `application.yaml` (shared defaults)
- `application-dev.yaml`
- `application-test.yaml`
- `application-staging.yaml` (when staging exists)
- `application-prod.yaml`

When relevant, include companion guidance for:
- containerized deployment (Docker, Compose)
- cloud-native deployment (Kubernetes, managed services)
- CI/CD pipelines and ephemeral environments

## Spring Profile and Loading Guidance
Always explain and correctly apply:
- profile activation (`spring.profiles.active`, CLI args, env vars)
- `spring.config.activate.on-profile` sections
- override precedence (command-line > env vars > profile files > base file)
- config loading order and external config locations
- inheritance pattern via shared base + targeted overrides

Prefer this model:
- put common non-sensitive defaults in base `application.yaml`
- override only changed values in profile files
- keep prod explicit and minimal, avoiding dev/test leakage

## Secrets Management Policy (Strict)
The assistant must explicitly discourage:
- hardcoded passwords
- committed secrets
- API keys in repositories
- plaintext production credentials

The assistant must recommend, in priority order as applicable:
- environment variables
- Docker secrets
- Kubernetes Secrets
- Vault systems (e.g., HashiCorp Vault)
- cloud secret managers (AWS Secrets Manager, GCP Secret Manager, Azure Key Vault)
- external secret providers/controllers

Always demonstrate secret references as placeholders, for example:
- `${DB_PASSWORD}`
- `${JWT_SECRET}`
- `${API_KEY}`

Must explain:
- safe vs sensitive configuration separation
- fallback values (`${VAR:default}`) and when defaults are unsafe
- secure fallback strategies (fail-fast in prod, safe defaults only for non-sensitive local dev)
- secret rotation implications and short credential lifetime
- principle of least privilege for DB/users/API scopes

Never output real credentials. If user supplies secrets, recommend rotation and removal from VCS history where appropriate.

## Environment Variables Guidance
Enforce env var best practices:
- uppercase snake case (e.g., `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`)
- consistent naming across local, CI, containers, and cloud
- avoid ambiguous names and collisions
- portability across OS/shells and runtime platforms
- compatibility with Docker/Kubernetes injection methods

Use patterns like:

```yaml
spring:
  datasource:
    url: ${DB_URL}
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
```

## Environment-Specific Best Practices

### Development
- prioritize fast feedback and local tooling compatibility
- allow non-sensitive local defaults where safe
- enable useful debug logging without noisy production settings

### Testing
- isolate from production dependencies
- prefer ephemeral/in-memory services where practical
- keep deterministic settings for reproducible tests

### Staging
- keep close parity with production topology
- use realistic integrations with non-production secrets/data
- enable pre-prod observability and release validation

### Production
- fail fast on missing critical secrets/config
- minimize verbose logs and disable debug features
- enforce secure transport, hardened actuator exposure, and conservative resource settings

### Containerized Deployments
- externalize all environment-specific values
- avoid baking secrets into images
- support health probes and graceful shutdown settings

### Cloud-Native Deployments
- support config via ConfigMaps/Secrets/parameter stores
- design for immutable infrastructure and rolling deploys
- align readiness/liveness and telemetry configuration with platform norms

### CI/CD Environments
- inject secrets at runtime from secure stores
- use profile-based or branch-based config selection
- validate configuration as part of pipeline gates

## Observability and Operations
When relevant, include modern Spring Boot guidance for:
- logging levels by package/profile
- structured logging patterns for production
- actuator endpoint exposure and security
- metrics/tracing integration points
- HTTP/server tuning and graceful shutdown

## Database Configuration Guidance
Cover:
- datasource configuration via env vars
- pool sizing and timeout tuning by environment
- migration tooling alignment (Flyway/Liquibase)
- SSL/TLS and certificate settings in production/cloud contexts
- read/write role separation where applicable

## Externalized Configuration
Recommend externalized config using:
- environment variables
- mounted files/secrets
- Spring Cloud Config (when architecture justifies it)
- platform-native configuration providers

Explain when central config services are useful vs over-engineering.

## Validation and Quality Checks
For review/refactor tasks, validate:
- no secrets committed
- no duplicated or conflicting keys
- profile overrides are intentional and minimal
- naming consistency and readability
- production safety defaults
- deploy target compatibility (Docker/K8s/cloud/CI)

Encourage optional safeguards:
- `@ConfigurationProperties` + Bean Validation
- startup fail-fast for required settings
- config contract documentation for teams

## Response Style for This Skill
When responding with this skill active:
- start with a recommended configuration strategy
- provide concrete file structures and examples
- explain why each critical setting belongs in base vs profile override
- call out security implications explicitly
- include migration notes when refactoring older Boot 2.x style config

Keep examples production-minded, concise, and ready to adapt.

## Reusable Starter Template
Use and adapt this baseline when user asks for a starting point:

```yaml
# application.yaml
spring:
  application:
    name: my-service
  profiles:
    active: ${SPRING_PROFILES_ACTIVE:dev}
  datasource:
    url: ${DB_URL:jdbc:postgresql://localhost:5432/app}
    username: ${DB_USERNAME:app}
    password: ${DB_PASSWORD:}
    hikari:
      maximum-pool-size: ${DB_POOL_MAX_SIZE:10}
  jackson:
    serialization:
      write-dates-as-timestamps: false

server:
  port: ${SERVER_PORT:8080}
  shutdown: graceful

management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
  endpoint:
    health:
      probes:
        enabled: true

logging:
  level:
    root: ${ROOT_LOG_LEVEL:INFO}
```

```yaml
# application-prod.yaml
spring:
  config:
    activate:
      on-profile: prod
  datasource:
    url: ${DB_URL}
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}

management:
  endpoints:
    web:
      exposure:
        include: health,info,prometheus

logging:
  level:
    root: INFO
```

In production guidance, prefer required env vars without insecure defaults for secrets.
