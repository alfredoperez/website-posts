---
title: "Setup Jest in Angular 18"
seoTitle: "Step-by-Step Jest Configuration for Angular 18"
seoDescription: "Guide to set up Jest for Angular 18 applications for faster, smoother testing. Steps to uninstall Karma/Jasmine and configure Jest included"
datePublished: Fri Aug 23 2024 18:25:17 GMT+0000 (Coordinated Universal Time)
cuid: cm071kob000030amgd3jyeh5j
slug: setup-jest-in-angular-18
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1724007298786/1801b0cf-8890-483e-99d8-b8bdc65fa3f5.jpeg
tags: tdd, angularjs, web-development, typescript, jest

---

Do you want to run tests faster? Are Karma and Jasmine taking too long to run, making you want to avoid testing anything? Well, one thing you can do is try the experimental [Web Test Runner](https://danywalls.com/testing-in-angular-replace-karma-to-web-test-runner) or you can use Jest.

Currently, I find that Jest is the best option because of its seamless integration with IDEs and its ability to automate test execution and debugging without interrupting my flow🪷.

---

In this guide, I will show you how to setup Jest in your Angular 18 application

## Installing dependencies

The first thing that you need to do is install all the libraries that you will use to run Jest. To use Jest in Angular, you can either set it up with the [Angular Jest Runner](https://www.npmjs.com/package/@angular-builders/jest?activeTab=readme) to run tests with `ng test`, or configure Jest separately and run tests with the `jest` command. I [prefer not using th](https://www.npmjs.com/package/@angular-builders/jest?activeTab=readme)e Angular Jest Runner because I find it simpler to configure and use Jest on its own.

```bash
npm install jest \
   @types/jest \
   @jest/globals \ 
   jest-preset-angular \ 
   --save-dev
```

Now you need to remove all the libraries related to Karma and Jasmine. Make sure to just have the libraries you actually have installed in your application.

```bash
npm uninstall karma \
   karma-chrome-launcher \
   karma-coverage \
   karma-jasmine \
   karma-jasmine-html-reporter
```

Finally, remove any files related to Karma, like the `karma.conf.js` and `src/test.ts` files

```sh
rm ./karma.conf.js ./src/test.ts
```

## Configure Jest

In order to run a test, the first thing that you need to configure is the TS Config file to include the types for Jest. Here is an example configuration you can use:

```json
// tsconfig.spec.json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec",
    "module": "CommonJs",
    "types": ["jest"]
  },
  "include": ["src/**/*.spec.ts", "src/**/*.d.ts"]
}
```

After this, you need to add a `setup-jest.ts` that includes the preset for Angular

```typescript
import "jest-preset-angular/setup-jest";
```

Now, create the `jest.config.js` and add the following:

```js
module.exports = {
    preset: 'jest-preset-angular',
    setupFilesAfterEnv: ['<rootDir>/setup-jest.ts'],
    globalSetup: 'jest-preset-angular/global-setup',
};
```

The last thing is to remove the test runner from the `angular.json`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723470279020/e37e3535-0a06-4d72-9491-5407dc234cac.png align="center")

## Run Jest

After completing these steps, you should be ready to update the scripts and run them. If you have many tests to migrate, you can use the following command to automate the migration:

```sh
npx jest-codemods
```

Finally, to run your tests, modify the `package.json` file to include the following scripts:

```json
 "test": "jest",
 "test:coverage": "jest --coverage"
```

And voila, once you run `npm run test`, you should see Jest executing your tests much faster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1723470784521/6c7222fe-aa91-481c-a75e-df76cd801a97.png align="center")

---

In conclusion, setting up Jest in your Angular 18 app is a breeze and can really boost your testing game. By following this guide, you can easily switch from Karma and Jasmine to Jest, making your tests run faster and smoother. Enjoy the seamless integration and get more done with less hassle. Happy testing! 🧪