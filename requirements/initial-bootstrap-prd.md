# Code Analysis PRD
## Context

The **ai-sdlc-code-analysis-api** microservice is designed to bootstrap an AI agent-based code analysis system. In this phase, the focus is on establishing the service's REST API endpoints and integrating the LangGraph agent infrastructure with MongoDB for persistent state management. The microservice will later be extended with additional functionality to analyze code bases and generate detailed reports. The current functionality includes triggering the analysis agent asynchronously and retrieving the state of that analysis via a RESTful interface.

---

## Data Model Reference

```python

class CodeAnalysisState(BaseModel):

"""State for the parent code analysis graph."""

repo_url: str = Field(..., description="The URL of the repository to analyze")

```

*This data model will be persisted in MongoDB using the `langgraph-checkpoint-mongodb` library, which provides checkpointing for the LangGraph state.*

---

## Feature Breakdown and User Stories

### Feature 1: Data Model & Database Integration

**Overview:**

Set up the MongoDB integration and establish persistence for the `CodeAnalysisState` model using the `langgraph-checkpoint-mongodb` library.


- **Story 1.1: Establish MongoDB Connection for CodeAnalysisState**

- **Story Title:** Establish MongoDB connection and persistence

- **User Story:**

As a backend developer, I want to configure MongoDB integration for persisting the CodeAnalysisState so that the state of code analysis executions is reliably stored and retrievable.

- **Acceptance Criteria:**

- **Given** an empty repository setup

**When** the microservice starts

**Then** the application should establish a connection to the MongoDB instance (database named `ai-sdlc-code-analysis-api`)

- **Given** a new CodeAnalysisState record is created

**When** the record is persisted

**Then** it should be stored using the `langgraph-checkpoint-mongodb` library for checkpointing.

- **Architecture Design Notes:**

- Use environment variables/configuration files to supply the MongoDB connection details.

- Implement connection pooling and error handling for database connectivity.

- **Dependencies:** This story must be completed before the REST API endpoints that create or read state records.


---

### Feature 2: POST /api/v1/code-analysis Endpoint

**Overview:**

Implement an endpoint to trigger a new execution of the code analysis agent by accepting a repository URL, creating a new state record, and initiating the agent asynchronously.

- **Story 2.1: Create Code Analysis Trigger Endpoint**

- **Story Title:** Implement POST /api/v1/code-analysis endpoint for triggering analysis

- **User Story:**

As a user, I want to trigger a new code analysis by providing a repository URL so that the analysis agent can start processing the specified code base.

- **Acceptance Criteria:**

- **Given** a valid HTTP POST request to `/api/v1/code-analysis` with a `repo_url` in the payload

**When** the request is received

**Then** the system should validate that the `repo_url` is present and correctly formatted.

- **Given** a valid `repo_url`

**When** processing the request

**Then** a new CodeAnalysisState record is created in MongoDB with the provided `repo_url` and assigned a unique thread ID.

- **Given** the creation of the state record

**When** the record is persisted

**Then** the system should asynchronously trigger the LangGraph `code_analysis` agent, passing the thread ID and `repo_url`.

- **Given** the initiation of the agent

**When** the API returns a response

**Then** the response should include the thread ID in a synchronous manner.

- **Architecture Design Notes:**

- Implement input validation for the repository URL.

- Use FastAPI to create the endpoint and handle asynchronous processing.

- Integrate with MongoDB to store the new state record and retrieve the generated ID for use as a thread ID.

- Trigger the LangGraph agent asynchronously, ensuring that the response is sent immediately with the thread ID.

- **Dependencies:** This story depends on the successful implementation of Feature 1 for database integration.

- **Story 2.2: Validate Repository URL Input**

- **Story Title:** Validate and sanitize input for repo_url in POST endpoint

- **User Story:**

As a user, I want the system to ensure that the provided repository URL is valid so that the analysis agent processes only valid repositories.

- **Acceptance Criteria:**

- **Given** a HTTP POST request with an invalid or missing `repo_url`

**When** the endpoint processes the request

**Then** the system should return an error response indicating that the repository URL is required and must be valid.

- **Architecture Design Notes:**

- Use FastAPI request validation features (e.g., Pydantic models) to enforce input constraints.

- Return standardized error messages for invalid input.

  

---

  

### Feature 3: GET /api/v1/code-analysis/{threadId} Endpoint

**Overview:**

Implement an endpoint to retrieve the state information for a given thread ID, which reflects the progress or results of the analysis.


- **Story 3.1: Retrieve Code Analysis State**

- **Story Title:** Implement GET /api/v1/code-analysis/{threadId} endpoint for retrieving state

- **User Story:**

As a user, I want to query the status of a code analysis execution using its thread ID so that I can monitor the progress or retrieve the current state of the analysis.

- **Acceptance Criteria:**

- **Given** a valid HTTP GET request to `/api/v1/code-analysis/{threadId}`

**When** the request is received

**Then** the system should validate the provided thread ID.

- **Given** a valid thread ID that exists in the database

**When** querying the state

**Then** the system should return a JSON response containing all state information as defined by the CodeAnalysisState model.

- **Given** a thread ID that does not exist

**When** querying the state

**Then** the system should return a 404 error with a meaningful error message.

- **Architecture Design Notes:**

- Use path parameters in FastAPI to capture the thread ID.

- Implement database lookup logic with error handling for non-existent IDs.

- Ensure the returned state is serialized according to the CodeAnalysisState schema.

- **Dependencies:** This story depends on the MongoDB integration from Feature 1.

  

---

### Feature 4: LangGraph Agent Integration and Checkpointing

  
**Overview:**

Integrate the LangGraph-based `code_analysis` agent, ensuring that it can be triggered with the provided thread ID and repository URL and persist its state using MongoDB checkpointing.


- **Story 4.1: LangGraph Agent Basic Execution**

- **Story Title:** Implement basic LangGraph code_analysis agent execution

- **User Story:**

As a backend system, I want the LangGraph `code_analysis` agent to receive the thread ID and repository URL so that it can initialize the analysis state and persist it via MongoDB checkpointing.

- **Acceptance Criteria:**

- **Given** the agent is triggered by the POST endpoint

**When** the agent execution starts

**Then** the agent should receive the thread ID and repo_url as parameters.

- **Given** the agent receives the parameters

**When** executing its initial step

**Then** it should update the state (e.g., confirm the `repo_url`) and persist it using the `langgraph-checkpoint-mongodb` library.

- **Architecture Design Notes:**

- Ensure that the LangGraph agent is callable as an asynchronous task.

- The agent should be modular, with a clear separation between initialization (state update) and future analysis functionality.

- Integrate the agent's checkpointing logic with the MongoDB persistence layer.

- **Dependencies:** This story depends on the MongoDB setup from Feature 1 and the POST endpoint implementation in Feature 2.

- **Story 4.2: Verify Checkpointing Mechanism**

- **Story Title:** Verify persistence of state via langgraph-checkpoint-mongodb

- **User Story:**

As a developer, I want to ensure that the agent's state is correctly checkpointed in MongoDB so that the state is recoverable and consistent throughout the agent's execution.

- **Acceptance Criteria:**

- **Given** the LangGraph agent is executing

**When** it performs a state update

**Then** the state must be persisted to MongoDB using the `langgraph-checkpoint-mongodb` library.

- **Given** the state is persisted

**When** a GET request is made for the corresponding thread ID

**Then** the response should accurately reflect the persisted state.

- **Architecture Design Notes:**

- Write tests to simulate agent execution and verify the checkpointed state in MongoDB.

- Document the integration points between the agent and MongoDB checkpointing.

- **Dependencies:** Completes after Story 4.1 is implemented.

  

---

## Verification & Completion

Each feature and user story above has been defined to ensure the initial phase requirements are met with no overlapping or ambiguity:

- **Database integration (Feature 1)** is the foundation and is referenced by all subsequent stories.

- **API endpoints (Features 2 and 3)** correctly manage the creation and retrieval of state.

- **LangGraph agent integration (Feature 4)** bridges asynchronous processing with the persistent state.

This document should guide the development of the microservice with clear and discrete user stories, ensuring every requirement from the initial specification is implemented.