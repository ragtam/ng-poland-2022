# !!! Performance Improvements | Michael Hladky

We have a leaf in the tree of components. We want to update h1 tag on the leaf on button click.
All components are rerencered.
Now we introduce onPush and async
All compoents from leaf to root are marked for check.
But still event with this best practice setup is wrong. We need to check all components from root to leaf.

Waterfall diagram shows downloaded assets, how requests are connected to each other. We want to get rid of chained requests. 
Sequential processes are always slower than paralell.
Next to waterfall there is priority column.

```
<link rel="preconnect" href="https://">
```

We can try to prioritize resources. They are inlined at the bottom. They are priorized (already done by Angular)

```
<img loading="lazy">
```
It will load image if it appears in viewport.
When we display list with images. we want to lazy load only part of it. The invisible part. We need to set 
```
[attr.loading]="lazy"
```

Prefetch LCP data
GLobal state service in APP_INITIALIZER, it loads the data necessary for display earlier. This way we move crucial
requests to the beginning. 

Angular bootstrap phase. Huge block task. 
1. webpack bootstrap
2. angular bootstrap
3. app initializer
4. route

For those we can add some scheduling time.

We can wrap setTimeout() around platformBrowser.bootstrap.

This way we chunk tasks into separate ones. We gained 50ms.

we disable initialNavigation: 'disable';

Instead we do setTimeout( router.navigate() ) in app.component.

## NGIF hack
Use RxLet, RxIf, RxFor

# !!! How to start loving writing tests again | Alex Okruszko

If we need to adjust tests everytime there is a change in a class, it means we are testing the structure, not behaviour.

1. End to End: Where whole application is running, whole backend is running.
2. Integration testing: Frontend runs, but is talking to mocked backend. So we can test user interactions.
3. Unit Tests

## SIFERS
Simple Injectable Functions Explicitly Returning State

In a typical test, we have a describe block, where we declare props, then we do provides, and later we do inject, to get the 
reference to a service instance. Then we, let`s say, spy on it. That's 4 blocks that are present in test file.

How can we solve that?

we create a async setup function, that might take parameters. There we declare const of type Partial<ServiceMocked> and we jest.mockReturnValueOf().

then we call TestBed.configureTestingModule();
later we return this prop.

Now in a test we call const { mockedService }  await setup(); at the beginning. This way we get the instance.

DO A READ: Testing With SIFERS
https://medium.com/@kolodny/testing-with-sifers-c9d6bb5b362

```
    const mockLogger: Partial<LogService> = {
        method: jest.fn()
    }
```

Instead of that we can use library: Jest-auto-spies.
This way we explicitly new the service.

What about global objects?
explicitly newing an object is wrong. Instead we can can create a token. This way we can test it easier.

Methods and properties of component class is NOT its public API. Its Template AND typescript class are public. So we need
to test component as a whole, we need to test it using a template and component, not component class only.
Coverage tools don`t measure if we test the template.

USE ANGULAR TESTING LIBRARY, HARNESSES
Instead of passing selector, we get the harness

```
GO AND SEE THE IMAGE 
```

ALWAYS USE DONE IF TESTING IN SUBSCRIBE BLOCK

# JOSE standard, CRYPTOGRAPHY | SAM BELLEN
non-repudiataion can prove who created the signature. My signature on paper can be proven its mine, by the way I write.

Signer uses private key to create signature
Public key to validate the signature
Key generator, signing validator, varifying algorithm

CHeck his github for the code examples. jose.sambego.tech @sambego jwt.io

# ACCESIBILITY | VITALII BOBROV

tools: lighthouse
library: axe-core
voiceover on mac, to listen to a website

# ANGULAR GOOD & BAD PARTS | KAMIL GAŁEK

- no explicit webpack
- webpack vs esbuild

esbuild is used as css optimizer
library: Angular Esbuild

https://dev.to/marcinwosinek/how-to-build-minimal-angular-12-app-with-esbuild-54cc

# !! KISS PRINCIPLE | NIR KAUFMANN

Keep It Simple & Straightforward: Simplicity as a key design.

Adding complexity is easy, simplifying is much harder.

Angular docs:
- Avoid aliasing inputs

Then go to docs:
- They add aliases to inputs

TMTOWTDI
There`s more than one way to do it
Pete Hunt: React Rethinking best practises

YAGNI
You aren`t gonna need it

Create a file:
zone-flags.ts

(window as any).__Zone_disable_timers

This way we can disable some parts of zone. Instead of turning it off completely, we can get rid of some elements only

TO READ:
Angular Logical Tree vs Angular Render Tree

# EVENT DRIVEN ARCHITECTURE | GREG RADZIO

TO READ: CQRS

# CONTENT PROJECTION  | MARCIN KRÓL

```
<message-box>
    Something went wrong
    <ng-container ngProjectAs="[details]">
        some details
    </ng-container>
</message-box>
```

conditional content projection

# SINGLE SCOPE TRAP IN MICRO-FRONTEND APPS | MACIEJ CZERWIAKOWSKI

# TAILOR YOUR ESLINT WITH PERFECTLY FITTING CUSTOM RULES | Gang of 3

https://netbasal.com/create-a-typed-version-of-simplechanges-in-angular-451f86593003

https://medium.com/bigpicture-one/writing-custom-typescript-eslint-rules-with-unit-tests-for-angular-project-f004482551db

# Tools that will make our life easier | Marta Wiśniewska TT: MartaW_PL
- angular cli, ng doc
- nx dev tools
- angular update guide
- npm-check
- angular dev tools, for profiling angular apps (with ivy enabled)
- storybook
- https://bit.dev/
- performance packages: ngx-loadable, hero-loader (code splitting for component level)
    ngx-quicklink, guess.js
- codelyzer
- compodoc

# !!! Microfrontends | Manfred Steyer

Splitting application into tiny parts. 
Customer is not interested in having separate applications. They want one app. This is why we need to have one HOST app.

WEBPACK 5, module federation

0. Module federation.
DEfine two roles of app. 
- Shell (host) 
  - we map urls { mfe1: 'http://' },
  - then we do dynamic import import('mfe/Cmp)
- Microfrontend (Remote)
  - we expose fines in webpack config
    { './Cmp': './my.cmp.ts' }

We can share libraries between applictions.
Using with the Angular CLI
@ng add @angular-architects/module-federation

For angular its just as lazy loading

1. Version mispatches
Selecting the highest compatible version. But what if there is no highest compatible version? By default both are loaded.
shared dependencies: 
```
"my-lib": { 
    singleton: true, 
    strictVersion: true // this will throw an error instead of console logging it 
} 
```

This leads to a question if we want a monorepo or not?
If we want to go by the book, we need to use different repositories. This is what microfrontends are about. We don`t want
to be dependent on the other team. We have a strict borders. Different libraries, different frameworks.
THis cones at cost, we need to load all this stuff to our browser.

This is why we can go with one monorepo. We define that there is only one version of angular, rxjs and so on. We
are self limitating, but it prevents many problems.

How to avoid coupling then?

We can prevent it with linting. (nx-enforce-module-boundaries)

3. Dynamic module federation
Loading micro feontend. We don`t want to now the mappings at compile time.
This way we need to use 
```
loadRemoteModule({ remoteEntry: 'http://', remoteName: 'mfe1', exposedModule: './Cmp' })
```
from webpack API, go to see image.

This way we can set microfrontends dynamically. See image

Checkout @angular-architects/module-federation-tools

# !! RxJS in Angular: Terms, Tips and Techniques | Deborah Kurata

## Terms:
- Observable:
collection of items over time, unlike an array, it doesn`t retain items. Emitted items can be observed over time.
Why?
  - to receive notifications over time
  - for reactive development

Consumer doesn`t receive anything until it subscribes.
Subscribtion is an object that represents the execution of an observable.
When subscribed, the observable begins emitting notifications
    - next
    - error
    - complete

We are using explicit observer, object with those three methods, but that is uncommon.

## Patterns
We can use classical way, method returning a stream.

Or expose a property$ from this service. Then in our ui we pass this property to template, and let it go through an async pipe.

How can we add sth to such stream? How to pass in a parameter? If we have a function, we provide it as a parameter.
We can use Subjects for that.
- it is OBSERVABLE, with subscribe method
- and also an OBSERVER, with next, error, complete

We expose the subject, using asObservable(), so that it was not able to call next on it.

to we do let`s say, categoryStream$.pipe( ???map( catId ) => this.http.get('url' + catId) );

??? what kind of map can we use?
To subscribe to an inner observable, we need some flattening operator. It automatically subscribes to inner observable,
and returning it.
- switchMap
- concatMap ( one stream completed, then next one. Internally MergeMap, with concurrency parameter set to 1)
- mergeMap

https://github.com/DeborahK/Angular-ActionStreams
https://github.com/DeborahK/Toh

# Update on Angular 13 | Minko Gechev

CHeck angular dev tools
https://web.dev/learn/css/

# Scalable Angular Components | Alex Rickabaugh

Tree Shaking getting rid of code, that optimizer can proof is not used.

Webpack is doing treeshaking on the level of es module, basically a file


