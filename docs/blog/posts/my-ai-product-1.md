---
date: 2025-06-20
categories:
  - AI
links:
  - My AI Product ②: blog/posts/my-ai-product-2.md
---

# My AI Product ①

In October 2024, under the wave of AI, 
my main development work shifted from the automation testing framework to the development of AI products.
At this point, I only use AI as a search engine.

Base on my experience with the automation testing framework, 
the first AI product I want to develop is an AI Code generator, it primarily focuses on the generation of automated test scripts. 
And I named it Step-Copilot.  

<!-- more -->

## Implementation

In our workflow, we manage the test cases on Jira via Xray plugins, 
and I create a script to automatically generate test scripts template which contain the 
test action and expected result based on the test cases in Jira.

Compared to widely known code assistants like GitHub Copilot. 
I only expect Step-Copilot to be able to generate the implementation code for action and expected result 
base on the code which tester has written in the past. 

So, I write a simple script to export the test scripts from our repo via AST, and format them into a JSON file.
The structure of the JSON file is like this:

```json
[
    {
        "input": "<action>Test Action</action><expected>Test Expected Result</expected>",
        "output": "The code for the action and expected result"
    }
]
```

With the help of other professional AI engineers 
(Without them, I think it would be very difficult for me to get started with AI application development.),
I use the above JSON file to fine-tune a pre-trained LLM model and integrate it as a service with my script using FastAPI.

## Issues

This model performs exceptionally well in generating non-UI operation code. 
For example, we need to use the `play_voice` method to play a voice in the test script,
Step-Copilot can effectively set different parameters based on different actions.

However, when generate UI operation code, it often hallucinates and calls non-existent methods.
Although I have many ideas to improve it, I still stopped the development of Step-Copilot at this point.

## Reflection

The entire product development, testing, and user feedback collection took approximately 15 working days.
During this process, I divided the code generation into three levels of difficulty:

1. **Very Easy**: write a bubble sort algorithm, a fixed function where only the parameters change 
(and the accepted parameters do not include complex objects). Or some fixed, repetitive code. 
These are pieces of code that people can instantly verify for correctness.
2. **Moderate**: Code that both humans and AI can generate, 
but the AI's output is difficult to verify at a glance—requiring more time to validate than writing it manually.
3. **Complex**: Complex or difficult code, or when one lacks the knowledge base. For example: 
Asking a Python engineer to develop an Android application.

Writing the UI test scripts is a moderate level task,
rather than waiting for AI to complete the script and then debug it, it's better to just develop it directly.
So, I stopped it.

In my opinion, this was a failed project, but fortunately, 
it didn't take much time, and through it, I gained some understanding of AI.
