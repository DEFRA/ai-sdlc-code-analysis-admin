**Template**
- [x] Local logs
- [x] pre-commit and local venv
- [x] Running locally without docker watch

**Tests** 
- [ ] POST API - happy path (returns 201, call DB correctly, async agent OK) & invalid URL
- [ ] GET API - happy path and invalid id
- [ ] Unit test for the reducer unique_code_chunks_reducer
- [ ] File exclusions 

**Refactor**
- [x] Separate model for return in GET API, instead of underlying state model 
- [ ] Health - not checking full stack
- [ ] Change the code_chunker to use the LangChain Antrhopic LLM functions
- [ ] Automatically remove tree sitter if above a certain size
- [ ] use S3 for repository cloning temp file
- [ ] remove logger from the code_chunker class
- [ ] prevent server from starting without the anthropic key
- [ ] change the folder structure for agents - one per agent/graph
- [ ] STATUS
- [ ] Error handling - propagate back to graph and get graph to fail and exit
- [ ] Add an "llm client factory" that returns the model with settings like retry. Can also have a local mode that returns a local model

**Rules**
- [ ] Remove \_init\_ everywhere?

**API Error Handling**
https://docs.anthropic.com/en/api/errors 

Our API follows a predictable HTTP error code format:

- 400 - `invalid_request_error`: There was an issue with the format or content of your request. We may also use this error type for other 4XX status codes not listed below.
    
- 401 - `authentication_error`: There’s an issue with your API key.
    
- 403 - `permission_error`: Your API key does not have permission to use the specified resource.
    
- 404 - `not_found_error`: The requested resource was not found.
    
- 413 - `request_too_large`: Request exceeds the maximum allowed number of bytes.
    
- 429 - `rate_limit_error`: Your account has hit a rate limit.
    
- 500 - `api_error`: An unexpected error has occurred internal to Anthropic’s systems.
    
- 529 - `overloaded_error`: Anthropic’s API is temporarily overloaded.

Of the above - we can have specific handling for 429, 529 and 500

429 - Rate Limits. https://docs.anthropic.com/en/api/rate-limits.  If you exceed any of the rate limits you will get a [429 error](https://docs.anthropic.com/en/api/errors) describing which rate limit was exceeded, along with a `retry-after` header indicating how long to wait. 

So for 429 we can retry using 

500 & 529 - We can have an exponential backoff for these errors

**Token Use Management**
As well as rate limits, we need to ensure that we don't exceed the amount of tokens that is allowed by per call. This can achieved using anthropic following this https://docs.anthropic.com/en/docs/build-with-claude/token-counting, as well as using libraries like tiktoken 

**Rate Limits** **& API Keys**
The agent is making a lot of API calls. An option at our disposal is to use multiple api keys for the different calls.  We could even dynamically rotate them potentially









