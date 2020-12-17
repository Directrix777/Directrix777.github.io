---
layout: post
title:      "Trick 6: CSS Situation"
date:       2020-12-17 20:43:11 +0000
permalink:  trick_6_css_situation
---


One of the overarching components of my projects has been CSS. It's what's in charge of making everything look nice. For instance, if you were to write in an index.css file, `.bordered {border: 1px solid rgb(0,0,0);}` Now everything in the dom with a className of `'bordered'` will have a thin border around its contents. But this isn't always what we want. Suppose we have a button that we want to be able to change color `onClick`. We can set up both css classes easily.
```
.green-button {
    height: 20px;
		width: 120px;
		text-align: center;
    border: 1px solid rgb(0,0,0);
		background-color:  rgb(0, 173, 0)

}

.red-button {
    height: 20px;
		width: 120px;
		text-align: center;
    border: 1px solid rgb(0,0,0);
		background-color:  rgb(173, 0, 0)

}
```

So now we can use the render method of our button container to render it one way or the other.

```
...
export default class Button Container extends Component{
    render() {
		    return <div className='green-button' onClick={this.handleClick}>Click Me!</div>
		}
		
		handleClick() {
		    //But what goes here???
		}
}
```
That's a very good question, codeblock! Let's start with something simple. Up before our render method, let's create something in the state, so we can keep track of what color the button is.

```
...
export default class Button Container extends Component{
    constructor(props) {
		    super()
				this.state = {buttonColor: 'green'}
		}
		...
}
```
Now, in our `handleClick` method, we can change the color to red, and vice versa.
```
...
export default class Button Container extends Component{
    ...
		handleClick() {
		    if(this.state.buttonColor === 'green')
				{
				    this.setState({buttonColor: 'red'})
				}
				else
				{
				    this.setState({buttonColor: 'green'})
				}
		}
		...
}
```
So now our button's state changes when we click it, but it's not actually changing color. That's because we're still hardcoding the className. If we did something like this: `className={'green-button'}`, the same effect would be achieved, since we just assign the return value of whatever is in the braces. But wait...couldn't we do some string interpolation here? 
```
 return <div className={`${this.state.buttonColor}-button`} onClick={this.handleClick}>Click Me!</div>
```
This way, our button will render based on the state of the class. We can render our CSS conditionally!

So that's my trick! You can use string interpolation to render conditional CSS without having to make a seperate method for deciding which version to render!
