---
title: Scalecube

language_tabs:
  - typescript : api
  - javascript : example

toc_footers:
  - <a href='https://github.com/scalecube/scalecube-js'>Scalecube github</a>

includes:
  - v-0.1.0/Terminology
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

