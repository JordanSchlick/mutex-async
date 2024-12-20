# Mutex Async
A basic javascript library that provides an asynchronous mutex. Designed for use with for async/await. It supports a variable number of locks at a time.


## Usage

```js
const mutexAsync = require('mutex-async');

const mutex = new mutexAsync.Mutex();

async function criticalSection() {
	let unlock = await mutex.lock();
	try {
		// Critical section code here
	} catch(err) {
		
	}
	await unlock();
}

// or

async function criticalSection() {
	await mutex.run(async () => {
		// Critical section code here
	});
}
```

## Multiple simultaneous locks

```js
const mutexAsync = require('mutex-async');

const mutex = new mutexAsync.Mutex({
	maxLocks: 10
});

async function criticalSection() {
	let unlock = await mutex.lock();
	try {
		await new Promise(resolve => setTimeout(resolve, 1000));
	} catch(err) {}
	await unlock();
}

// will only take 2 seconds
let promises = [];
for(let i = 0; i < 20; i++) {
	promises.push(criticalSection());
}
await Promise.all(promises);
```

## License

MIT License
