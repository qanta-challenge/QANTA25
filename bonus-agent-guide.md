# Building an Effective Bonus Agent

## Goals
By the end of this guide, you will:
- Create a bonus agent that answers multi-part questions accurately
- Generate well-calibrated confidence scores
- Provide clear explanations for answers
- Submit your agent for evaluation

## Bonus Question Structure
Bonus questions consist of:
- A leadin that introduces the topic
- 3 parts of increasing difficulty (easy, medium, hard)
- Each part tests specific knowledge within the topic

## Baseline System Performance

The default bonus agent achieves:
- Accuracy: ~45% across all parts
- Higher accuracy on easy parts, lower on hard parts
- Generic explanations with limited reasoning
- Poorly calibrated confidence scores

We'll improve this through targeted enhancements.

## Enhancement 1: Basic Model Configuration

### Current Performance
The default configuration uses:
- Model: gpt-4o-mini
- Temperature: 0.7
- Default system prompt
- Limited instructions for answer structure

### Implementing the Enhancement
1. Navigate to "Bonus Round Agents" tab
2. Select a stronger model (e.g., gpt-4o)
3. Reduce temperature to 0.3 for balance between consistency and creativity
4. Test on sample questions

### Validation
Run the agent on test questions and check:
- Has overall accuracy improved?
- Are answer formats more consistent?
- Have explanations improved in quality?

### Results
With better model configuration:
- Accuracy increases to ~60%
- More consistent answer formats
- Slightly improved explanations

## Enhancement 2: System Prompt Optimization

### Current Performance
The default prompt lacks:
- Clear output formatting instructions
- Guidelines for explanation quality
- Category-specific knowledge

### Implementing the Enhancement
1. Click "System Prompt" tab
2. Add specific instructions:

```
You are an expert quizbowl player specializing in bonus questions.

For each bonus part:
1. Analyze both the leadin and the specific part text
2. Identify key clues and relationships
3. Determine the precise answer
4. Format your answer according to expectations
5. Assess your confidence from 0.0 to 1.0
6. Explain your reasoning with specific clues

Format your response as:
ANSWER: <your specific, concise answer>
CONFIDENCE: <numerical value between 0.0 and 1.0>
EXPLANATION: <detailed reasoning connecting clues to your answer>
```

### Validation
Test on the same questions and check:
- Are answers better formatted?
- Are explanations more detailed and relevant?
- Is confidence more accurately calibrated?

### Results
With optimized prompts:
- Accuracy increases to ~70%
- Explanations include specific clues from questions
- Confidence scores better reflect actual knowledge

## Enhancement 3: Input-Output Structure

### Current Performance
Default configuration may not:
- Properly utilize leadin context
- Consistently format outputs
- Handle multi-part relationships

### Implementing the Enhancement
1. Click the "Inputs" tab to verify:
   - "leadin" variable is used appropriately
   - "part" variable is clearly defined
   
2. Click the "Outputs" tab to verify:
   - "answer" output is properly typed as string
   - "confidence" output is defined as float
   - "explanation" output is defined as string

![Inputs Tab](./imgs/inputs-tab.png)
![Outputs Tab](./imgs/outputs-tab.png)

### Validation
Test and verify:
- Does agent use leadin information effectively?
- Are all three outputs (answer, confidence, explanation) properly formatted?
- Does agent maintain consistency across multiple bonus parts?

### Results
With optimized I/O structure:
- Better integration of leadin information
- Consistent output formatting
- More cohesive answers across related parts

## Enhancement 4: Multi-Step Pipeline

### Current Performance
Single-step pipelines often struggle with:
- Balancing answer generation with explanation
- Accurately calibrating confidence

### Implementing the Enhancement
1. Click "+ Add Step" to create a two-step pipeline:
   - Step A: Answer Generator
   - Step B: Explanation and Confidence Evaluator
2. Configure each step:
   - Step A focuses only on determining the correct answer
   - Step B evaluates confidence and provides detailed explanation

### Validation
Test the multi-step pipeline and compare to single-step:
- Has answer accuracy improved?
- Are explanations more detailed and accurate?
- Is confidence better calibrated across difficulty levels?

### Results
With multi-step pipeline:
- Accuracy increases to ~80%
- Explanations include specific clues and reasoning
- Confidence scores accurately reflect answer certainty
- Better performance across all difficulty levels

## Final Evaluation and Submission

1. Run comprehensive testing across bonus question categories
2. Verify metrics match your goals
3. Export your pipeline configuration
4. Submit your agent for official evaluation

## Integrating Advanced Features

For even better performance, see [Advanced Pipeline Examples](./advanced-pipeline-examples.md) to explore:
- Specialized models for different question categories
- Knowledge graph integration
- Contextual reasoning across bonus parts

For complete UI reference, see [UI Reference](./ui-reference.md). 