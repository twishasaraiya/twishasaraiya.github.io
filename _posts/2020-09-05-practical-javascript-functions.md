## Practical Functions(with typescript)

I am sure, most of you must be aware of what functions are and how to write them in javascript. In this post, we will see how to write extensible/generic function.
For understanding purposes I will take few example throughout.

### Example: Display a snackabar

Snackbars are like notifications that display certain information to the user. You can read about them more [here](https://material.io/components/snackbars)

Consider a scenarios, where you have an Angular application and you want to invoke snackbar to show some message. Now, for this we have a built-in snackbar component. We have created a common service that allows all modules/components to show the snackbar.

A simple snackbar function can be written as follows

```javascript
function showSnackbar(message: string){
  this._snackBar.openFromComponent(SnackbarComponent, {
      data: message
  });
}
```

This is a simple function that allows to pass message to the snackbar component. But if you consider a practical scenario, this function wont work for all situations. Lets say, our snackbar component supports following

1. Type: Info, Error, Warning, Success
2. Postion: Top, Bottom, Left, Right
3. Time to Show: Amount of time the snackbar will be visible on screen

Ideally our `showSnackbar` funtion in a common service should allowing these parameters to be passed in. For this, what we can do is something like this

```javascript
function showSnackbar(message: string, type: string, position: string, duration: number){
  this._snackBar.openFromComponent(SnackbarComponent, {
      type: type,
      positon: string,
      duration: duration
      data: message
  });
}
```

This function will run as expected if we pass in everything perfectly. But what if we consider following scenarios

1. Want to pass only certain parameters and not all.
2. Allow to pass only specific values and not anything
3. Use default values for not passed in parameters

Now these are very real usecase and we will try to solve them one by one

1. Lets say, I dont want to pass in paramters in the specified order. The developer should be allowed to pass them in any order really. To resolve this, we can create a single options parameter for any additional information that needs to be passed besides `message`

```javascript
function showSnackbar(message: string, options?: { type?: string, position?: string, duration?: number }){
  this._snackBar.openFromComponent(SnackbarComponent, {
      type: type,
      positon: string,
      duration: duration
      data: message
  });
}
```
The `?` with parameter denotes that it is optional and not compulsory to pass in
You can now call the function in following ways

```javascript
showSnackbar('Hello');
showSnackbar('Hello', { type: 'success'})
showSnackbar('Hello', { duration: 3000, type: 'info'})
```

2. To allow only specific values, we will define `enum` for type and position paramerter

[Enum](https://www.typescriptlang.org/docs/handbook/enums.html) is a type just like string, boolean in typescript. It allows us to define some pre-defined constants. 

```typescript
enum Type{
    SUCCESS = 'success',
    INFO = 'info',
    ERROR = 'error',
    WARNING = 'warning'
}

enum Type{
    TOP = 'top',
    BOTTOM = 'bottom',
    LEFT = 'left',
    RIGHT = 'right'
}

function showSnackbar(message: string, options?: { type?: Type, position?: Position, duration?: number }){ 
    this._snackBar.openFromComponent(SnackbarComponent, {
      type: options.type,
      positon: options.position,
      duration: options.duration
      data: message
  });
}

showSnackbar('Hello');
showSnackbar('Hello', { type: Type.SUCCESS})
showSnackbar('Hello', { duration: 3000, type: Type.INFO})
```

3. Now let say, I dont always want to pass in the type of snackbar to be `INFO`. I want default type and position value. 
This can be easily done as follows

```typescript
enum Type{
    SUCCESS = 'success',
    INFO = 'info',
    ERROR = 'error',
    WARNING = 'warning'
}

enum SnackbarPosition{
    TOP = 'top',
    BOTTOM = 'bottom',
    LEFT = 'left',
    RIGHT = 'right'
}

interface Snackbar {
    type?: Type,
    position?: SnackbarPosition,
    duration?: number
}
function showSnackbar(message: string, { type = Type.INFO, position = SnackbarPosition.BOTTOM, duration = 3000}: Snackbar = {}){ 
    this._snackBar.openFromComponent(SnackbarComponent, {
      type: type,
      positon: position,
      duration: duration
      data: message
  });
}

showSnackbar('Hello');
showSnackbar('Hello', { type: Type.SUCCESS})
showSnackbar('Hello', { duration: 3000})

```
Here we have done a few things

1. First, we use ES6 desctructuring to replace the `options` object. This remove the necessity to always use `options.type` or `options.position`. As you can see in the example, we can directly use type, positon and duration.

2. We define an interface for Snackbar. This helps simplify the function definition and it is more easy to read

3. Then finally since our entire `options` paramter was optional we do `= {}` so that we may or may not pass options parameter. It will always take the default values for `type`, `position` or `duration`

With this, the post comes to end. I hope you found it useful. If so, let me know on my social media handles. Thank you for your time
