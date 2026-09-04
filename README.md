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
```py
import os
import io
import IPython.display
from PIL import Image
import base64 
import requests 
requests.adapters.DEFAULT_TIMEOUT = 60

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

# Helper function
import requests, json
from text_generation import Client

#FalcomLM-instruct endpoint on the text_generation library
client = Client(os.environ['HF_API_FALCOM_BASE'], headers={"Authorization": f"Basic {hf_api_key}"}, timeout=120)

prompt = "Was the internet invented or discovered?"
client.generate(prompt, max_new_tokens=256).generated_text

def format_chat_prompt(message, chat_history):
    prompt = ""
    for turn in chat_history:
        user_message, bot_message = turn
        prompt = f"{prompt}\nUser: {user_message}\nAssistant: {bot_message}"
    prompt = f"{prompt}\nUser: {message}\nAssistant:"
    return prompt

def respond(message, chat_history):
        formatted_prompt = format_chat_prompt(message, chat_history)
        bot_message = client.generate(formatted_prompt,
                                     max_new_tokens=1024,
                                     stop_sequences=["\nUser:", "<|endoftext|>"]).generated_text
        chat_history.append((message, bot_message))
        return "", chat_history

with gr.Blocks() as demo:
    chatbot = gr.Chatbot(height=240) #just to fit the notebook
    msg = gr.Textbox(label="Prompt")
    btn = gr.Button("Submit")
    clear = gr.ClearButton(components=[msg, chatbot], value="Clear console")

    btn.click(respond, inputs=[msg, chatbot], outputs=[msg, chatbot])
    msg.submit(respond, inputs=[msg, chatbot], outputs=[msg, chatbot]) #Press enter to submit

gr.close_all()
demo.launch(share=True, server_port=int(os.environ['PORT3']))

```

### OUTPUT:
<img width="1067" height="76" alt="image" src="https://github.com/user-attachments/assets/05a456b2-7098-41c3-a3fb-1deb92b6ce19" />

<img width="926" height="503" alt="image" src="https://github.com/user-attachments/assets/a9d4307b-411a-493b-a0d5-d19f284772e3" />

### RESULT:
The "Chat with LLM" application using Gradio Blocks framework was successfully built.
