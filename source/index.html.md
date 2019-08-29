---
title: Scalecube

language_tabs:
  - typescript : api
  - javascript : example

toc_footers:
  - <a href='https://github.com/scalecube/scalecube-js'>Scalecube github</a>

includes:
  - v-0.1.0/Core-concepts
  - v-0.1.0/API
  - v-0.1.0/Bootstrap
  - v-0.1.0/Basic-usage
  - v-0.1.0/Advance-usage
  - v-0.1.0/Errors
  - v-0.1.0/FAQ
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

Scalecube implements [Decouple by interface](https://en.wikipedia.org/wiki/Loose_coupling) microservice's architecture.  
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
   * addWorker - add a web-worker to sclaecube's ecosystem.
   * removeWorker -  remove a web-worker from sclaecube's ecosystem.

* **stringToAddress** - tool for creating address from string.  
receive URI format and convert it to Address.
  * can use partial URI format
  * use default params to fill the missing values from the given string.
  
  
## Browser

scalecube provide event-base tool for working in microservice approach.

### Isolation

#### RunTime 
Different services communicate via postMessage events, this solution isolates each service.
if one of the services throw exception, it won't break the whole js application.

#### Development 
Each feature/service can be developed in isolation from other features/services.
services will be able to integrate together base on the interface of each service.

### Scalability

#### RunTime
Scalecube bootstrap is the same if your feature located on the main thread or in a web-worker.
therefor, it is easier to use workers to scale your runtime processing.

#### Development
Scalecube provide easy way to integrate services base on their definition.

## NodeJS

### Isolation

different services communicate via web-socket, each service can run on different node server.

### Scalability

