---
date: 2025-07-17
categories:
  - AI
links:
  - My AI Product ③: blog/posts/my-ai-product-3.md
---

# My AI Product ④

In the entire automated testing process, the most time-consuming tasks are maintaining UI changes and analyzing test reports.
So, I want AI to analyze the test results to assist testers in improving the efficiency of report analysis.

This project is named RAA, base on AutoGen.

<!-- more -->

## Measurement

I believe that confirming the metrics is the top priority before starting development. 

The most straightforward aspect is how to reflect efficiency improvements. 
Directly asking developers whether time was saved is the ugliest approach, especially when perceived efficiency improves but actually takes more time.

Since our automated testing infrastructure is well-established with a dedicated platform where report analysis is completed directly, 
I can use tracking points to measure how long it takes for developers from starting the analysis to sending the report.

!!! Warning 
    The quality of AI performance and efficiency improvements are not directly linearly correlated.

We also need additional metrics to evaluate the performance of AI analysis, 
which will guide future optimization efforts. 

In my case, the analysis conclusion of a failed case must include the **category**, 
**issue link** (we use Jira for management), and a **comment** of the issue.

After the dev sends the report, the actual values from the user report and the AI's values will be compared to calculate a similarity rate.
Based on this similarity rate, I can identify the areas where the AI's performance is lacking.


## Design

It is necessary to clarify what kind of data analysis the AI should be based on and how to provide the data to the AI:
1. Screenshot of the device when it fails.
2. By combining the stack trace at the time of failure and the information from the Jira case, 
determine what needs to be done, how to do it, and what errors occurred during execution.
3. Historical analysis results can help the AI understand relevant defects and learn user preferences.
4. Developers can customize rules and analysis preferences for the AI.

For security considerations, I chose a local 7B multimodal model, MINICPM-V, for handling screenshots. 
To improve the accuracy of image recognition and the validity of the results, I fine-tuned this model by annotating some data myself.

The case information on Jira can be retrieved via the official API. 
To optimize requests to other platforms, I used Redis to cache certain data. 
For example, case information doesn't change frequently, so it's stored for 30 days, 
while defect tickets, which require higher timeliness, are cached for 12 hours.
I input the failed case script code and the actual retrieved information into my custom agent, which then summarizes:
1. What the test case was expected to do
2. How the code implemented it
3. Which steps succeeded
4. Which steps failed, and the reasons for the failures

Historical analysis results are stored in a database. 
During AI analysis, relevant results are queried based on similarity (case key + failed code line + exception message) for reference. 
This is implemented using AutoGen's ChromaDB memory, with Redis as an optimization layer. 
The result indices are stored in Redis and expire if unused for 15 days (or after at least two analysis cycles), 
after which they are deleted from the database.

I provide users with an entry to write their own prompts per test application. 
When analyzing issues, the application fetches these prompts dynamically, which is also implemented using memory.


## Efficiency

The project is already live. Without optimizing individual applications, 
the average similarity rate fluctuates between **40%** and **75%**.
Additionally, as the user's analysis history grows, the similarity rate shows a steady improvement.

Based on time statistics, the approximate time saved is around **20%**.
