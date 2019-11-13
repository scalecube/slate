# Advance usage

## remoteCall

```javascript
createMicroservice({
  services: [{
    definition: serviceDefinition,
    reference : new Service()
  }],
  address: 'A'
});

const localMs = createMicroservice({
  seedAddress: 'A'
});

const proxyA = localMs.createProxy({serviceDefinition});

proxyA.someMethod().then(console.log);
```

remoteCall is a request for a service that located in different microservice container in our [distributed environment](#distributed-environment),

the other microservice can be located on a different process and it will still be accessible with scalecube.

## dependency hook

```typescript
type ServiceFactory = ({ createProxy, createServiceCall }: ServiceFactoryOptions) => ServiceObject;

interface ServiceFactoryOptions {
  createProxy: CreateProxy;
  createServiceCall: CreateServiceCall;
}

interface ServiceObject {
  constructor?: any;

  [methodName: string]: any;
}
```

```javascript
import { createMicroservice, ASYNC_MODEL_TYPES } from '@scalecube/scalecube@scalecube/scalecube-microservice';
  const definitionA = {
    serviceName: 'serviceA',
    methods: {
      someMethodA: {
        asyncModel: ASYNC_MODEL_TYPES.REQUEST_RESPONSE,
      },
    },
  };

  const definitionB = {
    serviceName: 'serviceB',
    methods: {
      someMethodB: {
        asyncModel: ASYNC_MODEL_TYPES.REQUEST_RESPONSE,
      },
    },
  };

  class ServiceB {
    constructor(proxyA) {
      // work with proxy to serviceA
      proxyA.someMethodA().then(console.log);
    }
  }
  
createMicroservice({
  services: [
    {
      definition: definitionA,
      reference : { someMethodA}
    },
    {
      definition: definitionB,
      reference: ({ createProxy, createServiceCall }) => {
        const proxyA = createProxy({serviceDefinition: definitionA });
        
        return new ServiceB(proxyA);
      }
    }    
  ]
})
```

dependency hook is used if your service deepened on another service,  
or if you want to add life cycle to your bootstrap process.

### injectProxy example:

In the example we have two services with dependency between them,  
ServiceB deepened on ServiceA.

instead of passing the class instance of serviceB in the reference, we are passing a callback.

the callback is called from scalecube as part of the bootstrapping process.  
the callback will be called with createProxy or createServiceCall methods which allow us to create proxy to serviceA and inject it to ServiceB constructor.

this technique can be apply to modules by passing the proxy to a factory.


## gateway

```typescript
type RequestHandler = (serviceCall: ServiceCall, data: any, subscriber: any) => void;

interface GatewayOptions {
  port: number;
  requestResponse?: RequestHandler;
  requestStream?: RequestHandler;
}

interface GatewayStartOptions {
  serviceCall: ServiceCall;
}

interface Gateway {
  start: (options: GatewayStartOptions) => void;
  stop: () => void;
}
```

```javascript
import {Gateway} from '@scalecube/rsocket-ws-gateway';
import {createMicroservice} from '@scalecube/scalecube-microservice';

const gateway = new Gateway({port : 3000});
const serviceCall = createMicroservice({
  services: [{
    reference: new Service(),
    definition: serviceDefinition
  }]
}).createServiceCall({});

gateway.start({serviceCall});
```
Its a technique to centralize all the requests that are coming from outside of the distributed environment.

scalecube provide gateway implementation base on RSocketWebsocket.

* port - The gateway will listen to it.
* requestResponse - handle requestResponse requests.
* requestStream - handle requestStream requests.

* [serviceCall](#createservicecall) - low level approach for requesting a service in the distributed environment.

* start - start the gateway.
* stop - stop the gateway.
