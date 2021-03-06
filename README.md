# Qilan concept

## Mocks

### Creating a mock from a class
```js
class Dog {
	bark(type) { /* ... */ }
	async woof() { /* ... */ }
}

const dog = mock(Dog);
```

## Spies

### Creating a spy
```js
const bark = spy();
const woof = when().resolve();

const meow = when(() => spy()).do('m-m-meow');
const purr = when(() => spy('purr?')).return('purr!');
```

### Spying on all methods & properties
```js
spy(dog);
```

## Custom behavior

### Simple

```js
when(() => dog.name).return('Cooper');
when(() => dog.bark()).do(() => console.log('bark!'));
```

### Multiple calls

```js
when(() => dog.name).return('Cooper', 'Baxter');
when(() => dog.bark()).do(
	() => console.log('odd bark!'),
	() => console.log('even bark!'),
);

console.log(dog.name, dog.name, dog.name); // Cooper Baxter Cooper
console.log(dog.bark(), dog.bark(), dog.bark()); // odd bark! even bark! odd bark!
```

```js
when(() => dog.name).returnOnce('Cooper', 'Baxter');
when(() => dog.bark()).doOnce(
	() => console.log('first bark!'),
	() => console.log('second bark!'),
);

console.log(dog.name, dog.name, dog.name); // Cooper Baxter undefined
console.log(dog.bark(), dog.bark(), dog.bark()); // first bark! second bark! undefined
```

### Arguments

```js
when(() => cat.meow()).do(type => 'meow');
when(() => cat.meow(anyArgs())).do(type => 'm-m-meow');

when(() => dog.bark(any(String))).do(type => `${type} bark!`);
when(() => dog.bark(any(Number))).do(count => `bark #${count}!`);
```

## Expectations

### Equality assertion

```js
same(dog.breed, 'Doge');
equal(dog.age, any(Number));
```

### Call expectations

```js
called(() => dog.bark(anyArgs()));
calledOnce(() => dog.woof());
calledTimes(5, () => cat.meow(true));
notCalled(() => cat.purr(any(String), any()));
```

