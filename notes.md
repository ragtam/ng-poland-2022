# Late subscriber
not being able to get value from a stream

For behaviour subject we emit instantionously, so streams are not lazy, since they execute from the very beginning


# How to get rid of .subscribe
push-based.io
set get is imperative
# State

NgRX CQRS Command Query Segregation
NgRX is not redux, what is redux?

Global State: NGRx, Akita (more elegant NgRx)
Persistent state is a small piece of server state.
Push based scales

## In Angular we don`t have clear vision on Ephemeral (compoent) State
State in component dies, state in singleton service doesn`t. 


# state types
1. Global vs Local
2. Static vs Dynamic ( service: long vs ngIf: changes quickly, ofter )
3. Processed sources: Global: Http, Bluetootoh, local: intervals, event listener


# Where should we subscribe?
- async pipe 
returns null if no value is there, subscribe and unsibscribe. We should subscrive in constructor, ubsibscribe in ngOnDestroy.

instead of putting subject in component, we can create a service with a ngOnDestroy hook, and provide it in providers on a component.
We can provide a stream througn some method, and service will subscribe and ubsibscribe to it

every subscriber triggers to each operator in the pipeline.

share vs publish ( produce hot stream ).
Share introduces a layer in a stream, this layer stops. It starts the subscribtion, and forwards it. Whoever subscribes, it gets cached value
from share. 
+ share is lazy, won`t subscribe until first subscriber comes
- publish is not lazy.


# Is template the right place to subscribe?
Template is changing ofter. 
Provider should subscribe, provider of a value is a component. 

# Is there a way to subscrive in a stream?
yes: operators that ends with 'All'
Observable of observables, then we mergeAll. This way we get rid of a subject 

Coalescing should be done in a pipe or a directive

# Sometines we need to mix imperative code with reactive code

Article: Research on reactive ephemeral stata in component oriented frameworks
dev.to/rxjs/research-on-reactive-ephemeral-state-in-component-oriented-frameworks-38lk

Tutorial on rx-angular repo. app, demos, src
Follow readme there

When we  

state.select 
- doing filter undefined
- distinctUntilChanged
- custom logic provided in a select
- and then distinctUntilChanged 
- share replay at the end

No if/else in functionl programming. Instead of that we use filtering.
In case

We split:
state, trigger, effect

Do poczytania:
rx-angular/issues/304


# GET RID OF SUBSCRIBTIONS, GET RID OF ASYNC PIPE
Then get rid of zone.js

MVVM
model view view model.
view model is a derivation of model.

we have

`
    const model$ = this.store.select(); // we don't need to store it. We can derive view model directly from service service
    const viewModel$ = this.model$.pipe(map('doing ''))
`

model is normalized data structure. Means we don`t store the same value in several places.
n1: purest form of normalization

in view model we can have redundancy. 

Now we can derive several viewModels from our model. THis way we make even less change detection checks.

# Presented Pattern
When there are many buttons for instance. It encapsulates 

`
@Injectable() class extenda RxState<SomponentState> {
    // here we keep everything what html can call. Html can call only methods from here. Everything else in the component is private
}
`

Signals:
rx-angular/pull/1112


IF we complete in a flattening operator, the stream will not die. If it fails, the outer stream fails too.
`
of('sth'').pipe(concatMap( v => of(v).pipe() ))


# RxLet:
suspense, error, complete configuration in template

RxQuery Tim Deshmeyer
https://github.com/timdeschryver/rx-query

# pull/910
tasteJS.com

angular-movies repo
search for 'Perf Tip'


# Rx Marbles Design System Components
https://tastejs.com/movies/index.html

# Learning Tools and Categorisation
1. Function types:
- creation function ( of, from , fromEvent, interval ): create observable, might take parameter, might not
- operator function ( takes observable, returns observable ): always take parameter

2. Observable Order
- first order observable
- higher order observables

3. Operator Order
- first order operator
- higher order operator

## Algebraic approach
categorisation of broken pieces

Parts (small pieces): 
concat, map, merge, all, switch, to, exhaust

Out of small pieces we can understand for instance
switchMapTo

## Operator Matrix ( photo )

Merge Map ( we read it from right to left)

MAP: takes a single value to an observable
MERGE: 

Merge has concurency parameter. Limiting the number of concurrent streams.

https://stackblitz.com/edit/rxjs-queues-and-polling

Internally concatMap is just a mergeMap with concurrency parameter set to one.

- btn click, get all data from db, exhaustMap
- if type in search box, we query with param, when we do switchMap
- one table that merges together two db results, mergeMap
- authenticate first: concatMap

`if you con't know what to do, use concat

## SIDE EFFECTS
whenever we have a reference in a rxjs operator, its wrong.
In such cases we can use scan. External variable lives then in accumulator.

reduce is scan, but with takeLast(1). It ignores everything until source observable completes. 

## Drag and Drop Example
https://stackblitz.com/edit/rxjs-dnd-example

When we do switchMap() and ignore the parameter returned, we can use switchMapTo

for drag and drop, we would have to exhaust clucks, so that it took only the very first finger touch.

Check it on mobile, different fingers, different colors on the 
https://stackblitz.com/edit/rxjs-touch

## How to approach migration
if we want to gain performance:
we start to migrate from let to rxLet, ngFor, rxFor

if we want to gain developer experience
we start at component level, search for .subscribe, get rid of that ( no race conditions and no memory leaks )
and once its gone approach global state

READ THAT:
https://github.com/rx-angular/rx-angular/blob/main/libs/state/README.md

# Presentation

https://docs.google.com/presentation/d/1vJZ7KBfQrX4xsDBhSvIlFpnUBz5sHkCjK2oc1oFfkxo/edit#slide=id.p

# 4 terms

hot, cold
unicast, multicast

COLD:
every subscribtion will produce new producer. Each subscriber has its own code snippet that executes

HOT
If we don`t subscibe, there is stil a value.



Use share replay, publish replay, behaviour with ngIf
https://stackblitz.com/edit/ng-mat-rxjs-ex-hot-vs-cold

use refCount: true

Ask Michael in TT about Andreas problem with http and 1 second interval

links:
```
https://stackblitz.com/edit/rxjs-queues-and-polling
https://stackblitz.com/edit/rxjs-hot-vs-cold-observables
https://ng-mat-rxjs-ex-hot-vs-cold.stackblitz.io/ex-base
https://stackblitz.com/edit/ng-mat-rxjs-ex-hot-vs-cold
https://stackblitz.com/edit/rxjs-touch
https://github.com/rx-angular/rx-angular/blob/main/libs/state/README.md
https://docs.google.com/presentation/d/1vJZ7KBfQrX4xsDBhSvIlFpnUBz5sHkCjK2oc1oFfkxo/edit#slide=id.p

```



