# API

## Address
```typescript
interface Address {
  host: string;
  port: number;
  protocol: string;
  path: string;
}
```

Address is the URI for the service.

* host - unique identifier that allows other computers to access it.
* port - determine on which port number the server will receive the data.
* protocol - rules for communication between server and client (ws | pm | tcp)
* path - relative address.

## AsyncModel
```typescript

type RequestStreamAsyncModel = 'requestStream';

type RequestResponseAsyncModel = 'requestResponse';

type AsyncModel = RequestStreamAsyncModel | RequestResponseAsyncModel;
```

AsyncModel is the way a service can be resolved.  
It can be a stream and use `requestStream`
or can be a promise and use `requestResponse`

* RequestStreamAsyncModel - Defines Stream asyncModel ( Observable, Flowable , etc.. ).
* RequestResponseAsyncModel - Defines Async asyncModel ( Promise ).

## Cluster
```typescript
type JoinCluster = (options: ClusterOptions) => Cluster;

interface ClusterOptions {
  address: Address;
  seedAddress?: Address;
  itemsToPublish: any;
  retry?: {
    timeout: number;
  };
  debug?: boolean;
}

interface Cluster {
  getCurrentMembersData: () => Promise<MembersData>;
  listen$: () => Observable<ClusterEvent>;
  destroy: () => Promise<string>;
}
```

```javascript
// browser: 
import { joinCluster } from '@scalecube/cluster-browser';
// server:
import { joinCluster } from '@scalecube/cluster-nodejs';

const cluster = joinCluster({
  address: { /* my address */ },
  seedAddress: {/* address to the distributed environment */},
  itemsToPublish: [/* items to publish in the distributed environment */]
});

cluster.listen$().subscribe((response)=>{console.log(response)});
```
create a [member](#member) from the data it receive from the discovery,  
and use it in-order to share data in the distributed environment.

scalecube provide two cluster implementations:
* browser:  
import { joinCluster } from '@scalecube/cluster-browser';
* server:  
import { joinCluster } from '@scalecube/cluster-nodejs';

`@scalecube/cluster-browser` is the default cluster when using the IIFE version.

* address - address of the member
* seedAddress - address of the member that act as the seed
* itemsToPublish - item to share with the different members
* retry - retry configuration for connecting members
* debug - debug flag

* getCurrentMembersData - resolve with the current information of the other members in the distributed environment
* listen$ - subscribe to changes in the members state
* destroy - resolve when cluster is destroyed

## Discovery

```typescript
type CreateDiscovery = (options: DiscoveryOptions) => Discovery;

interface DiscoveryOptions {
  address: Address;
  itemsToPublish: Item[];
  seedAddress?: Address;
  cluster?: (opt: ClusterOptions) => Cluster;
  debug?: boolean;
}

interface Discovery {
  discoveredItems$: () => Observable<ServiceDiscoveryEvent>;
  destroy(): Promise<string>;
}

type Item = any;
```

Discovery is a tool that connect the microservice instance to the [distributed environment](#distributed-environment).  

It convert events received from the [distributed environment](#distributed-environment) to events that the registry can understand.

* address - A unique [address](#address).
* itemsToPublish - The data that the discovery need to share.
* seedAddress - the [address](#address) we want to use in-order to connect to the distributed environment.
* cluster - optional pluggable [cluster](#cluster)
* debug - discovery logs

* discoveredItems$ - An Observable sequence that describes all the items that published by **other** discoveries.  
Emits new array of all items each time new discovery is created or destroyed.
* destroy - Destroy the discovery:  
  - Completes discoveredItems$.  
  - Notifies other discoveries that this discovery's items are not available anymore.  
  - Resolves with the message, that specifies the [address](#address) of the discovery.

## Endpoint

```typescript
interface Endpoint {
  qualifier: string;
  serviceName: string;
  methodName: string;
  asyncModel: AsyncModel;
  address: Address;
}
```

Endpoint is the metadata of a service.  
Contain information of how to access the service.

* qualifier - The combination of serviceName and methodName: <serviceName/methodName>
* serviceName - The name of a service, that is provided in serviceDefinition.
* methodName - The name of a method, that is provided in the methods map in serviceDefinition.
* asyncModel - Type of communication between a consumer and a provider.
* address - A unique [address](#address) of an endpoint URI format: <protocol>://<host>:<port>/<path>

## LookUp

```typescript
type LookUp = (options: LookupOptions) => Endpoint[] | [];

interface LookupOptions {
  qualifier: string;
}
```

Search for [endPoints](#endpoint) in the registry that match the qualifier.

* qualifier - The combination of serviceName and methodName: <serviceName/methodName>

## Message

```typescript
interface Message {
  qualifier: string;
  data: any[];
}
```

```javascript
const message = {
  qualifier : 'Service/someMethod',
  data: ['value']
}
```

structure of the data in scalecube.

* qualifier - The combination of serviceName and methodName: <serviceName/methodName>
* data - Arguments of the invoked function.

## Microservice
```typescript
type CreateMicroservice = (options: MicroserviceOptions) => Microservice;

export interface Microservice {
  destroy: () => Promise<any>;
  createProxy: CreateProxy;
  createServiceCall: CreateServiceCall;
}

export interface MicroserviceOptions {
  defaultRouter?: Router;
  services?: Service[];
  seedAddress?: Address | string;
  address?: Address | string;
  transport?: Transport;
  cluster?: (opt: ClusterOptions) => Cluster;
  debug?: boolean;
}
```

```javascript
import { createMicroservice } from '@scalecube/scalecube-microservice';
import { TransportNodeJS } from '@scalecube/transport-nodejs';
import { joinCluster } from '@scalecube/cluster-nodejs';

const microserviceInstance = createMicroservice({
  services: [/* array of services */],
  seedAddress : 'pm://myOrganization:8080/ServiceA',
  address : {
    protocol : 'pm',
    host: 'myOrganization',
    port : 8080,
    path: 'ServiceB'
  },
  transport: TransportNodeJS, // scalecube provide a default transport configuration when running on browser,
  cluster: joinCluster, // scalecube provide a default cluster configuration when running on browser,
  defaultRouter: retryRouter({period:10}),
  debug: true // default is false
})
```
* [destroy](#destroy) - The method is used to delete a microservice and close all the subscriptions related with it.
* [createProxy](#createproxy) - Creates a proxy to a method and provides extra logic when is invoked.
* [createServiceCall](#createServiceCall) - Exposes serviceCall to a user (not via Proxy)

* defaultRouter - set a default router for this microservice container
* services - An array of [services](#service), that will exist inside a microservice container
* seedAddress - The seedAddress is an [address](#address) or a **string (URI format)** of another microservice container in our distributed env.
* address - An [address](#address) or a **string (URI format)** for this microservice instance, other microservices can use this address to connect with this microservice container.
* transport - a module that implements MicroserviceTransport.
* cluster - a module that implements [Cluster](#cluster) API
* debug - add logs

## Router

```javascript
import { roundRobin, retryRouter } from '@scalecube/routers';

const proxyA = ms.createProxy({serviceDefinition, router: roundRobin});

const proxyB = ms.createProxy({serviceDefinition, router: retryRouter({period: 10})});
```

```typescript

type Router = (options: RouterOptions) => Promise<Endpoint>;

interface RouterOptions {
  lookUp: LookUp;
  message: Message;
}


type RetryRouter = (options: RetryRouterOptions) => Router

interface RetryRouterOptions { 
  period: number; 
  maxRetry?: number 
}
```
router is a tool for picking the best service base on given criteria.

* lookUp - The function that finds [Endpoint](#endpoint) by given criteria.
* message - metadata, contain criteria for picking the [Endpoint](#endpoint)

### default

pick the first available item.

### RoundRobin

pick the next item from a list of available items.

### retryRouter

ping the registry every @ms and checking if there are any available items.
pick the first available item.

## Service

```typescript
interface Service {
  definition: ServiceDefinition;
  reference: ServiceReference;
}

interface ServiceDefinition {
  serviceName: string;
  methods: {
    [methodName: string]: {
      asyncModel: AsyncModel;
    };
  };
}

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

Service is combination of contract and the reference that uphold the contract.  

* definition - metadata that define the service.
* reference - code of the service

### ServiceDefinition
Its the contract that describe the service.

* serviceName - The name of a service
* methods - Map of methods that exist in the service.  
 each method describe its [asyncModel](#asyncmodel).

### ServiceReference
Its the code of the service.

* ServiceFactory - callback that provide life-cycle/inject hook in the bootstrap process.
* ServiceObject - object that contains the functionality described in the serviceDefinition.
 
 
## Transport
```typescript
interface Transport {
  clientProvider: Provider;
  serverProvider: Provider;
}

interface Provider {
  providerFactory: ProviderFactory;
  factoryOptions?: any;
  serializers?: PayloadSerializers;
  setup ?: ProviderSetup;
}

interface ProviderSetup {
    dataMimeType ?: string,
    keepAlive ?: number,
    lifetime ?: number,
    metadataMimeType ?: string,
}
  
interface PayloadSerializers {
  data: {
    deserialize: (data: any) => any;
    serialize: (data: any) => any;
  };
  metadata: {
    deserialize: (data: any) => any;
    serialize: (data: any) => any;
  };
}

type ProviderFactory = (options: { address: Address; factoryOptions?: any }) => DuplexConnection;

type DuplexConnection = any;
```
Opinionated communication layer.  
It is used when requesting a service from another microservice instance.

When bootstrapping microservice, it is possible to pass transport in the [options](#microservice).

transport is your custom implementation to [RSocket](https://github.com/rsocket/rsocket-js) Transport Providers.

* clientProvider - implementation for the client side.
* serverProvider - implementation for the server side.

* providerFactory - Factory for creating RSocket client transport provider
* factoryOptions - Extra configuration to pass to the factory
* serializers - Optional serialize functionality for the payload
* setup - Optional setup configuration for the provider
