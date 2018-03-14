# [MO] Get element size or position dynamically with onLayout

## Owner: [Julien Nassar](https://github.com/juliennassar)

## Control Points

- Hide your dynamically sized (or positioned) component width a `style={{ opacity: 0 }}` during the first rendering of your component to avoid glitches.

## Motivation

If you need :
- to create a custom loading bar display
- a list of views without previous knowlegde of their size but want them to be all the same
- just need to know the size or position of an element on your screen dynamically (whithout explicitly describing it the object's style)

You must have access those variables during the runtime of your application.

To do this, React offers a onLayout props in which you can pass a callback that will be executed after the rendering of a component.

## Prerequisites

A react/react-native app with components. We will take here the example of a loading bar like this one :
![loader](https://user-images.githubusercontent.com/13121639/37297957-56f5184e-261f-11e8-9b8b-22c8de783daa.png)

Your goal is to calculate the number of green pixels rows you have to display for a given loading status from 0 (0%) to 1 (100%) without previous knowlegde of the loader width :

```jsx
import React from 'react';
import { View } from 'react-native';

export default class Display extends React.Component {
  render() {
    <View style={{flexDirection: 'row'}}>
      <View>
        {/* Some react component to display whatever */}
      </View>
      <LoadingBar status={0.33}/>
    </View>
  }
}
```

We do not know the size of `LoadingBar` component but we want to display a 33% progress in the `LoadingBar`.

## Steps (~5 minutes)

First add a function to your `LoadingBar` component to get its width, and pass it in the `onLayout` props of your `LoadingBar` Component, and store it in your component's state :

```jsx
import React from 'react';
import { View } from 'react-native';

export default class LoadingBar extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      loaderWidth: 0
    }
  }

  getLoadingBarWidth = event => this.setState({loaderWidth: event.nativeEvent.layout.width })

  render() {
    <View onLayout={this.getLoadingBarWidth}>
      {/* Here you want to display x% of your bar, x being the props 'status' passed by the component above */}
    </View>
  }
}
```

the `onLayout` will generate an event when the LoadingBar is displayed. You can access any layouts info with :
```
const {posX, posY, width, height} = event.nativeEvent.layout
```

you can know calculate the number of green pixels rows with the width we just got with `onLayout` :
```
const numbersOfGreenPixelsRows = this.props.status * this.state.loaderWidth
```

Voilà !