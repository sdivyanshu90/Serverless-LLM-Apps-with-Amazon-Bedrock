# Serverless LLM Apps with Amazon Bedrock

This course, created by **[DeepLearning.AI](https://learn.deeplearning.ai/)** in collaboration with **[Amazon Web Services (AWS)](https://aws.amazon.com/)**, introduces how to build Serverless applications that use large language models (LLMs) on Amazon Bedrock. Below is a detailed explanation of the course topics and sub-topics.

---

## 1. First Generations with Amazon Bedrock

This section introduces how to perform the first inference with Amazon Bedrock. You will learn how to connect to the Bedrock runtime and send a prompt to an available foundation model (FM). Bedrock provides a fully managed API to interact with multiple foundation models without needing to manage infrastructure or train models.

The process generally involves:

* Initializing the Bedrock runtime client.
* Choosing a foundation model (e.g., Anthropic Claude, Amazon Titan, or AI21).
* Building a text prompt.
* Sending the prompt to the model and capturing the output.

This gives the foundation for all later tasks like summarization, Q&A, and custom workflows.

---

## 2. Summarize an Audio File

This section shows how to take an audio file, transcribe it into text, and then summarize it using Bedrock. It combines multiple AWS services — **Amazon S3**, **Amazon Transcribe**, and **Amazon Bedrock**.

### Sub-topics

#### Import packages and load the audio file

* Install and import necessary Python packages like `boto3` (AWS SDK for Python).
* Load a sample audio file that you want to process.

#### Setup:

a. **S3 client**

* Initialize the Amazon S3 client to upload and manage audio files.
  b. **Transcribe client**
* Initialize the Amazon Transcribe client to convert audio into text.

#### Upload the audio file to S3

* Audio must be available on S3 before Transcribe can access it.
* Upload the file programmatically or manually to your designated bucket.

#### Create the unique job name

* Transcribe jobs need unique identifiers.
* Generate a job name, often based on a timestamp or UUID.

#### Build the transcription response

* Start the transcription job using the Transcribe client.
* Wait for the job to finish and retrieve the transcription result (JSON format).

#### Access the needed parts of the transcript

* Extract the main text body from the transcription JSON output.
* Clean and format the transcript for better processing.

#### Setup Bedrock runtime

* Initialize the Bedrock runtime client.
* Choose the foundation model that will generate the summary.

#### Create the prompt template

* Design a structured prompt, e.g., *“Summarize the following transcript in 3–4 sentences:”*.
* Insert the transcript text into the prompt.

#### Configure the model response

* Specify model parameters such as temperature, maximum tokens, or top-p sampling.
* This controls creativity, length, and determinism of the output.

#### Generate a summary of the audio transcript

* Send the prompt to Bedrock.
* Capture and display the generated summary.

---

## 3. Deploy an AWS Lambda Function

This section demonstrates how to package the workflow into a **serverless function** using AWS Lambda. Lambda allows you to run your summarization pipeline without provisioning servers, making it scalable and cost-effective.

### Sub-topics

#### Setup:

a. **Import packages**

* Include `boto3` and other helper libraries needed inside the Lambda code.
  b. **Access the helpers functions**
* Define helper functions for tasks like calling Bedrock or extracting transcript text.

#### Build the prompt template

* Reuse the earlier summarization prompt template, but integrate it inside Lambda.
* Ensure it dynamically accepts input text.

#### Create a Lambda function:

a. **Lambda handler**

* Define the main entry point function `lambda_handler(event, context)`.
* This function processes incoming events.
  b. **Extract transcript from text**
* Parse the event input to get transcript text or location.
  c. **Summarization using Bedrock**
* Pass the transcript text through Bedrock for summarization.
* Return the summarized output as the Lambda response.

#### Deploy the Lambda function

* Package and upload the function to AWS Lambda.
* Set runtime (Python, Node.js, etc.) and configure IAM permissions to access S3, Transcribe, and Bedrock.

#### Perform some testing

* Trigger the Lambda function manually or via a test event.
* Confirm that input transcripts are summarized successfully.

---

## 4. Event-driven Generation

This section focuses on making the summarization **event-driven**. Instead of manually invoking Lambda, you connect it to AWS event sources such as:

* **S3 events**: Trigger when a new audio file is uploaded.
* **Step Functions**: For chaining multiple steps in workflows.
* **API Gateway**: To expose the function as an API endpoint.

By doing so, the summarization pipeline becomes automated. For example:

1. An audio file is uploaded to S3.
2. The event triggers the Lambda function.
3. Transcribe processes the audio.
4. Bedrock generates a summary.
5. The final result is stored back in S3 or returned via an API.

This completes a fully serverless, event-driven application with Amazon Bedrock.

---

## ✅ Acknowledgment

**DeepLearning.AI** creates this course in collaboration with **Amazon Web Services (AWS)**. All credit for course design and materials belongs to the original authors.

---

## 💡 Tips for Success

* Familiarize yourself with **AWS IAM roles and permissions** — misconfigured roles are the most common source of errors.
* Use **smaller audio samples** when testing transcription to avoid long processing times.
* Always set **unique job names** in Transcribe to prevent conflicts.
* Experiment with **different foundation models** in Bedrock (Claude, Titan, etc.) to see how summarization varies.
* Monitor **CloudWatch logs** for Lambda to quickly debug issues.
* When building event-driven workflows, start with a **manual trigger**, then scale to event automation.
