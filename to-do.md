**Template**
- [ ] Local logs
- [ ] pre-commit and local venv
- [ ] Running locally without docker watch

**Tests** 
- [ ] POST API - happy path (returns 201, call DB correctly, async agent OK) & invalid URL
- [ ] GET API - happy path and invalid id

**Refactor**
- [ ] Separate model for return in GET API, instead of underlying state model 
- [ ] Health - not checking full stack
- [ ] Change the code_chunker to use the LangChain Antrhopic LLM functions
- [ ] Automatically remove tree sitter if above a certain size

**Rules**
- [ ] Remove __init__ everywhere