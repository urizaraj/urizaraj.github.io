---
layout: post
title:      "React and Markdown"
date:       2018-06-11 20:39:35 +0000
permalink:  react_and_markdown
---


While learning the ropes on integrating React with Typescript and Node.js, I had an idea - it would be interesting to implement [Markdown](https://en.wikipedia.org/wiki/Markdown) with React, so that users can fill in a text box and be able to preview how it would look in Markdown, seamlessly from the web page. Markdown converts to HTML pretty easily, so I figured it would not take too much effort to implement.

To start, I installed the [react-markdown](https://github.com/rexxars/react-markdown) package from npm. Taking a look on Github, it looked easy to integrate with my project. After that, here's how I implemented it within my existing components.

First, within the constructor, I set the state to have a 'preview' attribute: 

```
constructor(props) {
  this.state = {
    preview: false
  }
}
```

Next, I created a method to toggle the state:

```
togglePreview = () => this.setState(({ preview }) => ({ preview: !preview }))
```

Within the render method, I conditionally render a textarea or a preview of the markdown based on the value of 'preview':

```
{this.state.preview ? (
  <div className="p-3 ">
    <ReactMarkdown source={this.props.content} />
  </div>
) : (
  <div className="form-group">
    <textarea
      className="form-control"
      name="content"
      value={this.props.content}
      onChange={this.handleChange}
    />
  </div>
)}
```

Finally, I added a button that, when clicked, toggles the preview state:

```
<button type="button" onClick={this.togglePreview}>
  Preview
</button>
```

On the backend, the markdown can just be saved as plaintext, and using the ReactMarkdown component again allows it to be displayed easily in any view.

The ReactMarkdown component also has a lot of options that allow for tweaking. For example, the component accepts a disallowedTypes property - if you want to prevent users from displaying images, you can edit this property to include the 'image' node and images will not be displayed.

This is a basic implementation, but I have other ideas for how this could be extended. You could, for example, easily create a toolbar with buttons that trigger certain actions, like inserting a table or bold text. Markdown is really powerful and simple to use, so there are a lot of use cases for it.
