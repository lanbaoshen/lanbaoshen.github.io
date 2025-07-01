---
date: 2025-06-21
categories:
  - AI
links:
  - My AI Product ①: blog/posts/my-ai-product-1.md
  - My AI Product ③: blog/posts/my-ai-product-3.md
---

# My AI Product ②

In the previous post, I introduced `Step-Copilot`, and I stop the development of it at November 2024.
Then, I thought how great it would be if AI could call the tools we had developed? 

We had previously built many tools, such as batch operations for Jira, Jenkins maintenance, and others, 
which provided services to users through GUIs or web interfaces. 
But regardless of the form, there is always some learning cost involved.

Even with comprehensive documentation provided, 
most users would likely ignore it and choose to directly ask the tool developers instead.

It would be incredibly helpful to have a AI that not only uses all the tools 
but also teaches users which scenarios to apply each tool in.

Based on this idea, I developed the **JAA**

<!-- more -->

## Implementation

JAA is based on [AutoGen](https://github.com/microsoft/autogen/), a Microsoft open-source multi-agent framework.

AutoGen make it easy to build AI agents, and it can easily register tools for the Agent, just like the following code:

```python hl_lines="10"
from autogen_agentchat.agents import AssistantAgent

def get_weather(city: str) -> str:
    return f"The weather in {city} is sunny with a high of 25°C."

agent = AssistantAgent(
        name='weather_agent',
        model_client=replace_with_your_model_client,
        # reflect_on_tool_use=True,  # If the tool's response is not in natural language, the AI will organize the returned data into a structured format.
        tools=[get_weather],
)
```

I registered a series of tools for JAA via python lib `atlassian-python-api`.  
Note, the methods provided by `atlassian-python-api` cannot be directly used by the Agent; 
annotations and parameter definitions need to be supplemented.

I also specifically declared a tool for generating ECharts charts:
```python
def save_echarts_chart(html: str) -> str:
    """
    Save echarts chart to file and return the url.

    Args:
        html: Echarts html content

    Returns:
        Url of the saved chart
    """
    html_file = 'xxxx'
    with open(html_file, 'w') as f:
        f.write(html)
    return html_file
```

With the tool in place, JAA allows users to organize complex Jira operations through natural language, for example:

User say: `Obtaining the status field of all issues in the active sprint of the Board 'XXX' and displaying it with a Pie Chart.`

JAA will complete the task by following these steps:

1. Search the board by user input name, and get the board ID. 
Since the input includes "active sprint" the agent will select the Agile board when multiple boards are queried.
2. Get the active sprint ID of the board.
3. Query the target issues fields via JQL tool.
4. Generate the ECharts chart HTML code based on the issues data.
5. Return the result to the user.

## Challenges

The process may seem simple, but in reality, JAA took me nearly two months to complete. 

Apart from developing the frontend to provide a better user experience 
(which relied heavily on AI, in the early stages, to avoid frontend development, I built it as a command-line application.),
most of my time was spent on tool development and optimization. 

That's right—it was not just about registering tools for the Agent and calling it a day.

During the initial launch phase, the most common issue I encountered was exceeding the token limit. 

Since each API returned a large amount of data, I had to retain only the most critical fields myself. 
For example, in Jira, a user's information includes not just the `display name` and `short name` but also a URL for their avatar.
Clearly, the URL was practically useless, so it had to be removed before the tool returned the data to the AI.

After spending a significant amount of time optimizing the tools, JAA can now complete user tasks faster and more accurately,
while also supporting more context (operations) in a single session.

Another challenge is that users often expect a single sentence to trigger an entire business process. 
If you only provide the most basic tools (APIs), the time spent crafting prompts can leave users feeling hopeless.

To address this, I encapsulate business process tools for power users, allowing them to choose whether to load these on the web page. 

Additionally, I provide a template feature, enabling users to save their own prompt templates or directly use the ones I’ve prepared for them, eliminating the tedious copy-paste routine.

For a long time after the launch, I made it a habit to spend time every day reviewing what users asked and how JAA responded. 
I would then meet with users in person to discuss how to optimize it or identify any additional features they needed.

## Other

Finally, here’s an idea I think works really well: I registered two tools, `Report Requirement` and `Report Bug`. 
Users can simply tell the Agent: "Submit a bug/requirement to the author" and it will organize the content based on our conversation. 
This makes it incredibly convenient for users to provide feedback to me.
