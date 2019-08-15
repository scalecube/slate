
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

## Invoking a method from proxy throw error code MS0015 - can't find services ...

createProxy or createProxies (without asyncModel flag set to true) does not guarantee that the service is available.

it just provide a proxy to the service.

### possible solutions:

1. set asyncModel flag to true when using [createProxies](##createproxies).  
this option will provide a promise to a proxy.
when calling the promise, it will resolve when the service is available.

2. implement retry logic when requesting a service.
  
