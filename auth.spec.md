# Authentication, Profiling, and Personalization Specification

## 1. Overview

This document outlines the specification for the authentication, user profiling, and personalization systems. The platform supports multiple authentication schemes to cater to different use cases, including programmatic API access and user-centric instance registration. User profiling and personalization are achieved by capturing registration data and leveraging user-specific interaction history.

## 2. Signup / Registration

The primary signup mechanism is designed around the registration of "upload instances," which represent a user's registered session or workspace.

### 2.1. Registration Flow

*   **Function:** `register_upload()`
*   **Trigger:** A user or service initiates the registration process.
*   **Process:**
    1.  The system retrieves identifying information for the current session, including email, a descriptive name, and technical parameters (e.g., `model_name`, `is_cot`, `document_count`) which are compiled as tags.
    2.  This information is sent to a `registry_client` via the `register_upload` method.
    3.  The registry service processes the request and returns a new `instance_id` and an `instance_password`.
    4.  The system's `LoadService` is updated with these new credentials for the current session.
*   **Inputs:**
    *   `upload_name`: A user-defined name for the instance.
    *   `email`: The user's email address.
    *   `description`: A description of the instance/upload.
    *   `tags`: A collection of metadata (e.g., `TrainingTags`).
*   **Outputs:**
    *   On Success: A JSON object containing the new `instance_id` and `instance_password`.
    *   On Failure: A structured API error response (e.g., `code: 400, message: "Failed to register upload instance"`).

## 3. Sign-in / API Authentication

Sign-in is handled primarily at the API level, providing access for clients and services. The system supports two main authentication schemes as defined in the OpenAPI specification.

### 3.1. OAuth 2.0

*   **Type:** `oauth2`
*   **Description:** The primary authentication method, utilizing Google's OAuth 2.0 implementation.
*   **Flow:** `authorizationCode`
*   **Endpoints:**
    *   **Authorization URL:** `https://accounts.google.com/o/oauth2/auth`
    *   **Token URL:** `https://oauth2.googleapis.com/token`
*   **Scopes:** The system defines various access scopes (e.g., `read`, `write`, `profile`, `email`) with descriptions, which are presented to the user during the authorization flow.

### 3.2. API Key

*   **Type:** `apiKey`
*   **Description:** A secondary authentication method for direct, stateless API calls.
*   **Mechanism:**
    *   **In:** `query`
    *   **Name:** `key`
*   **Usage:** The API key is passed as a query parameter in the request URL. This method is supported alongside OAuth2.

### 3.3. Auxiliary Authentication (Internal Tools)

An auxiliary authentication mechanism exists for managing automated browser sessions across a combination of external sites (e.g., "reddit", "shopping").

*   **Function:** `main(auth_folder: str = "./.auth")`
*   **Mechanism:** This system stores and renews site-specific cookies in a local `.auth` directory. It is used for internal automation scripts (`auto_login.py`) and is not intended for end-user authentication to this service.

## 4. User Profiling

User profiles are built from data collected during the instance registration process.

*   **Profile Data:**
    *   `username` / `upload_name`: A unique, user-selected identifier.
    *   `email`: The user's registered email address.
    *   `description`: A user-provided description for their instance.
    *   `tags`: Technical and descriptive tags associated with the instance, including:
        *   `model_name`: The ML model in use.
        *   `is_cot`: A boolean flag indicating a specific configuration (e.g., Chain of Thought).
        *   `document_count`: The number of documents associated with the user's instance.
*   **Data Retrieval:** The currently active user's name can be retrieved via the `username()` endpoint, which accesses the `LoadService`.

## 5. Personalization

Personalization is achieved by tailoring system behavior and responses based on a user's distinct profile and historical interactions.

*   **Interaction History:** The system maintains a "memory" of user interactions. As suggested by functionality in other modules (`search_memory`), this memory is associated with a user or application-specific ID.
*   **Mechanism:** When a user interacts with the system, their `user_id` is used to retrieve their specific interaction history from the RAG (Retrieval Augmented Generation) system.
*   **Application:** This retrieved context (e.g., previous questions, generated content, discovered facts) is then used to augment prompts sent to the LLM.
*   **Outcome:** The LLM's generated responses are more relevant and personalized, as they are grounded in the user's prior activity. For example, if a user has previously uploaded and discussed specific documents, the system will be able to reference that information in future conversations.
