# vue-project

This template should help get you started developing with Vue 3 in Vite on Replit!

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

Switch to the shell and run the following to install the packages manually (replit by default will do this for you every time you press the run button).

```sh
npm install
```

### Compile and Hot-Reload for Development

Pressing the run button will start your project with hot-reload for development by using the following command.

```sh
npm run dev
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

Switch to shell and run the following command to run the unit tests.

```sh
npm run test:unit
```

### Run End-to-End Tests with [Cypress](https://www.cypress.io/)

Switch to shell and run the following commands to run the Cypress end-to-end tests.

```sh
npm run build
npm run test:e2e
```

### Lint with [ESLint](https://eslint.org/)

Linting will ensure your code style is consistent.
You can configure all the preferred styles in `.eslintrc.cjs`
Run the following command in the shell to lint your code.

```sh
npm run lint
```

### Prepare for production and hosting.

1. Open the `.replit` file. If you do not see this in your files list. Then in your browser's URL, after the `#` remove everything and type `.replit`, it will look something like this `https://replit.com/@username/replname#.replit`.
2. In the `.replit` file, modify the `run` configuration from `npm run dev` to `npm run build && npm run preview`.
3. After this, go to the secrets tab and add a new secret, name this new secret `NODE_ENV` and set its value to `production`.

Now, pressing the run button will generate a new production build and host the app in production mode.
Enable Always-On for an even better hosting experience.

# Circle Master

Circle Master is a game in which the user has to maintain a main circle between two target circles by pressing and releasing the click or touch anywhere on the screen. The user's score is based on the position of the main circle. Try to achieve the highest score possible!

## Code Structure

The project is built using Vue.js 3 and utilizes the script setup and TypeScript.

### Template

The `App.vue` file contains the main template of the game, which includes:

- The main, inner, and outer circles.
- Elements for displaying the user's score, positioned at the top of the screen.
- Modals for instructions and game results.
- The click area that takes up 100% of the screen height.

### Script

The `App.vue` file also contains the script that handles the game logic, including:

- Refs for circle sizes, user's score, modal states, and other state variables.
- Functions to handle click or touch events and increase and decrease circle sizes.
- Functions to handle displaying modals and sharing the score.
- A function to generate random values for the inner circle size, outer circle size, and the main circle's growth speed on a daily basis.

### Styles

The `App.vue` file also includes CSS styles for positioning and styling the game elements, such as circles, score, and modals.

## Features

### Circles

The game consists of three circles:

1. Main Circle: The circle that the user can control by clicking or touching the screen. Its size grows when the user clicks or touches the screen, and it shrinks when the user releases the click or touch. The main circle is gray when it is not between the two other circles and turns green when it is positioned between them.

2. Inner Circle: The smaller static circle, which represents the lower boundary of the target size range for the main circle.

3. Outer Circle: The larger static circle, which represents the upper boundary of the target size range for the main circle. The outer circle starts at its maximum size and shrinks slowly until it reaches the inner circle size.

### Circle Sizes

The main circle's maximum size is 70% of the screen height. The inner and outer circles have a 3px border, and their sizes represent the target size range for the main circle, within an interval of 10% of the target circle size.

### Scoring

The user's score is displayed at the top of the screen and is calculated based on the main circle's position relative to the inner and outer circles:

- The score is incremented when the main circle is positioned between the inner and outer circles.

### Modals

There are two modals in the game:

1. Instructions Modal: Displays at the start of the game, explaining how to play. The user can close this modal by clicking the "Play" button or the close button.

2. End Game Modal: Appears when the outer circle has finished shrinking. It displays the user's final score and provides a button to restart the game or share the score with friends.

### Web App

The game is designed as a web app, with the game starting on the home page. When the user first visits the home page, the instructions modal is displayed, and the game begins after the user closes the modal.

