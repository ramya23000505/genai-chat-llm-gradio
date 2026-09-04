## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework
#### Date: 04-09-2026
#### Name: Ramya R
#### Reg No: 212223230169
### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
Standard static text interfaces fail to maintain conversational context across multiple turns of interaction. The goal is to build an interactive chatbot UI using gr.Blocks that formats dialogue history into a structured prompt, queries an LLM endpoint, and renders multi-turn responses dynamically.  

### DESIGN STEPS:

#### STEP 1:
Initialize environment variables to load the Hugging Face API key and target Falcon-LLM endpoint URL. Create a text_generation.Client instance with Bearer authentication.

#### STEP 2:
Define a prompt-formatting function (format_chat_prompt) to concatenate past turns from chat_history with the latest user input, and construct a respond callback function that queries the model and appends generated text back into the chat session.

#### STEP 3:
Build the UI using gr.Blocks(), laying out gr.Chatbot, gr.Textbox, gr.Button, and gr.ClearButton. Connect submit events (button click and Enter key press) to the respond function and launch the app.

### PROGRAM:

### OUTPUT:

### RESULT:
The "Chat with LLM" application using Gradio Blocks framework was successfully built.
