# Using Amazon Bedrock

## Using a specific model

To use a specific model, you need to: 

1. Make sure the model is activated. You can do that in the Bedrock UI through `Bedrock Configurations > Model access`. <br>
You just need to request access to the models you choose. 

2. In python, you should use, as `model_id` the Id of the **Inference Profile**. <br>
You can find that Id in Bedrock under `Inference and Assessment > Cross-region inference`. <br>
In that view, you'll see a list of *Inference Profiles* and there you should copy the *Inference Profile Id*. Use it as follows in Python: 
```
# Client - Use the right region
client = boto3.client("bedrock-runtime", region_name="us-east-1")

# Here you put the Inference Profile Id
model_id = 'us.anthropic.claude-3-5-haiku-20241022-v1:0' 

# Invoke the model
conversation = [
    {
        "role": "user", 
        "content": [{"text": system_prompt}]
    },
]

# Send the message to the model, using a basic inference configuration.
response = client.converse(
    modelId=model_id,
    messages=conversation,
    inferenceConfig={"maxTokens": 2000, "temperature": 0.3, "topP": 0.9},
)

# Extract the response
response_text = response["output"]["message"]["content"][0]["text"]
```

#### Important Note
Models have **different quotas** on the different regions. <br>
EU is **particularly poorly served** so using US East is in general much better. 

Different models also have different quotas, so make sure to check which model fits the best or use a combination of models.