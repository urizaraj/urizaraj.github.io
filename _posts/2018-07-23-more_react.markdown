---
layout: post
title:      "More React"
date:       2018-07-24 03:43:42 +0000
permalink:  more_react
---


One thing that is worth shouting out are the amount of great open source solutions for anything on the web. Especially with the node package manager - if there's something you would like to implement in your project, it feels like you will find something on GitHub that does what you want to do. 

One interesting package I've been looking into recently is the [react-motion](https://github.com/chenglou/react-motion) package - it allows you to harness the power of JavaScript to create animations with physics. Implementing this package is pretty easy - here's an example from the documentation:

```javascript
import {Motion, spring} from 'react-motion';
// In your render...
<Motion defaultStyle={{x: 0}} style={{x: spring(10)}}>
  {value => <div>{value.x}</div>}
</Motion>
```

The basics are, pass in the spring function to the style property in the `Motion` component and include a function as the child. Then, you can use the style object in the JSX that the function generates. The example above simply renders the number, but you can pass in `value.x` to other things such as an element's opacity, size, or position. 

For example, this package synergizes well with the [react-fontawesome](https://github.com/FortAwesome/react-fontawesome) package. FontAwesome provides what feels like endless icons for any situation, and with the latest update they made transformations for their icons possible. You can combine react-motion with react-fontawesome in interesting ways to have icons rotate with physics:

```javascript
<Motion defaultStyle={{x: 0}} style={{x: spring(10)}}>
  {value => <FontAwesomeIcon icon="spinner" transform={{ rotate: value.x }} />}
</Motion>
```

Finally, one interesting package that utilizes react-motion is the [react-collapse](https://www.npmjs.com/package/react-collapse) package. This package allows you to create collapsible elements easily, with only a single property needed to control whether an element is collapsed or not.

```javascript
<Collapse isOpened={true}>
  <div>Random content</div>
</Collapse>
```

I've been using this package to create really intuitive interfaces quickly. Collapsing elements that do not have a fixed height ahead of time can be a pain, so packages like this can make life a lot easier.
