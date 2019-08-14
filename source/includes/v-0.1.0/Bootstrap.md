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

service definition is the contract between the provider to the consumer of the service.  
in other words, it is the contract that the service must uphold.

  
it is possible that the service will contain more functionality then what you define in the contract,  
but only the functions that are in the definition will be accessible via scalecube.

* serviceName - The name of a service
* methods - Map of methods that exist in the service.  
 each method describe its [asyncModel](#asyncmodel).
 
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
ServiceReference can be a class instance, module or a callback function.

passing callback function in the ServiceReference call [depedency hook](#dependency-hook) 
and it can be used to inject proxy/service call to the service.

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

