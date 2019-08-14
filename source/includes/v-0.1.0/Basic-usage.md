# Basic usage

After bootstrapping the microservice, we can use it to request services from the [distributed environment](#distributed-environment)

## createProxy

```typescript
type CreateProxy = <T = any>(options: ProxyOptions) => T;

interface ProxyOptions {
  router?: Router;
  serviceDefinition: ServiceDefinition;
}

```

```javascript
const serviceProxy = ms.createProxy({serviceDefinition});
serviceProxy.someMethod().then(console.log) // resolve with `someMethod` response
```


It is possible to createProxy from the microservice instance we have just created. 

This is a proxy to a different service that is shared in the [distributed environment](#distributed-environment).

* router - Custom router specifies the logic of choosing the appropriate remoteService.
* serviceDefinition - The metadata for a service container, that includes the name of a service and the map of methods that are included in it.
                         
## createServiceCall

```typescript
type CreateServiceCall = (options: CreateServiceCallOptions) => ServiceCall;

interface CreateServiceCallOptions {
  router?: Router;
}

interface ServiceCall {
  requestStream: (message: Message) => Observable<Message>;
  requestResponse: (message: Message) => Promise<Message>;
}
```

```javascript
const message = {
  qualifier: 'Service/someMethod',
  data: ['value']
};

ms.createServiceCall().requestResponse(message).then(console.log);
ms.createServiceCall().requestStream(message).subscribe(console.log);

```

serviceCall is another way to request a service.  
it is more low level approach and the user must define the [`message`](#message) properly in-order for it to work.

good example for preferring serviceCall over Proxy is in the [`Gateway`](),  
in the Gateway we want to receive a request from outside of our [distributed environment](#distributed-environment) and then pass the request to the correct service inside ot it.

* router - Custom router specifies the logic of choosing the appropriate remoteService
* requestStream - A method using which a consumer requires a stream and receives an Observable sequence describing updates of the method and qualifier that was used for the invocation
* requestResponse - A method using which a consumer requires data and a provider responds with the data once in the form of promise,  
 that includes the response from the method and qualifier that was used for the invocation.

## createProxies

```typescript

type CreateProxies = (options: CreateProxiesOptions) => ProxiesMap;

interface CreateProxiesOptions {
  proxies: ProxiesOptions[];
  router?: Router;
  isAsync?: boolean;
}

interface ProxiesOptions {
  proxyName: string;
  serviceDefinition: ServiceDefinition;
}

interface ProxiesMap {
  [proxyName: string]: Promise<{ proxy: Proxy }> | Proxy;
}

type Proxy<T = any> = T;
```

```javascript
const { awaitServiceProxy } = ms.createProxies({
  proxies: [{
    serviceDefinition,
    proxyName: 'awaitServiceProxy'
  }],
  isAsync : true
});

awaitServiceProxy.then(({proxy: serviceProxy}) => {
  serviceProxy.someMethod().then(console.log);
});
```
createProxies is an advance way of creating a proxy to a service.
it provide a way to receive multiple proxies in one command.  
also, it provide two option of resolving the proxies:

1. if setting the `isAsync` to `true` then you will receive 
an object with Promises to the proxies.

2.  if setting the `isAsync` to `false` then you will receive 
    an object with the proxies.
    
promise to the proxy will resolve only when the service already in the registry.
this help you avoid retry logic till the service is available to use.

* proxies - List of ProxiesOptions, contain the configuration for creating the proxy
* router - optional router to provide extra logic (remoteCall).
* isAsync - optional flag to resolve the proxy asynchronous way  
if true then the proxy will be resolved when the service is in the registry.  
if false then the proxy will be resolved immediately.
* proxyName - name of the proxy (used as the key in the map).
* serviceDefinition - metadata of the service.
* ProxiesMap - Map of generic proxyName and a Promise to the proxy.

## destroy

```typescript
interface Microservice {
  destroy: () => Promise<any>;
}
```

```javascript
ms.destroy().then(console.log);
```
destroy the microservice instance.

1. stop listen to the [address](#address).  
2. notify in the [distributed environment](#distributed-environment) that the services from this microservice are not available anymore.  
3. remove it self from the [distributed environment](#distributed-environment).  

* destroy - The method is used to delete a microservice and close all the subscriptions related with it.

