# !!! Next Generation CSS Performance | Michael Hladky

Chrome rebuilt render pipeline to increase performance.

RAIL model: how to measure user experience.
- animation ( ok time of 10ms )
- ui response ( ok time of 50ms )
- idle work
- loading

Flame charts in dev tools
- scripting ( changing color, coursor position )
- style ( take css, look at dom, see what styles should be applied )
- layout ( positioning of elements that are now styled )
- paingt ( bring layers to the browse )
- composite ( put those layers to pixels )

This is like a waterfall. If we introduce a change on top, it will affect all the below layers.

How can we introduce a change?
go there to check
csstriggers.com


Purple is layout or style
Yellow is scripting

## go to settings, time: event initiators
Blue button shows up to reboot dev-tools

Animations for 
- width use CPU
- transforms and scales work on GPU

## CSS Containment
contain property: layout | paint | size | style

- LAYOUT is scoped to a document. If a change is in a leaf, it will reevaluate the DOM tree from top to bottom.
- PAINT: only scope of that element ( we need to hide overflow )
- SIZE: Size of parent is not affected if
- STYLE: No performance improvements
- CONTENT: combinatuon of layout and paint
- STRICT: layout paint and size

Jack Archibald contain property

## Content Visibility
Problem: DOM nodes outside of the viewport cause unnecessary work

content-visibility: auto

applies needed containment automatically and skips rendering childs of of screen elements.
NOw it works only for rows

What is not visible is skipped.

Virtual scrolling, natively in the browser

# !! Browser APIs | Francesco Leardini

Dragon, from spaceX uses web compoennts, JS :D

TT: @paco_ITA
dev.to/paco_ita

1. VisibilityChange event
If tab is active/visible or not
2. Screen Wake Lock sentinel
possible only through user interaction. Don`t allow the screen to lock ( if for instance big anmount of data is there to read )
3. Ambient Light
google maps does that, automatically switch on dark mode based on ambient light
4. Contact Picker
5. File System Access
Access to files as native application. Permissions are not persistent between sessions.
   
https://github.com/pacoita/modern-web

# 1,2,3 FASTIFY | Matteo Collina
What is that? Fastify, Pino
https://nodeland.dev/

# !!! The best JS framework ever created | Nir Kaufman

```
<script src="index.js" type="module">

app.js // there do document.body.innerHTML = "<h1>"

in index.js just do import './app.js'
```


it just works

```

create a class not in app.js, 

define private properties with #

```

window.customElements.define('todo-header', Class extending HtmlElement)

Extend HTML ELement

# !!!! Typescript | 

https://www.npmjs.com/package/commander

```
type Command = {
    option(command: string, description?: string): Command
    opts(): Record<string, any>
}

type Commander = {
    new(): Command 
}

declare const Commander: Commander;

const program = new Commander();

```

this is just a foundation

```
type Command = {
    option<U extends string>(command: U, description?: string): Command
    opts(): Record<string, any>
}

type Commander = {
    new(): Command 
}

declare const Commander: Commander;

const program = new Commander();

```

# Tooling | Chris Heilmann

# Impact Driven Development | Hen

# AI, HOW DID GITHUB COPILOT LEARN JAVASCRIPT? | Gerard Sans

Assistant that could write code. 

What if AI could do your job?
How long do you think it can take?

What has been happening in AI? 
- Github Copilot
It`s a coding assistant. 
Codex JavaScript Sandbox
OpenAI Copilot
GTT 3 writing assistant
copy.ai


# PASSWORD | SAM BEGO

presentation is online: 1990.SAMBEGO.TECH

# JEST

To match snapshot

https://stackoverflow.com/questions/50213784/how-snapshot-testing-works-and-what-does-tomatchsnapshot-function-do-in-jest

# MULTI FRAMEWORK MICRO FRONTENDS, MODULE FEDERATION AND WEB COMPONENTS | MANFRED STEYER

Egg Laying Wooly Milk Pig (German Metaphor) - product capable of doing everything, swiss army knife

# Core Web Vitals | MARTA TT: MartaW_PL

1. Largest Contentful Paint (loading)
2.5 is fine

2. First Input Delay (interactivity)

3. Cumulative Layout Shift (visual stability)
font-display: optional + link rel

## How to Measure it?
In the lab:
chrome dev tools, lighthouse, webPageTest, Web Vitals Extension

Do przes??uchania, web vitals report, statystyki w googla analytics

# ELM, delightful language for learning functional programming | KAJETAN
kajetan.dev


