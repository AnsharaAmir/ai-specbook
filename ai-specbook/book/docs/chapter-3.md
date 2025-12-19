# Chapter 3: Writing Your First Spec with Spec-Kit Plus
    6 
    7 In Chapter 1, we introduced the idea of "Spec-Driven Development"—creating a blueprint for our software before 
      build it. Now, it's time to learn how to draw that blueprint. In this chapter, we'll dive into the "spec-first"      methodology and use a tool called **Spec-Kit Plus** to write our very first specification.
    8 
    9 ## The Spec-First Methodology
   10 
   11 The spec-first methodology is simple but powerful: **write the spec, then write the code.**
   12 
   13 Think back to our house analogy. You wouldn't want your plumber and your electrician showing up and just guessi      where the pipes and wires should go. You'd want them to follow the blueprint.
   14 
   15 In software, the spec is our blueprint. It defines:
   16 - **What** our software should do.
   17 - **How** its different parts should interact.
   18 - **What** data it should expect and what data it should return.
   19 
   20 By writing the spec first, we catch design problems early, ensure everyone is on the same page, and create a   
      clear guide for our implementation.
   21 
   22 ## Introducing Spec-Kit Plus
   23 
   24 Spec-Kit Plus is a simple yet effective way to write specs. It's not a complex piece of software; it's a       
      convention that combines two languages you already know: **Markdown** and **YAML**.
   25 
   26 - **Markdown:** For the human-readable, narrative part of the spec. This is where you explain the *why* and the      *what* in plain English.
   27 - **YAML:** For the structured, machine-readable part of the spec. This is where you define the precise details      like function names, parameters, and data types.
   28 
   29 This combination allows the spec to be both a document for humans and a parseable artifact for tools.
   30 
   31 ## Our First Spec: A `GeminiModel` Client
   32 
   33 Let's create a spec for a Python class that will be the heart of our AI application: a client for the Gemini   
      language model. We'll call it `GeminiModel`.
   34 
   35 Here's the full spec. Don't worry, we'll break it down piece by piece.
  Spec: GeminiModel Client

  1. Overview

  This document specifies the functionality for the GeminiModel class, a Python client designed to interact with the 
  Google Gemini API. This class will handle single and parallel API calls, manage configurations, and include error  
  handling with retries.

  2. Class Definition

   1 class: GeminiModel
   2 description: A Python class for interacting with the Google Gemini large language model.

  3. Methods

  3.1. __init__ (Constructor)

  Description: Initializes the GeminiModel instance. It sets up the model name, configuration, and other parameters
  for API calls.

    1 method: __init__
    2 description: Constructor for the GeminiModel class.
    3 parameters:
    4   - name: model_name
    5     type: str
    6     required: false
    7     default: "gemini-1.5-flash"
    8     description: The name of the Gemini model to use.
    9   - name: temperature
   10     type: float
   11     required: false
   12     default: 0.01
   13     description: The sampling temperature for the model's output (controls randomness).
   14   - name: finetuned_model
   15     type: bool
   16     required: false
   17     default: false
   18     description: Flag to indicate if the model is a fine-tuned version.
   19   - name: distribute_requests
   20     type: bool
   21     required: false
   22     default: false
   23     description: Flag to distribute requests across different regions for better availability.

  3.2. call

  Description: Makes a single, synchronous call to the Gemini API with a given prompt. Includes automatic retry
  logic.

    1 method: call
    2 description: Makes a single call to the Gemini model.
    3 parameters:
    4   - name: prompt
    5     type: str
    6     required: true
    7     description: The text prompt to send to the model.
    8   - name: parser_func
    9     type: callable
   10     required: false
   11     description: An optional function to parse the model's raw text output.
   12 returns:
   13   - name: response
   14     type: str
   15     description: The processed text response from the model.

  3.3. call_parallel

  Description: Makes multiple calls to the Gemini API in parallel using a thread pool for efficiency.

    1 method: call_parallel
    2 description: Makes multiple parallel calls to the Gemini model.
    3 parameters:
    4   - name: prompts
    5     type: List[str]
    6     required: true
    7     description: A list of text prompts to send to the model.
    8   - name: timeout
    9     type: int
   10     required: false
   11     default: 60
   12     description: The timeout in seconds for each parallel request.
   13 returns:
   14   - name: responses
   15     type: List[Optional[str]]
   16     description: A list of text responses, with `None` for any failed requests.

    1 
    2 ### Breaking Down the Spec
    3 
    4 - **The Narrative (Markdown):** The headings (`## 1. Overview`) and the **Description** fields are written in  
      Markdown. They provide context and explain the purpose of each part of the class in simple terms.
    5 - **The Structure (YAML):** The `class`, `method`, `parameters`, and `returns` sections are written in YAML,   
      inside Markdown code blocks. This gives us a structured, easy-to-read definition of our code's "shape."        
    6 
    7 :::tip
    8 This spec is now our **single source of truth**. When we write our Python code, we will follow this blueprint  
      exactly. If we decide to change how the `GeminiModel` works, we'll update the spec *first*.
    9 :::
   10 
   11 ## What's Next?
   12 
   13 Congratulations on writing your first spec! You've taken a huge step towards building more robust and 
      maintainable software.
   14 
   15 In the next chapter, we'll take this spec and use it to write the actual Python code for our `GeminiModel` clas      We'll see how having a clear spec makes the coding process faster, easier, and more enjoyable.