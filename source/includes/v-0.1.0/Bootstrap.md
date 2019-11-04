# Bootstrap

bootstrap the microservice can be done in 3 steps:

## 1. define the service
 

```typescript
interface ServiceDefinition {
  serviceName: string;
  methods: {
    [methodName: string]: {
      asyncModel: AsyncModel;
    };
  };
}
```


```javascript
import { ASYNC_MODEL_TYPES } from '@scalecube/scalecube-microservice';

const serviceDefinition = {
  serviceName: 'Service',
  methods:{
  someMethod : {
    asyncModel : ASYNC_MODEL_TYPES.RequestResponse
    }
  } 
}
```

[service](#service) definition is the contract between the provider to the consumer of the service.  
in other words, it is the contract that the service must uphold.

service definition is used when we are bootstrapping a service
and when we are creating a proxy to a service.

when we are boostraping the service we are binding the serviceReference with the serviceDefinition.
when we are creating a proxy with a serviceDefinition then scalecube search for the serviceReference that is bound to the serviceDefinition. 

 
## 2. create the serviceReference

```javascript
// class example
class Service {
  constructor(){
    
  }
  
  someMethod(){
    return Promise.resolve('some method been resolved');
  }
}
// module example
const someMethod = () => Promise.resolve('some method been resolved');

```

```typescript
type ServiceReference = ServiceFactory | ServiceObject;

type ServiceFactory = ({ createProxy, createServiceCall }: ServiceFactoryOptions) => ServiceObject;

interface ServiceObject {
  constructor?: any;

  [methodName: string]: any;
}

interface ServiceFactoryOptions {
  createProxy: CreateProxy;
  createServiceCall: CreateServiceCall;
}
```
ServiceReference is the implementation of the contract.

It can be a class instance, module or a callback function.

passing callback function in the ServiceReference call [depedency hook](#dependency-hook) 
and it can be used to inject proxy/service call to the service.

it is possible that the service will contain more functionality then what you define in the contract,  
but only the functions that are in the definition will be public (accessible) in the [distributed environment](#distributed-environment).

## 3. creating microservice

```javascript
import { createMicroservice } from '@scalecube/scalecube-microservice';

const ms = createMicroservice({
             services : [{
               reference : {someMethod},
               definition: serviceDefinition
             },
             {
                reference : new Service(),
                definition: serviceDefinition
              }]
           });
```

```typescript
type CreateMicroservice = (options: MicroserviceOptions) => Microservice;

interface MicroserviceOptions {
  services?: Service[];
  seedAddress?: Address | string;
  address?: Address | string;
  transport?: Transport;
  cluster?: (opt: ClusterOptions) => Cluster;
  debug?: boolean;
 }

interface Service {
  definition: ServiceDefinition;
  reference: ServiceReference;
}
```
After creating a service and a service-definition we can now bootstrap our microservice.   

* services - An array of services, that will exist inside a microservice container.
* seedAddress - [seedAddress](#seedaddress) is the entry point to our distributed env.
* address - An [address](#address) for this microservice instance.
* transport - optional pluggable [transport](#transport).
* cluster - optional pluggable [cluster](#cluster).
* debug - optional logs.

