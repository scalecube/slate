
# Core-concepts

## Member

member is a microservice instances.

## Distributed environment

Distributed environment is collection of members that share services between them.

Each member have access to all the services that are shared in the distributed environment.

### environment [topology](https://en.wikipedia.org/wiki/Network_topology)
* Star - when all microservice container are connected to the same microservice [seed](#seed).
* Hybrid - most of the microservices instance act as [seed](#seed).


## Registry

Registry store all [endpoints](#endpoint) of the available services it can request.  
when a microservice bootstrap with a new service, it is connect via the [discovery](#discovery) to the [microservice seed](#seed),   
once connection is established, then they share their Endpoints.  
every change in the microservices in the [distributed environment](#distributed-environment) the registry is notified and update accordingly.

## LocalCall

```javascript
// main.js
import { createMicroservice, stringToAddress } from '@scalecube/scalecube-microservice';

const localMs = createMicroservice({
  services:[{
    // add your services
  }]
});

const proxy = localMs.createProxy({
 // create proxy to the services on the microservice instance
});
```

When a microservice use it own services.

### use-case

#### monolith application
starting building the application as monolith (in this step all services are local)  
change the application to microservice architecture when you are ready to scale-up.

#### gateway
when your microservice also act as a gateway,
some of the requests will be from the local services and some will require propagate the request to another microservice instance.

## RemoteCall

```javascript
// seed.js
import { createMicroservice, stringToAddress } from '@scalecube/scalecube-microservice';

createMicroservice({
  address: stringToAddress('seed'),
  services:[{
    // add your services
  }]
});

// main.js
import { createMicroservice, stringToAddress } from '@scalecube/scalecube-microservice';

const localMs = createMicroservice({
  address: stringToAddress('local'),
  seedAddress : stringToAddress('seed'),
  services:[{
    // add your services
  }]
});

const proxy = localMs.createProxy({
 // create proxy to the services on the other microservice instance
});
```
When a microservice use another microservice's services.

### steps:

1. use seedAddress to connect the microservice to the right distributed environment.
2. bootstrap the microservice with the seedAddress
3. createProxy to the service located on the other microservice instance.



## SeedAddress

seedAddress is the [address](#address) of the [seed](#seed).

## Seed
the seed is a microservice container that used as an entry-point to the [distributed environment](#distributed-environment).
each microservice can act as a seed for other microservice containers.

