## [expo issue 2959](https://github.com/expo/expo/issues/2595#issuecomment-441362040)


### what is the problem?
Title! As noted in the original post, this has larger implications, as installing new packages besides the bootstrapped ones results in jest not working.


### proposed solution to the problem?

[answer](https://github.com/expo/expo/issues/2595#issuecomment-440966998) is a very good one, but it seems to make only some of the jest tests work. This repo demonstrates this.

### environment
1. Node version: 10.7.0
2. NPM version: 6.1.0
3. Mac OS High Sierra 
4. Expo version: 2.4.0

----

### quick demonstration of problem + using proposed solution:

1. follow below reproduction steps OR ...clone this repo
3. `npm i` and `npm test`
4. step `7` of below reproduction steps is observed.

### reproduction steps (of how I made all of code)

1. `expo init`
2. choose template tabs
3. `rm -rf node_modules` - to set the slate 'clean'
4. `npm i jest-expo --save-dev`
5. `touch jest.config.js` - and added below to file

```
module.exports = {
  preset: 'jest-expo',
  transform: {
    '\\.js$': '<rootDir>/node_modules/react-native/jest/preprocessor.js',
  }
};
```

6. changed `"test": "node ./node_modules/jest/bin/jest.js --watchAll"` in package.json to `"test": "jest"` NOTE: the jest node module no longer even exists at this point....which makes sense why previous command no longer succesfully runs

7. `npm test` - jest at this point is now working, but the two bootstrapped jest tests in App-test.js do NOT work (while StyledTest-test.js does)

### NOTE: 

1. After step `6` of reproduction steps, removing
`  "jest": {
    "preset": "jest-expo"
  },` in package.json does not change results
2. After step `6` of reproduction steps, if I do `rm -rf node_modules` and `npm i` the results are also the same.

----

### error msg dump of step `7.` of reproduction steps
```
$ npm test

> my-new-project@ test /Users/mattscoo/Desktop/expoExample/ariana/jestExpoTransformError
> jest

 PASS  components/__tests__/StyledText-test.js
 FAIL  __tests__/App-test.js (20.013s)
  ● Console

    console.error node_modules/react-test-renderer/cjs/react-test-renderer.development.js:8075
      The above error occurred in the <App> component:
          in App (at App-test.js:14)

      Consider adding an error boundary to your tree to customize error handling behavior.
      Visit https://fb.me/react-error-boundaries to learn more about error boundaries.
    console.error node_modules/react-test-renderer/cjs/react-test-renderer.development.js:8075
      The above error occurred in the <App> component:
          in App (at App-test.js:19)

      Consider adding an error boundary to your tree to customize error handling behavior.
      Visit https://fb.me/react-error-boundaries to learn more about error boundaries.

  ● App snapshot › renders the loading screen

    TypeError: Cannot read property 'default' of undefined

      4 | import AppNavigator from './navigation/AppNavigator';
      5 |
    > 6 | export default class App extends React.Component {
        |                                                                                                                                   ^
      7 |   state = {
      8 |     isLoadingComplete: false,
      9 |   };

      at new App (App.js:6:387)
      at constructClassInstance (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:4810:18)
      at updateClassComponent (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:6581:5)
      at beginWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:7408:16)
      at performUnitOfWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10149:12)
      at workLoop (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10181:24)
      at renderRoot (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10267:7)
      at performWorkOnRoot (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11135:7)
      at performWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11047:7)
      at performSyncWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11021:3)

  ● App snapshot › renders the root without loading screen

    TypeError: Cannot read property 'default' of undefined

      4 | import AppNavigator from './navigation/AppNavigator';
      5 |
    > 6 | export default class App extends React.Component {
        |                                                                                                                                   ^
      7 |   state = {
      8 |     isLoadingComplete: false,
      9 |   };

      at new App (App.js:6:387)
      at constructClassInstance (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:4810:18)
      at updateClassComponent (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:6581:5)
      at beginWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:7408:16)
      at performUnitOfWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10149:12)
      at workLoop (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10181:24)
      at renderRoot (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:10267:7)
      at performWorkOnRoot (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11135:7)
      at performWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11047:7)
      at performSyncWork (node_modules/react-test-renderer/cjs/react-test-renderer.development.js:11021:3)

Test Suites: 1 failed, 1 passed, 2 total
Tests:       2 failed, 1 passed, 3 total
Snapshots:   1 passed, 1 total
Time:        25.233s
Ran all test suites.
npm ERR! Test failed.  See above for more details.
```


