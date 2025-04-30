# Quizbowl Pipeline Schema Documentation

## Overview
This document outlines the schema and validation criteria for both Tossup and Bonus pipelines in the Quizbowl system. The pipelines are built using a modular workflow system that allows for flexible configuration of model steps, inputs, outputs, and validation rules.

## Common Components

### ModelStep
```yaml
ModelStep:
  id: string                    # Unique identifier (A, B, C, etc.)
  name: string                  # Human-readable name
  model: string                 # Model identifier (e.g., "gpt-4o-mini")
  provider: string              # Model provider (e.g., "OpenAI")
  call_type: string             # One of: "llm", "search", "python_func"
  temperature: float            # Optional: Model temperature (0.0-5.0)
  system_prompt: string         # Instructions for the model
  input_fields:                 # List of input fields
    - name: string              # Field name
      description: string       # Human-readable description
      variable: string          # Source variable reference
      func: string              # Optional: Pre-processing function
  output_fields:                # List of output fields
    - name: string              # Field name
      type: string              # One of: "str", "int", "float", "bool", "list[str]", "list[int]", "list[float]", "list[bool]"
      description: string       # Human-readable description
      func: string              # Optional: Post-processing function
```

### Workflow
```yaml
Workflow:
  inputs: string[]             # List of input variable names
  outputs:                     # Map of output variables
    <output_name>: string      # Format: "{step_id}.{field_name}" or null
  steps: ModelStep[]           # List of ModelStep instances
```

## Tossup Pipeline

### Required Input Variables
- `question_text`: The full text of the tossup question

### Required Output Variables
- `answer`: The predicted answer
- `confidence`: Confidence score (0.0-1.0)

### Special Components
Tossup pipelines have a `Buzzer` component at key `buzzer` that determines whether the agent buzzes on a given question.
```yaml
Buzzer:
  method: string                # One of: "AND", "OR"
  confidence_threshold: float   # Minimum confidence to trigger buzz (0.0-1.0)
  prob_threshold: float         # Optional: Log probability threshold

TossupWorkflow:
  inputs: ["question_text"]
  outputs:
    answer: "A.answer"
    confidence: "A.confidence"
  steps: ModelStep[]
  buzzer: Buzzer
```

### Validation Rules
1. Must have at least one step
2. Must include required input variables
3. Must include required output variables
4. Buzzer configuration must be valid:
   - If `prob_threshold` is None, method must be "AND"
   - Cannot provide both `logprob` and `prob` simultaneously
   - Confidence threshold must be between 0.0 and 1.0

## Bonus Pipeline

### Required Input Variables
- `leadin`: The introductory paragraph establishing context
- `part`: The specific part text containing clues

### Required Output Variables
- `answer`: The predicted answer
- `confidence`: Confidence score (0.0-1.0)
- `explanation`: Detailed justification for the answer

### Validation Rules
1. Must have at least one step
2. Must include required input variables
3. Must include required output variables
4. Each part must be processed independently with access to the leadin

## Available Models
```yaml
Models:
  OpenAI:
    - gpt-4o
    - gpt-4o-mini
    - gpt-3.5-turbo
  Anthropic:
    - claude-3-7-sonnet
    - claude-3-5-sonnet
    - claude-3-5-haiku
  Cohere:
    - command-r
    - command-r-plus
    - command-r7b
```


## Example Configurations

### Simple Tossup Pipeline
```yaml
inputs: ["question_text"]
outputs:
  answer: "A.answer"
  confidence: "A.confidence"
steps:
  A:
    id: "A"
    name: "Tossup Agent"
    model: "gpt-4o-mini"
    provider: "OpenAI"
    temperature: 0.3
    call_type: "llm"
    system_prompt: "You are a quizbowl player answering tossup questions..."
    input_fields:
      - name: "question"
        description: "The tossup question text"
        variable: "question_text"
    output_fields:
      - name: "answer"
        description: "The predicted answer"
        type: "str"
      - name: "confidence"
        description: "Confidence in the answer (0-1)"
        type: "float"
```

### Simple Bonus Pipeline
```yaml
inputs: ["leadin", "part"]
outputs:
  answer: "A.answer"
  confidence: "A.confidence"
  explanation: "A.explanation"
steps:
  A:
    id: "A"
    name: "Bonus Agent"
    model: "gpt-4o-mini"
    provider: "OpenAI"
    temperature: 0.3
    call_type: "llm"
    system_prompt: "You are a quizbowl player answering bonus questions..."
    input_fields:
      - name: "question_leadin"
        description: "The leadin text"
        variable: "leadin"
      - name: "question_part"
        description: "The specific part text"
        variable: "part"
    output_fields:
      - name: "answer"
        description: "The predicted answer"
        type: "str"
      - name: "confidence"
        description: "Confidence in the answer (0-1)"
        type: "float"
      - name: "explanation"
        description: "Explanation for the answer"
        type: "str"
```
