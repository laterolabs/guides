# How we typically operate a project

#### Deployment Strategy - Paid (Preferred)

[Buddybuild](https://www.buddybuild.com/) is the recommended deployment toolchain.

#### Deployment Strategy - Open Source

[Fastlane](https://fastlane.tools/) is the open source alternative to deployment.


#### Testing - Jest

[Jest](https://facebook.github.io/jest/) has emerged as the standard testing library for React Native.

Our testing strategy is to focus on business logic and data transforms - less so UI logic. Although Jest has snapshot testing, because UI components can change so often, unless it's something that has solidified or won't change, it is not always so worth it to test them.

Instead, essential testing breakpoints are things like Reducers, Sagas, and Transforms. It is a good idea to test all reducer functions, all sagas, and all transforms. It is also useful to test other basic helper functions and calculation functions, such as functions that help with displaying data (e.g. capitalizing a name, or displaying cash values).


#### Debugging - Reactotron and debugging JS remotely

[Reactotron](https://github.com/infinitered/reactotron) is a debugger for react/redux applications.

- Use `console.tron.log` to log to the Reactotron console
- Subscribe to specific state slices in the `Redux Subscriptions` tab of Reactotron
- Watch API calls and the response/request cycle in the `Timeline` tab of Reactotron

Alternatively, debugging JS remotely in the Chrome dev-tools can provide finer grain debugging by `Pausing on Exceptions`. To debug with the Chrome dev-tools, hit `Debug JS remotely` after pressing `ctrl-z-cmd` on the simulator. Then, open the dev-tools and head to the `sources` tab and press `pause on exceptions`


### Prettier & ES Lint

We use [Prettier](https://github.com/prettier/prettier) to format out code. We use it alongside some git-hooks to make sure that all JS code is formatted the same. We also use [ES Lint](https://eslint.org/) to ensure various conventions are standardized as well. 

#### Integrating Third Party libraries

Read the documentation of the library, check the issues, and when it was last updated before deciding on using a third party library. React-Native moves fast and many promising libraries fall into unmaintained oblivion.

Furthermore, sometimes simply adding a package to your `package.json` and running `yarn` isn't enough to fully install a package.

If the package contains **Native Code** (is not purely JS-based) then you will need to run the `react-native link` command. This will add the native code to its respective place in both your iOS project and Android project. While these libraries are often very helpful, they can be difficult to debug since you have to deal with native code/XCode configurations/Android to figure out what's going wrong. It's good to be aware of what is happening when you add such packages.

It is worth reading this brief guide on what the above command does: [Linking Libraries on iOS](https://facebook.github.io/react-native/docs/linking-libraries-ios.html)
