# GenerativeAI-Project4-PersonalizedRealEstateAgent
Develop an innovative application named "HomeMatch". This application leverages large language models (LLMs) and vector databases to transform standard real estate listings into personalized narratives that resonate with potential buyers' unique preferences and needs.

# Real Estate Chatbot Documentation

## Overview
This code implements a conversational AI real estate assistant called "HomeMatch" that helps users find properties based on their preferences. The system uses LangChain, OpenAI's GPT models, and Gradio to create an interactive chat interface.

## Key Components

### 1. Data Models
- `Listing`: Base model for property listings stored in a database
- `Property`: Detailed property information model including:
  - Basic details (location, price, size, bedrooms, bathrooms)
  - Descriptions (property and suburb descriptions)
  - Marketing elements (highlights, tag line, call to action)
- `BuyerPreferences`: Model for storing user preferences including:
  - Location (limited to specific towns)
  - Budget and size requirements
  - Room requirements (bedrooms/bathrooms)
  - Desired features and lifestyle preferences
- `AgentState`: Tracks conversation state including:
  - Message history
  - Extracted preferences
  - Matched properties
  - Formatted descriptions

### 2. Core Tools

#### Extract Preferences Tool
```python
@tool
def extract_preferences(state: AgentState) -> Optional[BuyerPreferences]
```
- Analyzes conversation history to extract structured buyer preferences
- Uses GPT model with a template to parse user requirements
- Returns formatted BuyerPreferences object

#### Match Properties Tool
```python
@tool
def match_properties(buyer_preferences: BuyerPreferences) -> str
```
- Takes structured preferences and finds matching properties
- Uses embeddings for semantic search of property database
- Generates detailed, formatted property descriptions
- Returns formatted string with property matches

### 3. Conversation Flow

The system uses a StateGraph workflow with three main nodes:
1. `agent`: Processes user input and determines next actions
2. `tools`: Executes tools (preference extraction or property matching)
3. `respond`: Generates final responses to the user

Flow control:
- Entry point is the "agent" node
- Uses `should_continue()` to determine whether to:
  - Continue to tools
  - Generate a response
- Tools feed back to agent for further processing
- Responses end the current conversation cycle

### 4. Templates and Prompts

The system uses several prompt templates:
- Main system template: Defines the assistant's personality and behavior
- Extract preferences template: Guidelines for parsing user preferences
- Property description template: Format for property listings

### 5. User Interface

Uses Gradio's ChatInterface for:
- Real-time chat interaction
- Message history management
- Streaming responses

## Key Features

1. Natural Language Understanding
   - Extracts structured preferences from casual conversation
   - Maintains context across multiple messages
   - Handles various ways users might express preferences

2. Property Matching
   - Semantic search using embeddings
   - Considers multiple criteria simultaneously
   - Ranks and returns best matches

3. Response Generation
   - Generates engaging, emoji-enhanced property descriptions
   - Includes marketing elements (tag lines, calls to action)
   - Maintains professional real estate terminology

4. Error Handling
   - Graceful handling of missing information
   - Prompts for additional details when needed
   - Validates user inputs against allowed values

## Technical Details

### Dependencies
- LangChain (core, OpenAI integration)
- Gradio (UI framework)
- OpenAI GPT-4 (language model)
- Pydantic (data validation)
- LanceDB (vector database)

### Configuration
- Uses GPT-4-mini model
- Temperature set to 0.7 for balanced creativity
- Streaming enabled for real-time responses

### Limitations
- Limited to specific towns in the database
- Requires OpenAI API access
- Synchronous processing of requests

## Usage Notes

1. The system expects users to provide preferences around:
   - Location (specific towns only)
   - Budget
   - Property size
   - Number of bedrooms/bathrooms
   - Desired features
   - Lifestyle preferences

2. The chatbot will:
   - Ask clarifying questions if information is missing
   - Extract preferences once sufficient information is gathered
   - Match properties based on preferences
   - Present formatted property descriptions
   - Provide contact information for interested buyers

3. For contact inquiries, the system directs users to:
   - Email: (Dummy address)
   - Phone: 0412345678
