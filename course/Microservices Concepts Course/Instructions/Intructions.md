# Lesson 09: Data Consistency Patterns (Spring Boot Microservices)

## Instructions for Copilot
- Expand each bullet into 2–4 complete sentences.
- Use slides-style: concise, clear, beginner-friendly.
- Explain what the concept is and why it matters in microservices.
- Include short Spring Boot or pseudo-code examples where relevant.
- Keep bullets separate, do NOT merge multiple concepts.


# Copilot Instructions for Expanding Lesson 2: Service Discovery & Registration

## General Rules
- Expand each bullet point into **2–4 complete sentences**.
- Use **slides-style teaching**: concise, clear, beginner-friendly.
- Explain **what the concept is**, **why it matters in microservices**, and **how it works in Spring Boot**.
- Include **minimal Spring Boot code examples** where relevant.
    - Example: application.yml, @EnableEurekaClient, @RestController calls using DiscoveryClient.
- Do NOT merge multiple concepts into one bullet.
- Focus on **Eureka and Consul only** (no other discovery tools like Kubernetes DNS or NGINX).

## HTML Output Rules (if generating slides)
- Use `<h1>` for lesson title, `<h2>` for section headings.
- Use `<ul>` and `<li>` for bullets.
- Use `<pre>` blocks for code snippets.
- Each bullet should be a **full sentence inside a <li> tag**.

## Sections to Expand

### 1. Service Registration
- Definition: Process where a service announces its availability to a central registry.
- What it shares: service name, IP address, port, health status, metadata.
- When it happens: on startup, shutdown, and health status changes.
- Minimal Spring Boot example:
    - Add `@EnableEurekaClient` to your service class.
    - Configure `application.yml` with Eureka server URL and service name.

### 2. Service Discovery
- Definition: Process by which a service finds other services dynamically.
- Client-side discovery vs server-side discovery:
    - Client-side: service queries registry and chooses instance.
    - Server-side: registry or gateway routes requests automatically.
- Why it matters: removes hard-coded URLs, enables scaling and failover.
- Example: use `DiscoveryClient.getInstances("other-service")` to retrieve available instances.

### 3. Eureka Server Setup
- Explain how to create a Spring Boot project with Eureka Server dependency.
- Show minimal configuration in `application.yml`:
    - Enable service registration
    - Port configuration
- Mention dashboard access to see registered services.

### 4. Eureka Client Setup
- Add `@EnableEurekaClient` to a Spring Boot service.
- Configure service name and Eureka server URL in `application.yml`.
- Example endpoint calling another service via DiscoveryClient.

### Copilot Usage Instruction
- Prompt to Copilot:
  "Expand this lesson outline into **full teaching-style HTML slides** for Spring Boot microservices, using the rules in this instruction file."
- Generate **section by section** for clarity.
- Include **code snippets** and **practical examples** for each concept.
