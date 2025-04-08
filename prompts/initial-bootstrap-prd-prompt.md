```
ANALYSIS PHASE:

Read each of the following documents, analyse them.

CONTEXT:
This is a new AI agent microservice. The microservice is called 'ai-sdlc-code-analysis-api'. It will be used to analyse code bases and produce detailed reports on how they work. The interface is REST API's, which will be used to trigger the agent execution, and also used to get information about prior executions and about code reviews. 

An empty microservice repo exists. The purpose of this inital phase is to bootstrap the initial service API's and agents, and ensure everyting is in place so that in subsequent phases we can add functionality.   

TECH STACK:
This new microservice will be implemented using python and Fast API. The agents will be implemented using LangGraph. The agents will be triggered by calling REST API's. 

MongoDB will be used for application database and the LangGraph persistent state store using checkpointing. The MongoDb will also be called ai-sdlc-code-analysis-api. To implement LangGraph checkpoints we will use the libary langgraph-checkpoint-mongodb. 

DATA MODEL:
The LangGraph state model will be this (in python):

class CodeAnalysisState(BaseModel):
    """State for the parent code analysis graph."""

    repo_url: str = Field(..., description="The URL of the repository to analyze")

CodeAnalysisState will be persisited to the mongodb store using langgraph-checkpoint-mongodb.

LANGGRAPH AGENT:
code_analysis agent. This will be main 'parent' graph. This will recieve a thread_id and a repo_url. Initally this will simply update the repo_url state and persist that to the mongodb with checkpointing.  Further functionality will be added in later phases. 

API ENDPOINTS:

POST /api/v1/code-analysis - this will implement trigger a new exection of code_analysis agent.  

The payload will contain a mandatory repo_url url parameter, which will be passed to the agent. 

The API will create a new CodeAnalysisState record in the mongo db and use the mongo id as a new thread_id for the agent, and also pass that to the agent.  The agent will subsequently update this record as it progresses asyncronously.

The API will return a syncronous reponse containing the thread_id, and the agent will then be processed asyncronously 

GET /api/v1/code-analysis/{threadId} - this will get the state information for the given thread_id. it will contain everything in state at that moment in time, as per the CodeAnalysisState model.

FRONTEND INTERFACE REQUIREMENTS:
NONE

Confirm you have read the content of the documents above, then continue...

IMPLEMENTATION PHASE:

Create a detailed product requirements document that breaks down the functionality by feature.  The end result should be a list of features, with backend user stories detailed for the given feature. The stories should be discrete and detailed.  There may be multiple stories per feature.  The end result should be a hybrid of very good user stories, with the details found in a PRD.  Please number the features and the stories so they can be easily referred to later.

Each story format should be in the following format:
- Story title
- Story written in As a, I want, so that story format
- Testable acceptance criteria in Given, When, Then BDD format
- Detailed Architecture Design Notes
- Include any other detail or relevant notes that would help an AI-powered coding tool understand and correctly implement the features.
- Include any information about stories that are dependencies, such as backend stories that are needed to complete a frontend story, for example.
- Include any information about related stories for context.

You should also give any overarching context in the feature description.

At the top of the document include the detail of the data model for reference.

Do NOT include any summary, timelines, or non-functional requirements, unless they are relevant to the specific feature implementations.
Do NOT add any functionality that isn't in the above requirements, only add the functionality already defined.

Include a short 'Context' part at the top of the document that details the purpose and background information that is relevant to the project overall.

VERIFICATION AND COMPLETION PHASE:
Validate your work to check that all the features defined in the documents above meet all the necessary requirements and there is no ambiguity or overlap before writing the final output.
```
