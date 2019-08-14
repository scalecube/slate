---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - typescript : api
  - javascript : example

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - version/0.1.0

search: true
---

# Introduction

Scalecube is a toolkit for creating microservices based systems.

Scalecube provides tools: 
 
* [Microservice](#bootstrap)  
* [Router](#router)  
* [Discovery](#discovery)  
* [Cluster](#cluster)  
* [Transport](#transport)  
* [Gateway](#gateway)  


## Installation

yarn:

`
yarn add @scalecube/scalecube-microservice
`

npm:

`
npm install @scalecube/scalecube-microservice
` 

# Motivation

Scalecube implements [Decouple by interface](https://en.wikipedia.org/wiki/Loose_coupling) architecture.  
it is event-base communication system,  
and allow to create services that are loosely coupled in a distributed environment.
 
It can run on both browser or server.

It support Reactive programing.

## Browser

scalecube provide event-base tool for working in microservice approach.

### Isolation

different services communicate via postMessage events, this solution isolates each service.

### Scalability

scalecube bootstrap is the same if your feature located on the main thread or in a web-worker.


## NodeJS

### Isolation



### Scalability


# Core-concepts

## Member

member is a microservice instances.

## Distributed environment

Distributed environment is collection of members that share services between them.

Each member have access to all the services that are shared in the distributed environment.

## Registry

Registry store all [endpoints](#endpoint) received from the discovery.

## LocalCall

When a microservice use it own services.

## RemoteCall

When a microservice use another microservice's services.

## SeedAddress

seedAddress is an [address](#address) of another microservice(seed).

## Seed
the seed is a microservice container that used as an entrypoint to the [distributed environment](#distributed-environment).
each microservice can act as a seed for other microservice containers.

# Terminology

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

## Cluster
create a [member](#member) from the data it receive from the discovery,  
and use it in-order to share data in the distributed environment.

## Transport
is the way scalecube send request/response from one microservice to another.

## Router

```javascript
import { roundRobin } from '@scalecube/routers';

const proxy = ms.createProxy({serviceDefinition, router: roundRobin})
```

```typescript

type Router = (options: RouterOptions) => Endpoint | null;

interface RouterOptions {
  lookUp: LookUp;
  message: Message;
}
```
router is a tool for picking the best service base on given criteria.

* lookUp - The function that finds [Endpoint](#endpoint) by given criteria.
* message - metadata, contain criteria for picking the [Endpoint](#endpoint)

### default

pick the first available item.

### RoundRobin

pick the next item from a list of available items.


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

## LookUp

```typescript
type LookUp = (options: LookupOptions) => Endpoint[] | [];

interface LookupOptions {
  qualifier: string;
}
```

Search for [endPoints](#endpoint) in the registry that match the qualifier.

* qualifier - The combination of serviceName and methodName: <serviceName/methodName>

## Microservice
```typescript
type CreateMicroservice = (options: MicroserviceOptions) => Microservice;

export interface Microservice {
  destroy: () => Promise<any>;
  createProxies: CreateProxies;
  createProxy: CreateProxy;
  createServiceCall: CreateServiceCall;
}

export interface MicroserviceOptions {
  services?: Service[];
  seedAddress?: Address | string;
  address?: Address | string;
  transport?: Transport;
  cluster?: (opt: ClusterOptions) => Cluster;
  debug?: boolean;
}
```

* destroy - The method is used to delete a microservice and close all the subscriptions related with it.
* createProxies - Create a map of proxies or Promises to proxy. (deepened on configuration)
* createProxy - Creates a proxy to a method and provides extra logic when is invoked.
* createServiceCall - Exposes serviceCall to a user (not via Proxy)

* services - An array of services, that will exist inside a microservice container
* seedAddress - The seedAddress is an address of another microservice container in our distributed env.
* address - An address for this microservice instance, other microservices can use this address to connect with this microservice container.
* transport - a module that implements MicroserviceTransport.
* cluster - a module that implements Cluster API
* debug - add logs

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

## Service

```typescript
interface Service {
  definition: ServiceDefinition;
  reference: ServiceReference;
}
```

Service is combination of contract and the reference that uphold the contract.  

* definition - metadata that define the service.
* reference - code of the service
