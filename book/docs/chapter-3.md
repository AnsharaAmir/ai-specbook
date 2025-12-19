---
sidebar_position: 3
---

# Chapter 3: Writing Your First Spec with Spec-Kit Plus

In Chapter 1, we introduced the idea of "Spec-Driven Development"—creating a blueprint for our software before we build it. Now, it's time to learn how to draw that blueprint. In this chapter, we'll dive into the "spec-first" methodology and use a tool called **Spec-Kit Plus** to write our very first specification.

## The Spec-First Methodology

The spec-first methodology is simple but powerful: **write the spec, then write the code.**

Think back to our house analogy. You wouldn't want your plumber and your electrician showing up and just guessing where the pipes and wires should go. You'd want them to follow the blueprint.

In software, the spec is our blueprint. It defines:
- **What** our software should do.
- **How** its different parts should interact.
- **What** data it should expect and what data it should return.

By writing the spec first, we catch design problems early, ensure everyone is on the same page, and create a clear guide for our implementation.

## Introducing Spec-Kit Plus

Spec-Kit Plus is a simple yet effective way to write specs. It's not a complex piece of software; it's a convention that combines two languages you already know: **Markdown** and **YAML**.

- **Markdown:** For the human-readable, narrative part of the spec. This is where you explain the *why* and the *what* in plain English.
- **YAML:** For the structured, machine-readable part of the spec. This is where you define the precise details, like function names, parameters, and data types.

## Our First Spec: A `GeminiModel` Client

Let's create a spec for a Python class that will be the heart of our AI application: a client for the Gemini language model. We'll call it `GeminiModel`.

Here's the full spec. Don't worry, we'll break it down piece by piece.

````markdown
# Spec: GeminiModel Client

## 1. Overview

This document specifies the functionality for the `GeminiModel` class, a Python client designed to interact with the Google Gemini API. This class will handle single and parallel API calls, manage configurations, and include error handling with retries.

## 2. Class Definition

```yaml
class: GeminiModel
description: A Python class for interacting with the Google Gemini large language model.
```

## 3. Methods

### 3.1. `__init__` (Constructor)

**Description:** Initializes the `GeminiModel` instance. It sets up the model name, configuration, and other parameters for API calls.

```yaml
method: __init__
description: Constructor for the GeminiModel class.
parameters:
  - name: model_name
    type: str
    required: false
    default: "gemini-1.5-flash"
    description: The name of the Gemini model to use.
  - name: temperature
    type: float
    required: false
    default: 0.01
    description: The sampling temperature for the model's output (controls randomness).
  - name: finetuned_model
    type: bool
    required: false
    default: false
    description: Flag to indicate if the model is a fine-tuned version.
  - name: distribute_requests
    type: bool
    required: false
    default: false
    description: Flag to distribute requests across different regions for better availability.
```

### 3.2. `call`

**Description:** Makes a single, synchronous call to the Gemini API with a given prompt. Includes automatic retry logic.

```yaml
method: call
description: Makes a single call to the Gemini model.
parameters:
  - name: prompt
    type: str
    required: true
    description: The text prompt to send to the model.
  - name: parser_func
    type: callable
    required: false
    description: An optional function to parse the model's raw text output.
returns:
  - name: response
    type: str
    description: The processed text response from the model.
```

### 3.3. `call_parallel`

**Description:** Makes multiple calls to the Gemini API in parallel using a thread pool for efficiency.

```yaml
method: call_parallel
description: Makes multiple parallel calls to the Gemini model.
parameters:
  - name: prompts
    type: List[str]
    required: true
    description: A list of text prompts to send to the model.
  - name: timeout
    type: int
    required: false
    default: 60
    description: The timeout in seconds for each parallel request.
returns:
  - name: responses
    type: List[Optional[str]]
    description: A list of text responses, with `None` for any failed requests.
```
````

### Breaking Down the Spec

- **The Narrative (Markdown):** The headings (`## 1. Overview`) and the **Description** fields are written in Markdown. They provide context and explain the purpose of each part of the class in simple terms.
- **The Structure (YAML):** The `class`, `method`, `parameters`, and `returns` sections are written in YAML, inside Markdown code blocks. This gives us a structured, easy-to-read definition of our code's "shape."

:::tip
This spec is now our **single source of truth**. When we write our Python code, we will follow this blueprint exactly. If we decide to change how the `GeminiModel` works, we'll update the spec *first*.
:::

## What's Next?

Congratulations on writing your first spec! You've taken a huge step towards building more robust and maintainable software.

In the next chapter, we'll take this spec and use it to write the actual Python code for our `GeminiModel` class. We'll see how having a clear spec makes the coding process faster, easier, and more enjoyable.