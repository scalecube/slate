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

scalecube available in three templates:  

| package | default configuration |
| --- | --- |
| @scalecube/browser | provide default configuration for running in browser |
| @scalecube/node | provide default configuration for running in server |
| @scalecube/scalecube-microservice | does not provide any default configuration |


| package | yarn | npm |
| --- |  --- | --- |
| @scalecube/browser | `yarn add @scalecube/browser` | `npm i @scalecube/browser` |
| @scalecube/node |`yarn add @scalecube/node` | `npm i @scalecube/node` |
| @scalecube/scalecube-microservice |`yarn add @scalecube/scalecube-microservice` | `npm i @scalecube/scalecube-microservice` |

# Motivation

Scalecube provide solution for microservice's architecture.  
It is based on the principle of [Decouple by interface](https://en.wikipedia.org/wiki/Loose_coupling).  
it is event-base system that allow to create loosely coupled services in a distributed environment.

### Environment

scalecube can be used on browser or node

### Reactive programing.

support Observable pattern.

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
A feature can be located on the main thread or in a web-worker,
Scalecube will manage the communication between the services.  
Allow you to split your services between multiple process and scale your runtime processing.

**NodeJS**
A feature can be located on different servers.  
Scalecube will manage the communication between the services.

### Development
Scalecube provide easy way to integrate services base on their definition.

# Old browser support

currently `@scalecube/browser` transpile the code to es5.    
but it is still require to add proxy-polyfill to browsers that does not have proxy support.

| browser | version |
| --- | --- |
| Chrome | 37+ |  
| IE     | 11  |  
| Edge   | 15  | 
| FF     | 41  |   
| Safari | 7.1 |  
