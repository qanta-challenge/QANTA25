# Advanced Pipeline Examples

This guide shows how to implement and validate sophisticated multi-step pipelines for quizbowl agents.

## Goals
Using advanced pipelines, you will:
- Improve accuracy by 15-25% over single-step agents
- Create specialized components for different tasks
- Implement effective confidence calibration
- Build robust buzzer strategies

## Two-Step Justified Confidence Pipeline

### Baseline Performance
Standard single-step agents typically achieve:
- Accuracy: ~65-70%
- Poorly calibrated confidence
- Limited explanation for answers

### Loading the Pipeline Example

1. Navigate to the "Tossup Agents" tab
2. Click "Select Pipeline to Import..." and choose "two-step-justified-confidence.yaml"
3. Click "Import Pipeline"

### Understanding the Pipeline Structure

This pipeline has two distinct steps:

#### Step A: Answer Generator
- Uses OpenAI/gpt-4o-mini
- Takes question text as input
- Generates an answer candidate
- Focuses solely on accurate answer generation

#### Step B: Confidence Evaluator
- Uses Cohere/command-r-plus
- Takes question text AND generated answer from Step A
- Evaluates confidence and provides justification
- Specialized for confidence assessment

### Validation
Test the pipeline and check:
- Is accuracy improved compared to single-step?
- Are confidence scores better calibrated?
- Does the justification explain reasoning clearly?

### Results
Two-step justified confidence typically achieves:
- Accuracy: ~80-85%
- Well-calibrated confidence scores
- Clear justification for answers and confidence
- More strategic buzzing

## Enhancing the Two-Step Pipeline

### Step 1: Upgrade Answer Generator

#### Current Performance
The default example uses gpt-4o-mini which may lack:
- Specialized knowledge in some areas
- Consistent answer formatting

#### Implementation
1. Click on Step A
2. Change model to a stronger option (e.g., gpt-4o)
3. Modify system prompt to focus on answer precision

#### Validation
Test with sample questions and check:
- Has answer accuracy improved?
- Is formatting more consistent?

#### Results
With upgraded answer generator:
- Accuracy increases to ~85-90%
- More consistent answer formats

### Step 2: Improve Confidence Evaluator

#### Current Performance
The default evaluator may:
- Over-estimate confidence on some topics
- Provide limited justification

#### Implementation
1. Click on Step B
2. Enhance the system prompt:
```
You are an expert confidence evaluator for quizbowl answers.

Your task:
1. Evaluate ONLY the correctness of the provided answer
2. Consider question completeness and available clues
3. Provide specific justification for your confidence score
4. Be especially critical of answers with limited supporting evidence

Remember:
- Early, difficult clues justify lower confidence
- Later, obvious clues justify higher confidence
- Domain expertise should be reflected in your assessment
```

#### Validation
Test and verify:
- Are confidence scores better aligned with correctness?
- Does justification include specific clues from questions?
- Is confidence calibrated appropriately for question position?

#### Results
With improved evaluator:
- More accurate confidence calibration
- Detailed justifications citing specific clues
- Better buzzing decisions

## Three-Step Pipeline with Analysis

### Concept
Adding a dedicated analysis step before answer generation:

1. **Step A: Question Analyzer**
   - Identifies key clues, entities, and relationships
   - Determines question category and format
   
2. **Step B: Answer Generator**
   - Uses analysis to generate accurate answers
   - Focuses on formatting and precision
   
3. **Step C: Confidence Evaluator**
   - Assesses answer quality based on analysis and clues
   - Determines optimal buzz timing

### Implementation
Create this pipeline from scratch or modify the two-step example.

### Validation
Compare to the two-step pipeline:
- Does the analysis step improve answer accuracy?
- Does it provide better performance on difficult questions?
- Are there improvements in early buzzing?

### Results
Three-step pipelines typically achieve:
- Accuracy: ~90-95%
- Earlier correct buzzes
- Exceptional performance on difficult questions

## Specialty Pipeline: Literature Focus

### Concept
Create a pipeline specialized for literature questions:

1. **Step A: Literary Analyzer**
   - Identifies literary techniques, periods, and styles
   - Recognizes author-specific clues
   
2. **Step B: Answer Generator**
   - Specialized for literary works and authors
   - Formats answers according to literary conventions
   
3. **Step C: Confidence Evaluator**
   - Calibrated specifically for literature questions

### Implementation
Create specialized system prompts for each step focusing on literary knowledge.

### Validation
Test specifically on literature questions and compare to general pipeline.

### Results
Specialty pipelines can achieve:
- 95%+ accuracy in their specialized domain
- Earlier buzzing on category-specific questions
- Better performance on difficult clues

## Best Practices for Advanced Pipelines

1. **Focused Components**: Each step should have a clear, single responsibility
2. **Efficient Communication**: Pass only necessary information between steps
3. **Strong Fundamentals**: Start with a solid two-step pipeline before adding complexity
4. **Consistent Testing**: Validate each change against the same test set
5. **Strategic Model Selection**: Use different models for tasks where they excel