# Personal Notes - Open AI

- [Course Outline Notes](course-notes.md)

## Text Generation

```JAVASCRIPT
import OpenAI from "openai";

const openai = new OpenAI({ apiKey: 'YOURKEYHERE' });



async function main() {
  const completion = await openai.chat.completions.create({
    messages: [{"role": "system", "content": "You are a helpful assistant with a informal funny personality."},
        {"role": "user", "content": "Who won the super bowl in 2020?"},
        {"role": "assistant", "content": "The Kansas City Chiefs won the super bowl in 2020."},
        {"role": "user", "content": "How many passing yards did Patrick Mahomes complete?"}],
    model: "gpt-3.5-turbo",
  });

  console.log(completion.choices[0]);
}

main();
```

- The main input is the `messages` parameter.
- The 'messages' parameter must be an array of objects, where each object ahs a role. 'system', 'user', or 'assistant'
- Conversations can be as short as one message or many back and forth turns.
- Typical format, 'system' message first, followed by alternating 'user' and 'assistant' messages.

- System messages, sets the behavior of the assistant. You can set the personality of the assistant or provide specific instructions about how it should behave in the conversation.
- User messages, provide requests or comments for the assistant to respond to.
- Assistant messages, sore previous assistant responses, but can also be written by you to give examples of desired behavior.

- Including conversation history is important when user instructions refer to prior messages.
- In the example above, 'How many passing yards' only makes sense in the context of the prior messages about the Super Bowl.

## Function Calling

In an API call, you can describe functions and have that model choose to output a JSON object that contains arguments to call one or many functions.

The 'Chat Completions API' does not call the function; instead, the model generates JSON that you can use to call the function in your code.

The latest models, `gpt-3.5-turbo-0125` and `gpt-4-turbo-preview`, have been trained to both detect when a function should be called and respond with JSON that adheres to the function signature more closely than previous models.

✋ Recommend building in user confirmation flows before taking actions on behalf of users (sending email, posting something online etc.)

### Common Use Cases

Function calling allows you to more reliably get structured data back from the model:

- Create assistants that answer questions by calling external APIs.
  - For example; `send_email(to: string, body: string)` or `get_current_weather(location: string, unit: 'celsius' | 'fahrenheit')`

Basics sequence for function calling:

1. Call the model with the user query and a set of functions defined in the functions parameter.
2. The model can choose to call one or more functions; the content will be a stringified JSON object.
3. Parse the string into JSON in your code and call your function with the provided arguments if they exists.
4. Call the model again by appending the function response as a new message, and le the model summarize the results back to the user.

- Not all model versions are trained with function calling. See [supported models](https://platform.openai.com/docs/guides/function-calling/supported-models).

### Parallel Function Calling

The ability to perform multiple function calls together, allowing the effects and results to be resolved in parallel.

💡 Cookbook: [How to build an agent with the OpenAI Node.js SDK](https://cookbook.openai.com/examples/how_to_build_an_agent_with_the_node_sdk)

## Guides - Prompt Engineering
