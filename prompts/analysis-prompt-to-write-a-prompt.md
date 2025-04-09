# CONTEXT

I'm writing an application that takes in a group of files in a code base and runs some analysis. The group of files is called a `chunk`. The information returned will later be used to fully analyze the whole of the code base.

# ANALYSIS PHASE

## Review the following input code chunk model

class CodeChunk(BaseModel):
    """A chunk of code from the repository."""
    chunk_id: str = Field(..., description="The unique identifier")
    description: str = Field(..., description="A description of the chunk")
    files: list[str] = Field(..., description="The files within the chunk")
    content: str = Field(..., description="The code and content within the chunk")

## Review the following output chunk model

class CodeAnalysisChunk(BaseModel):
    """The data structure for the analyzed code chunks"""
    summary: str
    data_model: Optional[str] = None
    interfaces: Optional[str] = None
    business_logic: Optional[str] = None
    dependencies: Optional[str] = None
    configuration: Optional[str] = None
    infrastructure: Optional[str] = None
    non_functional: Optional[str] = None

# IMPLIMENTATION PHASE

I want you to write a prompt that takes an input code chunk, then performs detailed analysis to return the resulting CodeAnalysisChunk JSON object. The fields are defined as follows:

- summary - the summary of what the code chunk is from a functional perspective
- data_model - the data model used in the files of the chunk, including a mermaid ERD diagram and a breakdown of the model and fields
- interfaces - the definition of the set of method signatures. Classes that implement this interface commit to providing concrete implementations for these methods.
- business_logic - any business logic that is used in the code chunk, broken down in a standard format
- dependencies - a mapping of dependencies, both within the code chunk, but also external to the code chunk. Include any 3rd party libraries or api calls.
- configuration - any configuration used in the code chunk, including the defaults and options for each configuration variable
- infrastructure - analysis of any infrastructure used in the code chunk, including any details required for a successful deployment of the code.
- non_functional - analysis of non-functional requirements found in the code chunk. This analysis will include any and all non-functional categories.

The created prompt should ONLY return the above output in a JSON format, with no other text, so include the system AND user prompts.

# VERIFICATION PHASE

The results of this work should be as follows:

- A detailed user prompt that requests analysis of the above information for a given CodeChunk input.
- The result of the user prompt should be in a JSON format only
- The result of the user prompt should be in the JSON format of the CodeAnalysisChunk model
- A short system prompt to send to the LLM reinforcing the need for returning only JSON as defined.