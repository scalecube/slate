# Debug

## Mocks

```javascript
import utils = require('@scalecube/utils');

// polyfill for messageChannel
utils.mockMessageChannel();
```

When trying to test app as if running on browser 
and testing microservices in node environment (jest) 
and the test involve multiple microservice instances
and performing [remoteCall](#remotecall) then it is require to 
add the code snippet before the test is running.

the snippet will mock scalecube check if running in node
and will add polyfill [messageChannel](https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel) which required by browser implementation of scalecube.
