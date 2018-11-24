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

### quick demonstration of problem:

1. follow below reproduction steps OR ...clone this repo
3. `npm i` and `npm test`
4. step `7` of below repdouction steps is observed.

### reproduction steps (of how I made all of code)

1. expo init
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





