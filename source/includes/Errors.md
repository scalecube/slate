# Errors

|Error Code | Message | Possible solution | 
| --- | --- | --- |
| MS0000 | microservice does not exists | microservice instance have been destroyed |
| MS0001 | [Message](#message) has not been provided | calling requestResponse or requestStream must contain a Message |
| MS0002 | [Message](#message) data has not been provided | Message must contain data property |
| MS0003 | [Message](#message) qualifier has not been provided | Message must contain qualifier property |
| MS0004 | [Message](#message) should not to be empty object | can not pass empty object as Message |
| MS0005 | [qualifier](#qualifier) expected to be service/method format | qualifier is not a string divided by '/'|
| MS0006 | [Service](#service) missing definition | when [bootstrap microservice](#bootstrap) you provide service without a contract |
| MS0007 | [Message](#message) format error: data must be Array | Message data property must be array, [how can i pass array in message](#passing-array-serviceall)|
| MS0008 | Not valid format, [services](#3-creating-microservice) must be Array | when [bootstrap microservice](#bootstrap), services must be array of service |
| MS0009 | Not valid format, [service](#service) must be Object | service must be an object|
| MS0010 | Not valid format, [Microservice configuration](#microservice) must be an Object | missing configuration when bootstrapping a microservice instance |
| MS0011 | [qualifier](#qualifier) should not be empty string | qualifier must be valid string in the format 'part1/part2'|
| MS0013 | Transport provider is not define | when running on nodejs, [Transport](#transport) must be provided|
| MS0014 | service method <methodName> missing in the [serviceDefinition](#1-define-the-service) | try to call a method from a proxy that does not in the definition|
| MS0015 | can't find services that match the give criteria: <[qualifier](#qualifier)> | requesting a service that is not in the [registry](#registry)|
| MS0016 | <[asyncModel](#asyncmodel) does not match, expect asyncModel in the proxy definition>, but received <asyncModel in the endpoint definition>| asyncModel in the request does not match the asyncModel define in the registry for the service|
| MS0017 | service (<[qualifier](#qualifier)>) has valid definition but reference is not a function.| [ServiceObject](#2-create-the-servicereference) method is not a function  |
| MS0018 | service does not uphold the contract, <serviceName> is not provided | definition has a method that is not provided in the reference |
| MS0019 | Not valid format, <serviceName> reference must be an Object | [ServiceReference](#2-create-the-servicereference) must return object|
| MS0020 | Invalid format, definition must contain valid serviceName | serviceDefinition must contain property `serviceName` |
| MS0021 | Invalid format, definition must contain valid methods of type object | methods must be of type object |
| MS0022 | Invalid format, definition must contain valid methods | methods must be none empty object |
| MS0023 | Invalid format, serviceName must be not empty string but received type <typeof serviceName> | fix the serviceDefinition, serviceName must be a string |
| MS0024 | ROUTER_NOT_PROVIDED | pass router plugin in the createProxy |
| MS0025 | (service provider)'s service 'serviceName/methodName' define as Promise but service return not Promise | incorrect implementation of the service |
| MS0026 | (service provider)'s service 'serviceName/methodName' define as Observable but service return not Observable | incorrect implementation of the service |
| MS0027 | (service provider)'s 'serviceName/methodName' has no valid response, expect Promise or Observable | incorrect implementation of the service |
| MS0028 | invalid async model | asyncModel in definition is invalid |
