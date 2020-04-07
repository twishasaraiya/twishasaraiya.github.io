
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



### Demo

[Stackblitz Demo](https://angular-t1fx8w.stackblitz.io)
