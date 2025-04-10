```
Please refactor []. Currently this is a simple LangGraph node that updates the state with arbitary data. Now we want it to update the same state, but with real data. 

It should call out to Anthropic API and ask it to do a code analysis of the given code_chunk, recieved in state: CodeChunkAnalysisState - state.code_chunk. 

Use the SYSTEM_PROMPT and USER_PROMPT below to prompt Anthropic

Follow the Cursor Rule Model Integration in LangGraph for Anthropic integration and structured output handling

Use the values returned to update the analyzed_code_chunk state 

SYSTEM PROMPT:
You are a specialized code analysis system that produces ONLY valid JSON output following the CodeAnalysisChunk schema. Your entire response must be parseable JSON with no surrounding text, markdown, explanations, or formatting. Never include anything outside the JSON structure. Always include all fields from the schema, using null for fields where no applicable content exists in the code chunk. Maintain this strict JSON-only format under all circumstances.

USER PROMPT:
Analyze the following code chunk, in the following json format. 

{
  "chunk_id": "{{chunk_id}}",
  "description": "{{description}}",
  "files": {{files}},
  "content": "{{content}}"
}

Your analysis must return ONLY a valid JSON object with these fields:

1. summary (required): Concise functional description of what this code does from a business perspective (3-5 sentences).

2. data_model (string): If applicable, include:
   - Mermaid ERD diagram as a string (wrapped in triple backticks with "mermaid" tag)
   - Detailed breakdown of each model's fields, types, and relationships
   - Set to null if no data models are present

1. interfaces (string): If applicable, document:
   - Method signatures with parameters and return types
   - API endpoints with request/response formats
   - Interface contracts and requirements
   - Set to null if no interfaces are defined

1. business_logic (string): If applicable, analyze:
   - Algorithms and processing workflows
   - Business rules, validations, and conditional logic
   - Set to null if no significant business logic exists

1. dependencies (string): Always analyze:
   - Internal dependencies (imports from other project files)
   - External dependencies (third-party libraries)
   - API calls or external services
   - Set to null only if truly no dependencies exist

1. configuration (string): If applicable, document:
   - Configuration variables with defaults and valid options
   - Environment variables and config files
   - Loading mechanisms
   - Set to null if no configuration exists

1. infrastructure (string): If applicable, analyze:
   - Deployment requirements and environment needs
   - Resource requirements and scaling considerations
   - Set to null if no infrastructure elements exist

1. non_functional (string): If applicable, document:
   - Performance, security, reliability aspects
   - Error handling, logging, monitoring
   - Maintainability and compliance considerations
   - Set to null if no significant non-functional elements exist

Your response must be a valid JSON object following EXACTLY this structure:

{
  "summary": "string",
  "data_model": "string",
  "interfaces": "string",
  "business_logic": "string",
  "dependencies": "string",
  "configuration": "string",
  "infrastructure": "string",
  "non_functional": "string"
}

All string fields should contain detailed markdown-formatted text. For fields with no applicable content, use null instead of an empty string. Do NOT include any content outside this JSON structure.

```
