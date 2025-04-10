```
ANALYSIS PHASE:

We have added two new feilds to the LangGraph state CodeAnalysisState and to class CodeAnalysis - report_sections and consolidated_report

First analyse these 

We want to extend the parent LangGraph Code Analysis @ 

IMPLEMENTATION PHASE:

We want to add the new nodes below that are executed after process_code_chunks: 

Create Data Model Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the data_model element from each, and use them all as context to ask Anthropic to produce a report on the data model and data architecture within the whole code repo. 

Create Interfaces Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the interfaces element from each, and use them all as context to ask Anthropic to produce a report on the interfaces within the whole code repo. 

Create business logic Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the business_logic element from each, and use them all as context to ask Anthropic to produce a report on the business logic within the whole code repo. 

Create dependencies Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the dependencies element from each, and use them all as context to ask Anthropic to produce a report on the dependencies within the whole code repo. 

Create configuration Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the configuration element from each, and use them all as context to ask Anthropic to produce a report on the configuration within the whole code repo. 

Create infrastructure Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the infrastructure element from each, and use them all as context to ask Anthropic to produce a report on the infrastructure within the whole code repo. 

Create non functional Report Node -> Input CodeAnalysisState, Output -> update report_sections in CodeAnalysisState. Logic -> for every chunk in analyzed_code_chunks - take the non_functional element from each, and use them all as context to ask Anthropic to produce a report on the non functional aspects within the whole code repo, e.g. security. 

Create consolidated report node -> Input CodeAnalysisState, Output -> update consolidated_report in CodeAnalysisState. Logic -> for every element in state.report_sections, flatten into a single string and set the consolidated_report. 

VERIFICATION PHASE:
There will be 8 new nodes appended onto the existsing parent code analysis graph after process_code_chunks 

CONSTRAINTS:
- Maintain consistency with existing codebase
- Follow the cursor rules
- Do not add any unit tests at this time

```

