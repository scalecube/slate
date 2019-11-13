
# FAQ

## How can I pass array in message?

```javascript

const arrayToPassInArgs = [1,2,3,4];
const valueToPassInArgs = 5;
const objectToPassInArgs = {'6':6};

const message = {
  qualifier : 'Service/someMethod',
  data: [arrayToPassInArgs, valueToPassInArgs, objectToPassInArgs]
}

```

It is possbile to pass any data structure in the message.  
just need to make sure it wrap in array.


please look at the [example](?javascript#passing-array-argument-in-message)

### related topics:

[message](#message)

## Invoking a method from proxy throw error code MS0015 - can't find services ...

createProxy does not guarantee that the service is available.

it just provide a proxy to the service.

### possible solutions:

using retryRouter when creating the proxy.
retryRouter will ping the registry before the remoteCall till the service is registered.

### related topics:

[remoteCall](#remotecall), [retryRouter](#router)

## Does the order of services is important when using the dependency hook?

The order of the services you provide is the order in which they are registered in the registry.

but, you don't need the service in the registry in order to create a proxy to it.

farther more, it is possible to use the dependency hook for services that are located in other microservice instances. 

### possible solution: 

it is common to apply retry logic in order to make sure the service will run.

### related topics:

[remoteCall](#remotecall)


## Does seedAddress and address are mandatory for scalecube?

No, they are both optional.

### Why to provide seedAddress?

seedAddress is used to connected to a distributed environment.

you must provide the address to one of the microservice instance in the distributed environment you want to joined.

### why to provide address?

It is important to set your microservice instance address 
if you want that other microservice instance will use it as seedAddress.

farther more, microservice without address won't be able to share it services in the distributed environment.

if no address is provided, then microservice bootstrap will generate address for you. 
auto generate address stop you from sharing your services in the distributed environment.
### related topics:

[remoteCall](#remotecall), [distributed environment](#distributed-environment), [seed](#seed), [address](#address)
