---
title: Scalecube

language_tabs:
  - typescript : info
  - javascript : example

toc_footers:
  - <a href='https://github.com/scalecube/scalecube-js'>Scalecube github</a>

includes:
  - Core-concepts
  - API
  - Bootstrap
  - Basic-usage
  - Advance-usage
  - Debug
  - Errors
  - ReleaseNotes
  - FAQ
search: true
---

# Introduction

Scalecube is a toolkit for creating [microservices](#bootstrap) based systems.

as part of the solution, Scalecube provides the follow modules: 
 
* [Router](#router)  
* [Discovery](#discovery)  
* [Cluster](#cluster)  
* [Transport](#transport)  
* [Gateway](#gateway)  

all modules are pluggable in a [microservice](#bootstrap) container and can be customized and replaced.

## Installation
scalecube available by installing the `@scalecube/scalecube-microservice`.
`@scalecube/scalecube-microservice` does not come with default configurations.

another option is to use template.

there is a template for running on browser `@scalecube/browser`
and a template for running on node `@scalecube/node`.


yarn:  
`
yarn add @scalecube/scalecube-microservice
`

npm:  
`
npm install @scalecube/scalecube-microservice
` 

## Templates

**Browser**

yarn:  
`
yarn add @scalecube/browser
`

npm:  
`
npm add @scalecube/browser
`

**Node**
yarn:  
`
yarn add @scalecube/node
`

npm:  
`
npm add @scalecube/node
`

# Motivation

Scalecube provide solution for microservice's architecture.  
It is based on the principle of [Decouple by interface](https://en.wikipedia.org/wiki/Loose_coupling).  
it is event-base system that allow to create loosely coupled services in a distributed environment.

### Environment

scalecube can run on both browser and server.  
on browser it will use postMessage events.  
on server it will use websocket.

### Reactive programing.

support Observable pattern.

### Tools

Provides additional tools:

* **workers** - tool for using web-workers in scalecube ecosystem.
  '@scalecube/scalecube-microservice' export workers which contain:
   * initialize - initialize the web-worker util.
   * addWorker - add a web-worker to sclaecube's ecosystem.
   * removeWorker -  remove a web-worker from sclaecube's ecosystem.
* **stringToAddress** - tool for creating address from string.  
receive URI format and convert it to Address.
  * can use partial URI format
  * use default params to fill the missing values from the given string.
  
## Isolation
 
### RunTime 
Different services communicate via events, this solution isolates each service.
if one of the services throw exception, it won't break the whole js application.

### Development 
Each feature/service can be developed in isolation from other features/services.
services will be able to integrate together base on the interface of each service.

**private methods/property**
  
side effect of using Scalecube allow you to design a system in which the developer of the service can determine which methods/property are public.
only methods/property that are in the service definition can be access from out-side the service.

## Scalability

### RunTime
Easy to bootstrap in every environment/process.

**Browser**
A feature can be located on the main thread or in a web-worker.
Scalecube will manage the communication between the services.
that way scalecube help you to use workers and to scale your runtime processing.

**NodeJS**
A feature can be located on different servers.
Scalecube will manage the communication between the services.

### Development
Scalecube provide easy way to integrate services base on their definition.

# Old browser support

currently `@scalecube/browser` transpile the code to es5.  
but it is still require to add proxy-polyfill to browsers that does not have proxy support.

**tested on:**  
Chrome 37+  
IE 11  
Edge 15  
FF 41  
Safari 7.1  
