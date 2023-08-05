# GPT Engineer

[![Discord Follow](https://dcbadge.vercel.app/api/server/8tcDQ89Ej2?style=flat)](https://discord.gg/8tcDQ89Ej2)
[![GitHub Repo stars](https://img.shields.io/github/stars/AntonOsika/gpt-engineer?style=social)](https://github.com/AntonOsika/gpt-engineer)
[![Twitter Follow](https://img.shields.io/twitter/follow/antonosika?style=social)](https://twitter.com/AntonOsika)


**Specify what you want it to build, the AI asks for clarification, and then builds it.**

GPT Engineer is made to be easy to adapt, extend, and make your agent learn how you want your code to look. It generates an entire codebase based on a prompt.

[Demo](https://twitter.com/antonosika/status/1667641038104674306)

## Project philosophy

- Simple to get value
- Flexible and easy to add new own "AI steps". See `steps.py`.
- Incrementally build towards a user experience of:
  1. high level prompting
  2. giving feedback to the AI that it will remember over time
- Fast handovers back and forth between AI and human
- Simplicity, all computation is "resumable" and persisted to the filesystem

## Usage

Choose either **stable** or **development**.

For **stable** release:

- `pip install gpt-engineer`

For **development**:
- `git clone https://github.com/AntonOsika/gpt-engineer.git`
- `cd gpt-engineer`
- `pip install -e .`
  - (or: `make install && source venv/bin/activate` for a venv)

**Setup**

With an OpenAI API key (preferably with GPT-4 access) run:

- `export OPENAI_API_KEY=[your api key]`

To set API key on windows check the [Windows README](./WINDOWS_README.md).

**Run**:

- Create an empty folder. If inside the repo, you can run:
  - `cp -r projects/example/ projects/my-new-project`
- Fill in the `prompt` file in your new folder
- `gpt-engineer projects/my-new-project`
  - (Note, `gpt-engineer --help` lets you see all available options. For example `--steps use_feedback` lets you improve/fix code in a project)

By running gpt-engineer you agree to our [terms](https://github.com/AntonOsika/gpt-engineer/blob/main/TERMS_OF_USE.md).

**Results**
- Check the generated files in `projects/my-new-project/workspace`


To **run in the browser** you can simply:

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://github.com/AntonOsika/gpt-engineer/codespaces)



## Features

You can specify the "identity" of the AI agent by editing the files in the `preprompts` folder.

Editing the `preprompts`, and evolving how you write the project prompt, is how you make the agent remember things between projects.

Each step in `steps.py` will have its communication history with GPT4 stored in the logs folder, and can be rerun with `scripts/rerun_edited_message_logs.py`.

## Vision
The gpt-engineer community is building the **open platform for devs to tinker with and build their personal code-generation toolbox**.

If you are interested in contributing to this, we would be interested in having you.

If you want to see our broader ambitions, check out the [roadmap](https://github.com/AntonOsika/gpt-engineer/blob/main/ROADMAP.md), and join
[discord](https://discord.gg/8tcDQ89Ej2)
to get input on how you can [contribute](.github/CONTRIBUTING.md) to it.

We are currently looking for more maintainers and community organisers. Email anton.osika@gmail.com if you are interested in an official role.


This project is an implementation of a chatbot using OpenAI's GPT-4 model. It's designed to handle chat conversations, serialize and deserialize messages, and keep a log of token usage during the chat. It also provides fallback to GPT-3.5-turbo if GPT-4 is not available.

The project consists of three main files: ai.py, chat_to_files.py, and db.py.

ai.py

This file contains the main class AI that handles the chat process, token counting, and serialization/deserialization of messages.

AI Class

Properties

temperature: Controls the randomness of the AI's responses.
model_name: The name of the AI model to use.
llm: The chat model object.
tokenizer: The tokenizer for the chat model.
cumulative_prompt_tokens: The total number of tokens used in prompts.
cumulative_completion_tokens: The total number of tokens used in completions.
cumulative_total_tokens: The total number of tokens used in prompts and completions.
token_usage_log: A list of TokenUsage objects detailing the token usage for each step in the conversation.
Methods

start: Starts a chat conversation.
fsystem, fuser, fassistant: Helper methods to create SystemMessage, HumanMessage, and AIMessage objects respectively.
next: Generates the next message in the conversation.
serialize_messages, deserialize_messages: Methods to serialize and deserialize messages.
update_token_usage_log: Updates the token usage log after each conversation step.
format_token_usage_log: Formats the token usage log into a string.
num_tokens, num_tokens_from_messages: Methods to count the number of tokens in a text or a list of messages.
Helper Functions

fallback_model: Returns a fallback model if the desired model is not available.
create_chat_model: Creates a chat model object.
get_tokenizer: Returns the appropriate tokenizer for a given model.
serialize_messages: A helper function to serialize messages.
chat_to_files.py

This file contains functions to parse chat data and store it into different files.

parse_chat: Parses chat data and extracts code blocks along with their preceding filenames.
to_files: Writes the parsed chat data into separate files.
db.py

This file contains a simple key-value store class, DB, where keys are filenames and values are file contents. It provides methods to check if a key exists in the database, get the value of a key, and set the value of a key.

How to Use

Create an AI instance with the desired model name and temperature. Use the start method to start a conversation and the next method to continue the conversation. The serialize_messages and deserialize_messages methods can be used to serialize and deserialize the conversation messages. The format_token_usage_log method can be used to get a string representation of the token usage log.

The to_files function in chat_to_files.py can be used to store chat data into separate files.

The DB class in db.py can be used to create a simple file-based database. Use the get method to get the value of a key and the set method to set the value of a key.

Please note that this code uses OpenAI's GPT-4 model, which may not be available to all users at the time of writing. A fallback to GPT-3.5-turbo is provided in case GPT-4 is not available.


Project Structure

The ai.py file is part of a larger project named langchain. The high-level structure of the project is as follows:

langchain/
callbacks/
streaming_stdout.py (provides a callback handler for streaming results to stdout)
chat_models/
base.py (defines the base chat model)
ChatOpenAI.py (implements a chat model using the OpenAI API)
schema/
AIMessage.py (defines the data schema for AI messages)
ai.py (main application file)
Overview

ai.py is the main entry point to the application. It imports necessary modules and defines the main data class and functions for the AI application.

Data Class

@dataclass AIMessage: This data class represents a message from the AI. It contains the message content and other related properties.
Functions

def __init__(self, api_key: str, model: str) -> None: The constructor for BaseChatModel class. It sets up the OpenAI API with the provided API key and model.

def get_chat_model(self, name: str) -> BaseChatModel: This function returns a chat model instance given its name.

def get_chat_models(self) -> List[BaseChatModel]: This function returns a list of all available chat models.

def get_chat_model_names(self) -> List[str]: This function returns a list of the names of all available chat models.

def send_message(self, message: Union[str, AIMessage]) -> AIMessage: This function sends a message to the AI and returns its response.

def send_messages(self, messages: List[Union[str, AIMessage]]) -> List[AIMessage]: This function sends a list of messages to the AI and returns a list of its responses.

def from_json(cls, data: str) -> AIMessage: This class method creates an AIMessage instance from a JSON string.

def to_json(self) -> str: This method converts an AIMessage instance to a JSON string.

Flow of the Application

The application starts by initializing the BaseChatModel with the OpenAI API key and model. Then, it can use the send_message or send_messages methods to communicate with the AI. The messages can be either simple strings or AIMessage instances. The AI's responses are returned as AIMessage instances. These can be easily converted to and from JSON for storage or transmission.

The get_chat_model, get_chat_models, and get_chat_model_names methods provide ways to interact with different chat models. This can be useful if the application needs to switch between different AI models.

The tiktoken library is used in the application, likely for counting tokens in a text string without making an API call. This can be useful for managing usage of the OpenAI API, as it charges per token.
