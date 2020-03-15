
# Core-concepts

## Member

member is an entity in the distributed environment.  
in the eco-system of scalecube, member will be a microservice container.

## Distributed environment

Distributed environment is collection of members that share services between them.  
Each member have access to all the services that are shared in the distributed environment.

### possible [topologies:](https://en.wikipedia.org/wiki/Network_topology)
* Star - when all microservice container are connected to the same microservice [seed](#seed).
* Hybrid - most of the microservices instance act as [seed](#seed).


## Registry

Registry store all [endpoints](#endpoint) of the available services it can request.    
the registry reflect the state in the [distributed environment](#distributed-environment).  
if the state has changed, then the registry is notified and update accordingly.

## ServiceCall

ServiceCall is the process of requesting a service to execute.

```typescript
Scenario: microservice use its own service (LocalCall)
Given     a service (definition + reference)
And       bootstraping a microservice container with the service
When      using the service definition to create a proxy from the microservice container
And       calling a method from the service
Then      the microservice container will perform localCall inorder to execute the method


Scenario: microservice use other microservice service (RemoteCall)
Given     a service (definition + reference)
And       bootstraping a microservice container A with the service
And       bootstraping a microservice container B without the service
When      using the service definition to create a proxy from the microservice container B
And       calling a method from the service
Then      the microservice container B will perform remoteCall to microservice container A inorder to request excution of the method
And       the microservice container A will perform localCall inorder to execute the method
```

```javascript
// main.js
import { createMicroservice } from '@scalecube/scalecube-microservice';

const msA = createMicroservice({ address: 'msA', services:[{ /* add your services */ }]});
const msB = createMicroservice({ address: 'msB', seedAddress : 'msA', services:[{ /* add your services */ }]});

const proxyA = msA.createProxy({ /* create proxy to the services on the microservice instance */ });
const proxyB = msB.createProxy({ /* create proxy to the services on the other microservice instance */ });
```

### LocalCall

When a microservice use its own services.

### RemoteCall

When a microservice use another microservice's services.

## Seed
the seed is a microservice container that used as an entry-point to the [distributed environment](#distributed-environment).
each microservice can act as a seed for other microservice containers.

seedAddress is the [address](#address) of the [seed](#seed).
