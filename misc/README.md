# AWS-misc
Misc AWS learnings

Q. Process large no of messages from SQS. Order of messages doesn't matter.

A. Use EC2 instances in auto scaling group. Use multiple threads within each EC2 instance. Read messages in batches.

Q. How to manage cold start times of Lambda?

A. 1. Use more memory => leads to more CPU=> faster processing on initial load.
2. USe interpreted languages like Nodejs, Python etc, instead of languages like .net and java which have heavy processing JIT compilation etc on initial load.
3. Use less dependencies in your lambda code, and keep the function lightweight.
4. Use provisoned concurrency feature of Lambda.
5. a btach job to invoke Lambda at regular intervals with a dummy payload to keep it warm.

Q. Mobile app that supports offline sync when internet comes back.

A. Amplify Data store helps sync data on your device with AWS storage.

## GenAI with AWS

Q. How are new Foundation Models different from traditional models in AI?

A. You would need one traditional model each for respective use cases of text generation, summarisation, Q&A, chatbot etc. But with FMs, a single model is capable of doing it all.

Q. What are the Generative AI services provided by AWS?

A. 
1. **SageMaker JumpStart:** ML hub with FMs (Foundation models) and pre built ML solutions that you can deploy with a few clicks. You choose the model, instance type etc, and Jumpstart will launch a pre built model for your inference, and also generates Notebook for more control. Many pre built models created by 'Hugging Face' are also now available for various NLP use cases.
2. **Amazon BedRock**: A fully managed service that makes FMs from leading AI startups and Amazon available via an **API**, so you can choose from a wide range of FMs to find the model that is best suited for your use case. You can easily customize these models and call the API from your code/scripts. 
```
from langchain.llms import Bedrock

llm = Bedrock(
    credentials_profile_name="bedrock-admin",
    model_id="amazon.titan-text-express-v1"
)
```
Bedrock also help create chatbots, as it can help save a memory of the chat that has happened thus far.

3. **Amazon CodeWhisperer**: Plugin installed in your IDE will interact with Amazon CodeWhisperer and generate code suggestions for you. You just specify the problem statement as a comment in plain English, and code will be created for you along with unit test cases. Also performs security scanning and OWASP top 10 checks with suggested remediation. Multiple code suggestions are presented, and the developer can choose the best suggestion. Suggested code also tries to match coding style of the already present code.

4. **Stable diffusion**: Both AWS BedRock and Sagemaker Jumpstart support Stable diffusion mdoels. Diffusion models are used to generate image from a text. The process includes converting text into tokens, adding noise, and then de noise to generate the image.

5. **DJL**: DJL (Deep Java Library) is an open-source, high-level, Java-based deep learning framework that simplifies the development of machine learning and deep learning models for Java developers. It's designed to be user-friendly and provides support for various deep learning libraries, such as TensorFlow, PyTorch, and MXNet. AWS Sagemaker (as well as some other AWS services) supports use of DJL, making it easy to train models using DJL and then deploy them on SageMaker endpoints.

6. **DeepSpeed**: is an optimization library for deep learning to help engineers train large deep learning models faster and with less computational resources. AWS supports use of DJL along with DeepSpeed to serve LLM models with subsecond latency.

Q. What is Retreival Augmented Store (RAG)?

A. RAG is an AI framework for using outside model information with your LLMs. This reduces your overhead on retraining the LLM model with data available in the store. Eg. suppose your taks is to summarise an article with LLM. When you pass in the document as a text to LLM and ask it to summarise, you can run into the issue of allowed tokens for the input. Instead, you can have all your documents stored in a nowledge base, and let LLM refer the document from the store using a doucment context,a nd work on it. Same holds true if you want to use the model on some latest infromation, which is stored in the knowledge store. 
This data is stored as Vectors, supported on Aurora Postgres, RDS PostGres, and AWS Opensearch.

-----

**Fine tuning** a LLM model is a very taxing process. Hence first try these techniques of prompt engneering before you actually try to fine tune a LLM model:
1. Zero shot vs N-shot: Feed multiple similar questions with answers so that model can learn from them, and then ask your question.
2. Chain of thought prompting: QnA type of conversation to arrive at a conclusion.
3. Self consistency: By adhering to the principle of self-consistency, engineers and designers aim to create systems and products that are reliable, efficient, and free from internal contradictions. 
4. General knowledge: General knowledge in prompt engineering is foundational and serves as a basis upon which engineers can build their expertise and specialization. It ensures that engineers have the core knowledge required to understand and address the challenges and opportunities within their specific engineering domain.
5. ReAct(Reasoning and Action taking): Engineers first reason on how the system should behave, and then act on any deviations.
6. RAG 
 
If you still need to fine tune, then use **PEFT** technique, ie parameter efficient fine tuning. PEFT creates task specific pluggable adapters that can be plugged into original model.

**LLM Evaluation Metrics**:
1. **Rouge**: Used for text summarisation. Compare a summary to one or more reference summaries.
2. **Blue Score**: Used for text translation. compare to human generated translations.

**Semantic caching** is a technique that can be used to improve the performance of large language model (LLM) applications while reducing expenses1. It involves storing precomputed representations of frequently used language elements in a cache, which can significantly reduce the time it takes to retrieve data, reduce API call expenses, and improve scalability.

Techniques to improve LLM results in increasing order of cost and time:
1. Prompt engineering
2. Use RAG
3. Fine tune the model to your domain use case (eg. LoRA technique)
4. Train the LLM model from scratch (v. expensive)

**RHLF**: Reinforcement Learning from Human Feedback (RLHF) is a technique that combines reinforcement learning with human feedback to improve the performance of large language model (LLM) applications.

**Vector databases** are a powerful solution for managing unstructured data types including text, audio, images, and videos in numerical form. They have gained traction in the AI sphere due to their ability to efficiently manage similarity searches across thousands of columns. Vector databases store and provide access to structured and unstructured data, such as text or images, alongside their vector embeddings. Vector embeddings are the data’s numerical representation as a long list of numbers that captures the original data object’s semantic meaning. Because similar objects are close together in vector space, the similarity of data objects can be calculated based on the distance between the data object’s vector embeddings. This opens the door to a new type of search technique called vector search that retrieves objects based on similarity. While many traditional databases support storing vector embeddings to enable vector search, vector databases are AI-native, which means they are optimized to conduct lightning-fast vector searches at scale. 

Redis Enterprise cloud now supports vector data, and can be used to cache common context searches in RAG db. 
AWS Bedrock now supports Redis Enterprise as the data store, along with other options of Pinecone and Open search.
