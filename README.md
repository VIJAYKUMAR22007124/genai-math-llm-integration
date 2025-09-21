## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for calculating addition of two numbers, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:

#### STEP 1:
Import the required libraries (json, openai) and define the function add_numbers(a, b) that takes two numbers and returns their sum in JSON format.

#### STEP 2:
Describe the function for the LLM using a JSON schema. This schema includes the function’s name, description, and parameters (two numbers: a and b).

#### STEP 3:
Send a user query to the ChatCompletion API (e.g., “Can you add 15 and 27?”). The LLM analyzes the query, triggers the add_numbers function with the provided arguments, and returns the result.

### PROGRAM:

```
import json
import openai

# Define your own function
def add_numbers(a, b):
    return json.dumps({"a": a, "b": b, "sum": a + b})

# Create the function schema for OpenAI
functions = [
    {
        "name": "add_numbers",
        "description": "Add two numbers together",
        "parameters": {
            "type": "object",
            "properties": {
                "a": {"type": "number", "description": "The first number"},
                "b": {"type": "number", "description": "The second number"},
            },
            "required": ["a", "b"],
        },
    }
]

# User message
messages = [
    {"role": "user", "content": "Can you add 15 and 27?"}
]

# Ask the model
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)

# Extract function call
response_message = response["choices"][0]["message"]
print("Model response:", response_message)

# If function call exists, run it
if "function_call" in response_message:
    args = json.loads(response_message["function_call"]["arguments"])
    result = add_numbers(**args)
    print("Function result:", result)

```

### OUTPUT:

<img width="525" height="259" alt="image" src="https://github.com/user-attachments/assets/b485c49b-55ab-495f-8de1-6e1dedaab0cc" />


### RESULT:  

The function add_numbers(a, b) was successfully integrated with the ChatCompletion system, and for inputs a = 15 and b = 27, it returned the sum 42.
