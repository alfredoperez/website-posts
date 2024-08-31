---
title: "Faster Development with JetBrains AI "
seoTitle: "Speed Up Coding with JetBrains AI"
seoDescription: "Enhance coding speed with JetBrains AI Assistant: intelligent code suggestions, automatic documentation, AI chat support, and code refactoring"
slug: faster-development-with-jetbrains-ai
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725123947692/6d71c6fc-4070-4ef5-a21e-d585325c3363.jpeg
tags: ai, webstorm, ides, devtools, web-development, jetbrains

---

It's safe to say that AI is everywhere these days, especially with all the coding options like GitHub Copilot, TabNine, and Cursor. So, I figured it was a good time to try out the AI features in my go-to IDE, WebStorm.

%[https://www.youtube.com/watch?v=-NnYtfzO7qU] 

[JetBrains AI Assistant](https://www.jetbrains.com/ai/) is an integrated artificial intelligence feature within JetBrains' suite of IDEs, such as IntelliJ IDEA, PyCharm, and WebStorm. It provides various functionalities to enhance the coding experience, including:

1. **Code Completion**: offers intelligent code suggestions and autocompletion to speed up coding and reduce errors.
    
2. **Documentation Generation**: Automatically generates documentation for classes and methods based on the code context.
    
3. **AI Chat**: Allows developers to interact with an AI assistant directly within the IDE to ask questions, get code examples, and receive coding advice.
    
4. **Code Refactoring**: assists in refactoring code by providing suggestions and automating repetitive tasks.
    
5. **Error Detection**: Identifies potential errors and suggests fixes to improve code quality.
    

These features aim to improve developer productivity, code quality, and overall efficiency in the software development process, but please note that in order to get JetBrains AI Assistant, you will need to pay for a separate subscription on top of the WebStorm or IntelliJ subscription.

---

Here are the goals I aimed to achieve:

1. **Write Documentation**: The first task was to write JS Docs for all the public methods and the class itself.
    
2. **Create Unit Tests**: Next, I wanted to add some missing tests.
    
3. **Improve Code**: After completing the tests, I aimed to enhance the code for better readability and reduced cognitive load.
    
4. **Commit the Code**: Once all the refactoring was done, I just needed to create a commit message to make a pull request.
    

---

## Write Documentation

OK, let’s get started. The first thing I want to do is write documentation for all the methods in this class, as well as for the class itself.

There are a couple of ways I could achieve this. I could start typing and see what the AI suggests, or open the chat and ask for the documentation. The route I took was to right-click on the method name and select the option to "Generate Documentation."

![A screenshot showing the JetBrains AI menu with options for actions, highlighting "Write Documentation".](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122418820/471ee542-4598-4642-bbcc-ebb26d26067f.png align="center")

### Prompt Library

What's interesting about JetBrains AI is that it provides a set of actions that can be executed with just a right-click. It also has a prompt library where you can specify exactly what prompt to use while an action is being executed.

For example, when I click on "Generate Documentation," I can add a set of rules for how I want the documentation to be generated, such as not adding author, version, or any other tags I feel are unnecessary. This is really useful because I can teach JetBrains AI how I like things to be done.

![Screenshot of JetBrains AI interface with a highlighted Write Documentation prompt for JavaScript, displaying guidelines for writing JSDocs. A note below reads: 'Provides a prompt to Write Documentation.](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122460282/cb68b339-eb5d-409a-b12e-33b5898e3155.png align="center")

Here is an example prompt I am currently using for my documentation:

```markdown
- Do not use @author or @version or @since tags.
- DO NOT use html tag s such as <p>, <lu>, <li>.
- DO NOT add type for the params
- DO NOT add type for the return value
- If it is a method, DO NOT generate example usage.
- If it is a method, DO NOT generate usage example.
- When the method does not return anything, write "This method does not return any value."

Write JSDocs
```

Note that I had to specify what to write when the method doesn't return anything to keep things consistent. Since it's generated with AI, it might create different messages for the same situation.

Another interesting feature is that you can ask things directly in the code. For example, if you don’t like how the documentation was generated, you can open the AI tool next to the line where the documentation was created and ask to regenerate or add anything that is missing.

In the following example, you can see how I asked it to add the example field and it was able to event document how the service should be used.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122642441/13645524-ceca-4e80-8973-7b8054e078be.png align="center")

Thanks to all this, I was able to generate the service, and now whenever I hover over in the IDE, I can see beautiful documentation. ✨

---

## Creating tests

Adding documentation was easy enough and can be done with any AI tool. However, we discovered the Prompt library and that JetBrains AI tool is context-aware. Moving on to my next task, I needed to add some unit tests. To my surprise, there is also a prompt to generate unit tests. This time, I decided to modify the prompt to specify how I want the test structure and which testing tools I use.

Here is how the prompt ended up:

```markdown
- Try to avoid brackets in arrow functions
- Use the ngMocks.faster(); before the first `beforeAll` or `beforeEach` statement, but inside the first `describe`
- Use the MockBuilder from ng-mocks and Mock every dependency 
- Add an extra `beforeEach` to define the current service tha is being tested

- Group tests by the public methods in the class and create a `describe` for each
- Add `it` statements of what should be tested 
- Test failing cases

- Add test implementation
- when using spies, make sure use `.toHaveBeenCalledWith`
- Put mock data should be under an object call `data`
- Use short names for the mocked data and assuming it is context aware
- Do NOT add urls and requests if the service doesnt make API requests
- Do NOT write code comments explaining the testss
- Add clear fail messages into the assertion calls.
```

So now, with the prompt created, I right-clicked on the class name and selected 'Generate Unit Tests.' It not only generated the tests but also created a file for them. This is a huge plus for me because, with a single click, I can generate tests and then just need to review them.

![A screenshot of JetBrains AI interface is shown. The screen displays code in a dark-themed editor, with a context menu open and the option "Generate Unit Tests" highlighted. ](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122765398/20228195-ede9-4489-81ca-771241114590.png align="center")

---

## Improve Code

Now that I had the unit tests ready, it was time to find ways to improve the code, knowing I could re-run the tests to ensure nothing breaks. To do this, I opened the chat and typed the prompt to improve the code. I specified the file by using `#` and selecting the class I was working on.

![A screenshot displaying a code editor with the JetBrains AI tool. It shows an auto-completion popup suggesting various code elements such as files, symbols, and functions.](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122924441/6d4110f4-989b-406b-b871-d199ad46c3aa.png align="center")

I really like that I can use autocomplete inside the chat. This allows me to create more detailed prompts where I can ask about multiple files or classes.

Once the prompt ran, it showed me several ways to improve the code and explained how each improvement was made.

\--IMAGE showing the how to improve -

![Screenshot of JetBrains AI, showing a chat window where the AI Assistant provides suggestions to improve code for better error handling, type safety, serialization, deserialization, and prefixing keys. The suggestions are accompanied by a sample code snippet.](https://cdn.hashnode.com/res/hashnode/image/upload/v1725122975480/031e150b-9824-4f01-9596-150e01b8a2e5.png align="center")

One thing I really liked is that the code is collapsible, has scrollbars, and even uses the same theme as the IDE. This makes it so easy to navigate and read.

---

## **Commit Code**

The code was well-documented, tested, and improved; the next step was to commit and Let It Rip! And to accomplish this last step, I also utilized JetBrains AI, which also offers a method to generate commit messages directly from the "Commit" tool window, as you can see in the screenshot below:

![Screenshot of a JetBrains AI tool generating commit messages for a code repository. The tool lists file changes and suggested commit messages for improvements and refactoring in the code. ](https://cdn.hashnode.com/res/hashnode/image/upload/v1725123108221/7a58cc46-61b3-4f39-8feb-8839017bce21.png align="center")

Here is the prompt I used:

```markdown
- Use Conventional Commit format
- Avoid overly verbose descriptions or unnecessary details.
- Start with a short sentence in imperative form, no more than 50 characters long.
- Then leave an empty line and continue with a more detailed explanation.
- Use bullet point for  multiple changes
```

## Extra - Creating a Story

I was feeling inspired and liked the results so far, so I was thinking that I could extend my usage of JetBrains AI by creating a custom prompt to create StoryBook Story for a component.

Below, you can see the prompt I created. You might notice it includes "$Selection." This tells JetBrains AI to use the selected code as the context for the prompt:

```markdown
- Create a StoryBook Story for $SELECTION
- The first story should be called `Default` and should not modify any input
- Avoid using template or HTML when defining a story
- Prefer modifying just inputs in the stories
- Add one story for each input 
- Do NOT add instructions about how to setup StoryBook
- Add all the stories
```

After selecting the component, right-clicking, and choosing the new prompt, an AI Chat tool window opened with the prompt and the story itself.

![Screenshot of JetBrains AI interface, featuring a TypeScript code prompt to create a StoryBook story, and its AI-generated response with example code.](https://cdn.hashnode.com/res/hashnode/image/upload/v1725123260573/80eaefec-ae38-4f86-a935-092ec10360bc.png align="center")

Once again, having a prompt library is a huge win since you can evolve it and generate the perfect results for your needs.

## Wishlist

I enjoyed working with JetBrains AI and I am excited to see how it improves with future releases. Here are some things on my wishlist for JetBrains AI:

* **Specify File Name**: I would like to specify the file name whenever the prompt generates a file. For example, when I generated unit tests, it used the prefix "tests.ts." It would be great to specify "specs.ts" or any other preferred name.
    
* **Create Files**: Having a prompt library is fantastic, but it would be even better if the prompt could generate a new file with a specified name format by default. This would be very helpful for generating unit tests, mock data files, stories, component tests, etc.
    
* **Better Code Replacement**: I would like it to be smarter about where to replace code. Sometimes, when you ask for something, it requires changes in multiple places. For example, it might need to add a new property in an interface, modify a method, and update the test. In these cases, it would be great to define which files to modify and which ones need more refinement.
    

## Conclusion

Both JetBrains AI Assistant and GitHub Copilot are designed to make coding easier with AI, but they each bring something unique to the table.

**JetBrains AI Assistant** is tightly woven into JetBrains' suite of IDEs. It offers a smooth and powerful experience with features like smart code completion, automatic documentation, AI-driven chat support, code refactoring, and error detection. This deep integration means it’s more context-aware and customizable, giving developers a tailored development environment.

**GitHub Copilot**, on the other hand, shines when it comes to code completion and suggestions, especially in Visual Studio Code. It pulls from a vast library of public code repositories to help generate code snippets and complete lines of code. However, it doesn’t offer the same level of IDE-specific features that JetBrains AI Assistant does, like automatic documentation and advanced refactoring tools. Also, in my opinion, Copilot creates better code and understands the context really well.

In short, JetBrains AI Assistant offers a more comprehensive toolkit for developers working in JetBrains IDEs. It's a powerful companion, particularly for those who appreciate a highly customizable and context-sensitive coding assistant.