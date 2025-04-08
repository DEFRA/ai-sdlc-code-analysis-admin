**Template**
- [x] Local logs
- [x] pre-commit and local venv
- [x] Running locally without docker watch

**Tests** 
- [ ] POST API - happy path (returns 201, call DB correctly, async agent OK) & invalid URL
- [ ] GET API - happy path and invalid id

**Refactor**
- [ ] Separate model for return in GET API, instead of underlying state model 
- [ ] Health - not checking full stack
- [ ] Change the code_chunker to use the LangChain Antrhopic LLM functions
- [ ] Automatically remove tree sitter if above a certain size
- [ ] use S3 for repository cloning temp file
- [ ] remove logger from the code_chunker class
- [ ] prevent server from starting without the anthropic key

**Rules**
- [ ] Remove \_init\_ everywhere