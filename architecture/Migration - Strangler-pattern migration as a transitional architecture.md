# Migration - Strangler-pattern migration as a transitional architecture

## Table of Contents

- [Migration - Strangler-pattern migration as a transitional architecture](#migration---strangler-pattern-migration-as-a-transitional-architecture)
  - [Table of Contents](#table-of-contents)
  - [Strangler-pattern migration](#strangler-pattern-migration)
    - [Cases where Apollo Federation + Apigee + legacy SOA actually makes a lot of sense](#cases-where-apollo-federation--apigee--legacy-soa-actually-makes-a-lot-of-sense)
    - [Cases where it’s a bad idea](#cases-where-its-a-bad-idea)
    - [Practical patterns that work well](#practical-patterns-that-work-well)
    - [Bottom line](#bottom-line)
  - [References](#references)

## Strangler-pattern migration

### Cases where Apollo Federation + Apigee + legacy SOA actually makes a lot of sense

1. **You’re doing a strangler-pattern migration**  
   You have dozens or hundreds of SOAP/REST services behind Apigee (some proxied to SOA/ESB, some already RESTified). You don’t want to touch Apigee or the backend services yet, but you need to start delivering a unified, consumer-friendly GraphQL API to your frontend teams *now*.  
   → Apollo Federation lets you compose a graph out of multiple subgraphs. One (or many) of those subgraphs can be an “Apigee Gateway subgraph” that simply calls the existing Apigee proxies (which in turn call the SOA endpoints). You get immediate value: one GraphQL endpoint, proper typing, client-specific fields, auth handled once, etc., while the heavy legacy stuff stays untouched.

2. **You have multiple teams and bounded contexts**  
   Federation shines when different teams own different domains. Some teams may already have modern microservices with their own GraphQL gateways. Other domains are still stuck behind Apigee/SOA. Federation lets everyone play in the same graph without forcing the legacy teams to rewrite anything today.

3. **You need to reduce client coupling and over-fetching fast**  
   Mobile/web apps hitting 10–15 Apigee endpoints per screen → huge latency, battery drain, complex client code.  
   A federated graph (even if one subgraph is just a thin wrapper around Apigee calls) collapses that into 1 round-trip and exactly the fields the client needs.

4. **Apigee is your edge/security layer and you’re not getting rid of it soon**  
   Many large enterprises are stuck with Apigee for years because of OAuth, rate-limiting, monetization, developer portal, etc. Using Apollo Federation in front of it doesn’t fight that reality — it embraces it.

### Cases where it’s a bad idea

1. **You think GraphQL magically modernizes the backend**  
   It doesn’t. If your SOA services still take 800 ms each and you now call three of them sequentially inside a resolver, your GraphQL response time is 2.4 s+. Federation doesn’t fix latency or bad contracts — it can actually hide the smell for a while and make it worse.

2. **You’re adding an extra network hop for no reason**  
   Client → Apollo Gateway → Apigee subgraph → Apigee edge → SOA  
   That’s two extra hops compared to Client → Apigee → SOA. If you’re not getting real composition benefits (i.e., you only have one domain), you’re just making things slower and more complex.

3. **You’re planning to migrate away from Apigee entirely in <12 months**  
   Then building a proper federated subgraph that talks to Apigee is throwing away work. Better to start replacing the Apigee proxies with real GraphQL services or REST-for-now services.

4. **Your Apigee proxies are already very coarse-grained and chatty**  
   GraphQL encourages fine-grained field selection, but if the only way to get one field is a SOAP call that returns 10 MB of XML, you’re going to have a bad time.

### Practical patterns that work well

- Build an “Apigee Adapter” subgraph (Node.js or Java with Apollo Subgraph library) that:
  - Reuses the same OAuth tokens passed from the gateway (propagate JWT)
  - Calls Apigee proxies over internal network (fast)
  - Transforms responses into clean GraphQL types
  - Adds `@requires` or `@provides` only where necessary
- Put the Apollo Router/Gateway in the same VPC as Apigee to minimize latency
- Incrementally move entities out of the Apigee subgraph into real owned subgraphs as you modernize domains

### Bottom line

> “Using Apollo Federation to front Apigee + SOA endpoints doesn’t make sense”  
→ is too absolute.

It makes **perfect sense as a transitional architecture** in a large enterprise doing incremental modernization. It makes **zero sense** if you’re looking for a quick cosmetic rewrite or believe GraphQL alone will fix your backend performance/contract issues.

Used pragmatically in a strangler pattern, it’s one of the most effective ways I’ve seen big banks and telcos start delivering real developer experience wins without boiling the ocean on the backend. Your mileage obviously varies by how entangled and slow the SOA layer really is.

## References
