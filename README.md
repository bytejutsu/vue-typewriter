## Typewriter Vue 3 Component

The Typewriter component is a Vue 3 component that simulates the effect of text being typed out, character by character, in a typewriter fashion. It offers customization options, such as typing speed, start delay, and more.

### Features:

- **Dynamic Typing Effect**: Simulates the typing of text one character at a time.
- **Customizable Typing Speed**: Define the speed at which each character is "typed".
- **Loop Control**: Option to continually loop the typing-deleting cycle or run it just once.
- **Custom Styles**: Pass your own CSS styles for both the text and cursor.
  
### Installation:

To install this component in your project, use npm:

```
npm install @btjspr/vue-typewriter
```

### Usage:

Import the component and register it within your Vue component or globally.

```javascript
import Typewriter from '@btjspr/vue-typewriter';
```

Use it in your template:

```vue
<Typewriter :speed="500" :delay="5000" :loop="false" :textStyles="{color: 'red', fontSize: '2em'}">
  This is your text.
</Typewriter>
```

### Props:

- `speed`: (Number) - Controls the speed of typing. Defines the time (in ms) it takes for each character to appear. Default is `400`.

- `delay`: (Number) - The delay (in ms) before the text starts typing (and before it starts deleting in loop mode). Default is `5000`.

- `loop`: (Boolean) - If set to `true`, the typing effect will repeat indefinitely. If `false`, the effect will run once. Default is `true`.

- `textStyles`: (Object) - A set of CSS properties to style the text. Example: `{color: 'red', fontSize: '2em'}`.

- `cursorStyles`: (Object) - A set of CSS properties to style the cursor. Default styles are: 
  ```javascript
  {
      width: '12px',
      height: '1em',
      backgroundColor: 'currentColor'
  }
  ```

### Slots:

The main text to be "typed out" is passed as a default slot:

```vue
<Typewriter>
  Your text goes here.
</Typewriter>
```

### Styling:

The component has a default cursor style that blinks to simulate the effect of a typing cursor. You can extend or override these styles using the `cursorStyles` prop.

### Example:

Here's an example of the Typewriter component in action:

```vue
<Typewriter :speed="500" :delay="5000" :textStyles="{color: 'blue', fontSize: '1.5em'}" :cursorStyles="{backgroundColor: 'blue'}">
  Hello, world!
</Typewriter>
```
