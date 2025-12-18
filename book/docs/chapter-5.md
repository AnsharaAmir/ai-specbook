---
sidebar_position: 5
---

# Chapter 5: User Authentication and Personalization

Our RAG chatbot is architecturally sound, but it's impersonal. Every user gets the same experience. To create a truly engaging application, we need to know who our users are. This chapter will cover how to add user authentication to our FastAPI backend and how to use that identity to create a personalized user experience.

## Effortless Authentication with Better-Auth

Authentication can be complex to set up. To keep things simple and secure, we'll use a fictional library called **Better-Auth**. Imagine `Better-Auth` as a modern authentication handler for FastAPI that makes it trivial to add sign-up and sign-in flows, including support for social logins (like Google) and passwordless email links.

The core idea behind `Better-Auth` is to abstract away the complexities of OAuth2 and token management. You configure it once, and it provides you with protected routes and easy access to the current user's identity.

### Integrating Better-Auth

Let's see how we would integrate `Better-Auth` into our FastAPI application.

First, we would initialize it with a secret key and configure a provider, like Google.

```python
# main.py (Updated)
from fastapi import FastAPI, Depends
from pydantic import BaseModel
from my_rag_logic import get_rag_answer
from better_auth import BetterAuth, User

# 1. Initialize BetterAuth
auth_handler = BetterAuth(
    secret_key="YOUR_SUPER_SECRET_KEY",
    google_oauth_client_id="YOUR_GOOGLE_CLIENT_ID",
    google_oauth_client_secret="YOUR_GOOGLE_CLIENT_SECRET",
)

app = FastAPI()

# 2. Add auth routes to the app
app.include_router(auth_handler.router)

class Query(BaseModel):
    question: str
    conversation_id: str | None = None

# 3. Protect the chat endpoint
@app.post("/chat")
async def chat(query: Query, user: User = Depends(auth_handler.get_current_user)):
    """
    Receives a question, retrieves context, and returns a personalized answer.
    The user object is now available thanks to BetterAuth.
    """
    answer, new_conversation_id = await get_rag_answer(
        question=query.question,
        conversation_id=query.conversation_id,
        user_profile=user.profile, # Pass the user's profile to the logic
    )
    return {"answer": answer, "conversation_id": new_conversation_id}

```

With just a few lines of code, `Better-Auth` does the following for us:
- Creates API endpoints like `/login/google` and `/auth/callback`.
- Handles the OAuth2 flow, exchanging a code for a user token.
- Provides a `Depends(auth_handler.get_current_user)` dependency that ensures only authenticated users can access the `/chat` endpoint.
- Gives us a `user` object containing their ID, email, name, and a profile we can customize.

## User Profiling and Content Personalization

Now that we can identify users, we can start building a profile for them to personalize their experience. A user profile is a collection of information we store about a user that helps us tailor the application to their needs.

This profile can be stored in our Neon Postgres database and could include:
- **Explicit Preferences:** Settings the user configures, like preferred language or chatbot personality (e.g., "formal," "witty").
- **Implicit History:** Past conversations, topics they frequently ask about, or documents they have accessed.

### Enhancing the RAG Prompt

The most powerful way to use this profile is by incorporating it into our RAG prompt. By giving the `GeminiModel` context about the user, we can get a much more personalized response.

Let's update our RAG logic to use the `user_profile`.

```python
# my_rag_logic.py (Updated)

async def get_rag_answer(question: str, conversation_id: str | None, user_profile: dict):
    # ... (retrieve context chunks from Qdrant as before) ...
    context = "\n".join(chunk.text for chunk in retrieved_chunks)

    # Get user's name from their profile, with a default
    user_name = user_profile.get("name", "there")
    
    # Get preferred tone, with a default
    bot_personality = user_profile.get("preferred_tone", "a helpful assistant")

    prompt = f"""
You are {bot_personality}. The user's name is {user_name}.
A user has asked a question. Using ONLY the following text, please answer the user's question.
Address the user by their name in your response.

---
TEXT:
{context}
---

QUESTION:
{question}
"""
    
    # ... (call GeminiModel and store conversation history as before) ...
    
    answer = gemini_model.call(prompt=prompt)
    
    # You could also update the user profile here, e.g., by saving the topic.
    # db.update_user_profile(user_profile.id, {"last_topic": "some_topic"})

    return answer, new_conversation_id
```

By making this small change, our chatbot is transformed. Instead of a generic "You," it can now say, "Hi Alex, I found this information for you..." This simple act of personalization dramatically improves the user experience, making the application feel more like a personal assistant.

## What's Next?

We've added a crucial layer to our application, enabling us to build a relationship with our users. With authentication and personalization in place, we're ready to start thinking about the user interface.

In the next chapter, we'll shift our focus from the backend to the frontend. We'll explore how to build a responsive and intuitive web interface for our chatbot using a modern JavaScript framework, allowing users to interact with the powerful system we've just built.

```