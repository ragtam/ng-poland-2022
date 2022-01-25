# Performance Improvements | Michael Hladky

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

# How to start loving writing tests again | Alex Okruszko

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



