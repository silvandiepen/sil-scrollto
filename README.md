# Scroll To

A simple Vue Directive add a function of an element when the browser scrolls. 


### Install

Install the package
`npm install @sil/scroll-to`


Import the package

`import ScrollTo from '~@sil/scroll-to`

Define the component:

```js
import Vue from 'vue';
Vue.directive(ScrollTo);
```
Add the selector v-scrollto to the element you want to scroll from.
To scroll to an element, give the v-scrollto the id or class of the element to scroll to

```html
<a v-scrollto="'#element-to-scroll-to">
```

If you don't give a value. The directive will look for an href selector with a '#'value.

```html
<a href="#element-to-scroll-to" v-scrollto></a>
```
Giving extra settings like time, use comma separated values:

```html
<a v-scrollto="['#element-to-scroll-to',3000,'easeInQuad'"></a>
```

#### Arguments

| Argument | Default       | Description                                             |
| -------- | ------------- | ------------------------------------------------------- |
| element  | undefined     | Animate to this element (ID), example: '#id-of-element' |
| duration | 1000          | Time it takes to do the animation                       |
| easing   | 'easeOutQuad' | The easing in the animation                             |


#### Easings

Available easings:

| type   | in            | out            | inout            | default  |
| ------ | ------------- | -------------- | ---------------- | -------- |
| linear |               |                |                  | `linear` |
| quad   | `easeInQuad`  | `easeOutQuad`  | `easeInOutQuad`  |          |
| cubic  | `easeInCubic` | `easeOutCubic` | `easeInOutCubic` |          |
| quart  | `easeInQuart` | `easeOutQuart` | `easeInOutQuart` |          |
| quint  | `easeInQuint` | `easeOutQuint` | `easeInOutQuint` |          |