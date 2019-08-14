
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
