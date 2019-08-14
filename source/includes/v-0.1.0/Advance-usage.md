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

remoteCall is a request for a service that located in different microservice container in our [distributed environment](#distributed-environment)

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

## gateway

