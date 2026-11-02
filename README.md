

### AIM:
To design and implement a Python function for calculating the volume of a cylinder, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:

### DESIGN STEPS:
1. Identify the Problem – Understand the problem.
2. Collect Information – Gather the required details.
3. Plan the Design – Prepare a design plan.
4. Develop the Design – Create and implement the design.
5. Test and Evaluate – Check the design and correct errors.

### PROGRAM:
```python
import os
import json
import math
from dotenv import load_dotenv, find_dotenv
from openai import OpenAI

# Load API key
load_dotenv(find_dotenv())
client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

# Define the mathematical function
def calculate_hexagon_area(side):
    """Calculate the area of a regular hexagon."""
    area = (3 * math.sqrt(3) / 2) * side**2

    return json.dumps({
        "side": side,
        "area": round(area, 2)
    })

# Define the tool
tools = [
    {
        "type": "function",
        "name": "calculate_hexagon_area",
        "description": "Calculate the area of a regular hexagon",
        "parameters": {
            "type": "object",
            "properties": {
                "side": {"type": "number"}
            },
            "required": ["side"],
            "additionalProperties": False
        },
        "strict": True
    }
]

# Ask the question
response = client.responses.create(
    model="gpt-5.5",
    input="Calculate the area of a regular hexagon with side length 5.",
    tools=tools,
    tool_choice="required"
)

# Process the tool call
for item in response.output:
    if item.type == "function_call":
        args = json.loads(item.arguments)

        result = calculate_hexagon_area(**args)

        print("Function arguments:", args)
        print("Calculated area:", result)
```


### OUTPUT:
<img width="1437" height="770" alt="image" src="https://github.com/user-attachments/assets/a1727528-bc57-4f4f-b93e-1f982c7d5931" />


### RESULT:
The design was completed successfully, tested, and evaluated.

