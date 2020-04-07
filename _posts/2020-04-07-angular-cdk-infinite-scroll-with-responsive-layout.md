
> I recently implemented Infinite Scrolling for a large amount data stored in firestore, with response layout using Angular 9. So this post is just a small replica of the same

## Pre-requisite

- A running angular 9 application

## Setup

1. **Install [Angular CDK](https://material.angular.io/cdk/categories)**

`npm i @angular/cdk`

Angular CDK consists of many modules, but for this post we will focus on `Scrolling` and `Layout` modules

2. **Create a basic virtual scroll**

Add `ScrollingModule`

**app.module.ts**
```typescript
// other imports here
import {ScrollingModule} from '@angular/cdk/scrolling';

@NgModule({
  imports: [ 
    BrowserModule,
    FormsModule,
    ScrollingModule
  ],
  providers: [
    AngularFirestore,
    AngularFirestoreModule
  ],
  declarations: [ AppComponent, HelloComponent ],
  bootstrap:    [ AppComponent ]
})

// remaining code here
```

**app.component.html**
```html
<cdk-virtual-scroll-viewport itemSize="140" class="card-container">
  <div *cdkVirtualFor="let item of items">
       {{ item }}
  </div>
</cdk-virtual-scroll-viewport>
```

Define variable `items`
**app.component.ts**
```typescript
items: Array<string> = ["one", "two", "three"];
```

3. Flex items 

Currently `virtual scroll` displays one item in a row, what if we want multi elements in stacked in a row
So there is no way to do it directly from CSS by applying `flex` property to the container.To solve this, the `items` array is divided into smaller chunks of predefined size

<p align="center">
<img src="https://github.com/twishasaraiya/twishasaraiya.github.io/blob/master/assets/2020-04-07%20flex-direction%20row.png">
</p>

**app.component.ts**
```typescript
tail = (a) => a[a.length - 1];

chunkReducerForSize = (chunkSize = 2) => (result, item) => {
    const lastElm = this.tail(result);
    if (lastElm.length === chunkSize) {
      result.push([item]);
    } else {
      lastElm.push(item);
    }
    return result;
  };

toChunks = (arr, chunkSize = 5) => {
    return arr.reduce(this.chunkReducerForSize(chunkSize), [[]]);
 };
```


```javascript
console.log(toChunks(items,2));

[Array[2], Array[1]]
0: Array[2]
  0: "one"
  1: "two"
1: Array[1]
  0: "three"
```

4. **Responsive Layout**

Now, since we want to change the number of elements in a single row depending on screen size, we will use [LayoutModule](https://material.angular.io/cdk/layout/overview) from `@angular/cdk` and change the `chunkSize` accordingly

Add `LayoutModule`

**app.module.ts**
```typescript
import {LayoutModule} from '@angular/cdk/layout';
```

We use a `BehaviorSubject()` for `chunkSize` so that the chunks are generated whenever the chunkSize changes. Default `chunkSize` is defined as 2.

```typescript
chunkSize = new BehaviorSubject(2);
```


Now we subscribe to predefined breakpoints, but custom breakpoints can also be defined

> The built-in breakpoints are based on [Material Design Specification](https://material.io/design/layout/responsive-layout-grid.html#breakpoints)

**app.component.ts**
```typescript
import { BreakpointObserver, Breakpoints } from "@angular/cdk/layout";
// other imports

constructor(private breakpoint$: BreakpointObserver){
breakpoint$.observe([
      Breakpoints.XSmall,
      Breakpoints.Small,
      Breakpoints.Medium
    ]).subscribe(result => {
      if (result.breakpoints[Breakpoints.XSmall]) {
        this.chunkSize.next(2);
      }
      else if (result.breakpoints[Breakpoints.Small]) {
        this.chunkSize.next(4);
      }
      else if (result.breakpoints[Breakpoints.Medium]) {
        this.chunkSize.next(6);
      }
      else {
        this.chunkSize.next(8);
      }
    });
}
```


### Demo

[Stackblitz Demo](https://angular-t1fx8w.stackblitz.io)
