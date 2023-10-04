# AWS-misc
Misc AWS learnings

Q. Process large no of messages from SQS. Order of messages doesn't matter.
A. USe EC2 instances in auto scaling group. Use multiple threads withn each EC2 instance. Read messages in batches.

Q. How to manage cold start times of Lambda?
A. 1. Use more memory => leads to more CPU=> faster processing on initial load.
2. USe interpreted languages like Nodejs, Python etc, instead of languages like .net and java which have heavy processing JIT compilation etc on initial load.
3. Use less dependencies in your lambda code, and keep the function lightweight.
4. Use provisoned concurrency feature of Lambda.
5. a btach job to invoke Lambda at regular intervals with a dummy payload to keep it warm.

Q. Mobile app that supports offline sync when internet comes back.
A. Amplify Data store helps sync data on your device with AWs storage.

Q. What are the Generative AI services provided by AWS?
A. 1. SageMaker JumpStart
2. Amazon BedRock: A fully managed service that makes FMs (Foundation models) from leading AI startups and Amazon available via an API, so you can choose from a wide range of FMs to find the model that is best suited for your use case. You can easily customize these models and call the API from your code/scripts. 
```
from langchain.llms import Bedrock

llm = Bedrock(
    credentials_profile_name="bedrock-admin",
    model_id="amazon.titan-text-express-v1"
)
```
Bedrock also help create chatbots, as it can help save a memory of the chat that has happened thus far.

3. Amazon CodeWhisperer: Plugin installed in your IDE will interact with Amazon CodeWhisperer and generate code suggestions for you. You just specify the problem statement as a comment in plain English, and code will be created for you along with unit test cases. Also performs security scanning and OWASP top 10 checks with suggested remediation. Multiple code suggestions are presented, and the developer can choose the best suggestion. Suggested code also tries to match coding style of the already present code.
4. 
5. 

Q. What is Retreival Augmented Store (RAG)?
A. RAG is an AI framework for using outside model information with your LLMs. This reduces your overhead on retraining the LLM model with data available in the store. Eg. suppose your taks is to summarise an article with LLM. When you pass in the document as a text to LLM and ask it to summarise, you can run into the issue of allowed tokens for the input. Instead, you can have all your documents stored in a nowledge base, and let LLM refer the document from the store using a doucment context,a nd work on it. Same holds true if you want to use the model on some latest infromation, which is stored in the knowledge store. 
This data is stored as Vectors, supported on Aurora Postgres, RDS PostGres, and AWS Opensearch.


 