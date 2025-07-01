---
date: 2025-07-01
categories:
  - AI
links:
  - My AI Product ②: blog/posts/my-ai-product-2.md
  - My AI Product ④: blog/posts/my-ai-product-4.md
---

# My AI Product ③

The previously mentioned JAA has been operating continuously for six months and has gained a stable user base. 
Tomorrow, I need to attend the company's OPEN AI DAY to share about JAA in Shanghai.

If it were half a year ago, I would have been very willing and had a lot to say, 
but now, I think it's already somewhat outdated.

<!-- more -->

After JAA had been running for a short while, I once thought about what other directions it could develop in:

First, the core of JAA is function call, so simply replacing the tools can turn it into a Jenkins AI Assistant, GitHub AI Assistant, and so on.

Second, perhaps I can package the tool part of my code into a Python library. 
Then others can download this library and develop applications that go beyond my existing framework (I'm using AutoGen).

While I was having these ideas (early December 2024), Anthropic introduced MCP at the end of the previous month. 
So I immediately went to GitHub to create a repo named `mcp-jira`, 
but I discovered that [mcp-atlassian](https://github.com/sooperset/mcp-atlassian) (Jira + Confluence) already existed and had 100 stars.
Instead, I contributed some code to that repository and discussed a few ideas with the author via GitHub.

At the same time, since I also needed to use Jenkins for work, 
I developed another project called [mcp-jenkins](https://github.com/lanbaoshen/mcp-jenkins).
Although there were other similar repo at the time, they had few stars, limited tool support, 
and I felt their engineering practices were lacking.

After developing my own MCP server, I abandoned the initial idea of replacing function calls with MCP.

I realized that MCP is better suited for providing general-purpose functionalities, 
while my JAA contains a lot of independent business logic that might change with API versions. 
From both a security and practicality perspective, it was not suitable to serve as an MCP server.
