---
layout: post
title:      "React Animations and CSS Transforms"
date:       2018-07-30 23:13:58 -0400
permalink:  react_animations_and_css_transforms
---


The world of web page animations sometimes feels endless - you can check out websites like [CodePen](https://codepen.io/) and see amazing things people are making, just with HTML, CSS, and JavaScript. One way to implement some cool looking animations in an easy way is to use [the CSS transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) property and the package [react-motion](https://github.com/chenglou/react-motion) package. I've mentioned the react-motion package before, but to summarize it uses the power of physics to allow you to create animations with spring. One way to apply react-motion in a practical way is to combine it with CSS transformations to transform HTML elements in a dynamic way. For example:

```
const ActionCardView = (props: Props) => {
  const { name, strength, type, description, onClick, clicked } = props

  const style = {
    scale: spring(clicked ? 0.8 : 1, presets.noWobble),
    rotate: spring(clicked ? 3 : 0, presets.noWobble)
  }

  return (
    <Motion style={style}>
      {({ scale, rotate }) => (
        <div
          style={{
            height: '100%',
            transform: `scale(${scale}) rotateZ(${rotate}deg)`
          }}
          className={`message ${typeToColor[type]}`}
          onClick={() => onClick()}
        >
          <div style={ height: '100%' } className="message-body">
            <Level>
              <LevelLeft>
                <LevelItem>
                  <strong>{name}</strong>
                </LevelItem>
              </LevelLeft>
              <LevelRight>{strength}</LevelRight>
            </Level>
            <p>{description}</p>
          </div>
        </div>
      )}
    </Motion>
  )
}
```

This is a component from a project I am working on - a message that displays some information on screen. When this component remains unclicked, it remains unmodified. When clicked, however, react-motion comes into play: a transformation is applied to the outer div element. The div element (and its children) are scaled by a factor of 0.8 and it is rotated along the z axis 3 degrees. Plus, because react-motion calculates the current value of these CSS properties using physics, if the element is clicked rapidly the animation is not interrupted, it just changes directions seamlessly. There are a lot of different ways to display feedback to a user on screen, and using the CSS transform property in simple but effective ways can possibly be one of them. In the end, the user should enjoy using whatever software is put in front of them and pleasing animations can be a step towards that.  
