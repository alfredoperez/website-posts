---
title: "Angular 11 - Setting up Jest"
datePublished: Mon Jul 06 2020 21:21:59 GMT+0000 (Coordinated Universal Time)
cuid: clh42itlm000209ldht440alf
slug: angular-11-setting-up-jest
canonical: https://dev.to/alfredoperez/angular-10-setting-up-jest-2m0l
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682898809450/703ef6db-6c77-45f7-8b53-01d63637c7ea.png
tags: unit-testing, javascript, angularjs, web-development, jest

---

This is a quick guide to setting up Jest in your new Angular 10 application.

## 1\. Install Jest

```plaintext
npm install jest @types/jest jest-preset-angular --save-dev
```

## 2\. Uninstall Karma

```plaintext
npm uninstall karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter @types/jasmine @types/jasminewd2 jasmine-core jasmine-spec-reporter
```

## 3\. Remove test from `angular.json`

Remove the test section from `angular.json`, this section looks like the following:

```json
   "test": {
          "builder": "@angular-devkit/build-angular:karma",
          "options": {
            "main": "src/test.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "tsconfig.spec.json",
            "karmaConfig": "karma.conf.js",
            "assets": [
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [
              "src/styles.scss"
            ],
            "scripts": []
          }
        },
```

## 4\. Remove `karma.conf.js` and `src/test.ts` files

## 5\. Create `setupJest.ts` file

This file should have the following:

```typescript
import 'jest-preset-angular';
```

## 6\. Modify `tsconfig.spec.json`

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec",
    "types": [
      "jest",
      "node"
    ]
  },
  "files": [
    "src/polyfills.ts"
  ],
  "include": [
    "src/**/*.spec.ts",
    "src/**/*.d.ts"
  ]
}
```

## 7\. Modify `package.json` file

Modify the test scripts to the following:

```json
 "test": "jest",
 "test:coverage": "jest --coverage",
```

Add Jest configuration to the end of this file:

```json
"jest": {
    "preset": "jest-preset-angular",
    "setupFilesAfterEnv": [
      "<rootDir>/setupJest.ts"
    ],
    "testPathIgnorePatterns": [
      "<rootDir>/node_modules/",
      "<rootDir>/dist/"
    ],
    "globals": {
      "ts-jest": {
        "tsConfig": "<rootDir>/tsconfig.spec.json",
        "stringifyContentPathRegex": "\\.html$"
      }
    }
}
```