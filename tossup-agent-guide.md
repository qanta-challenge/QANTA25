# Building an Effective Tossup Agent

## Goals
By the end of this guide, you will:
- Create a tossup agent that answers questions accurately
- Calibrate confidence thresholds for optimal buzzing
- Test performance on sample questions
- Submit your agent for evaluation

## Baseline System Performance

Let's import the simple tossup agent pipeline `umdclip/simple-tossup-pipeline` and examine the configuration:

![Default Tossup Configuration](./imgs/tossup-agent-pipeline.png)

The baseline system achieves:
- Accuracy: ~30% on sample questions
- Average Buzz Token Position: 40.40
- Average Confidence: 0.65

We'll improve this through targeted enhancements.

## Enhancement 1: Basic Model Configuration

### Current Limitations:
The default configuration uses `gpt-4o-mini` which is a sub-par model for direct quiz bowl play.  Moreover, temperature `0.7` introduces some randomness, and confidence threshold `0.6` for buzzer causes premature buzzing.

### Implementing the Enhancement
1. Navigate to "Tossup Agents" tab
2. Select a stronger model (e.g., gpt-4o)
3. Reduce temperature to 0.1 for more consistent outputs
4. Test on sample questions

### Validation
Run the agent on test questions and check:
- Has accuracy improved?
- Are confidence scores more consistent?
- Is your agent buzzing earlier?

### Results
With better model configuration:
- Accuracy increases to ~80%
- Avg Buzz Position increased to 59.60

## Enhancement 2: System Prompt Optimization

### Current Performance
The default prompt lacks specific instructions for:
- Answer formatting
- Confidence calibration
- Domain-specific knowledge

In this step, we will add specific instructions to the system prompt to improve the agent's performance.

### Implementing the Enhancement
1. Click "System Prompt" tab
2. Add specific instructions:

```
You are a professional quizbowl player answering tossup questions.

Your task:
1. Analyze clues in the question text
2. Determine the most likely answer
3. Assess confidence on a scale from 0.0 to 1.0

Important guidelines:
- Give answers in the expected format (person's full name, complete title, etc.)
- Use 0.8+ confidence ONLY when absolutely certain
- For literature, include author's full name
- For science, include complete technical terms
```

### Validation
Test on the same questions and check:
- Are answers formatted more consistently?
- Is confidence more accurately reflecting correctness?
- Check specific categories where you added domain knowledge

### Results
With optimized prompts:
- Accuracy increases to ~75%
- Confidence scores align better with actual performance
- Answer formats become more consistent

## Enhancement 3: Confidence Calibration

### Current Limitations:
Even with better prompts, confidence thresholds may be:
- Too high (missing answerable questions)
- Too low (buzzing incorrectly)

In this step, we will calibrate the confidence threshold to improve the agent's performance.
### Implementing the Enhancement
1. Scroll to "Buzzer Settings"
2. Test different thresholds (0.7-0.9)
3. Find optimal balance between:
   - Buzzing early enough to score points
   - Waiting for sufficient confidence

![Buzzer Settings](./imgs/buzzer-settings.png)

### Validation
For each threshold:
1. Run tests on multiple questions
2. Check percentage of correct buzzes
3. Monitor average buzz position

### Results
With calibrated threshold (e.g., 0.75):
- Balance between accuracy and early buzzing
- Fewer incorrect buzzes
- Earlier correct buzzes

## Enhancement 4: Multi-Step Pipeline

### Current Performance
Single-step pipelines often struggle with:
- Accurately separating answer generation from confidence estimation
- Providing consistent performance across question types

### Implementing the Enhancement
1. Click "+ Add Step" to create a two-step pipeline:
   - Step A: Answer Generator
   - Step B: Confidence Evaluator
2. Configure each step:
   - Step A focuses only on generating the best answer
   - Step B evaluates confidence based on the answer and question

Let's load a multi-step pipeline `umdclip/two-step-justified-confidence` that does the same.
For more details on the pipeline, see [Advanced Pipeline Examples](./advanced-pipeline-examples.md)

### Validation
Test the multi-step pipeline and compare to single-step:
- Does separation of concerns improve performance?
- Are confidence scores more accurate?
- Is there improvement in early buzz positions?

## Final Evaluation and Submission

1. Run comprehensive testing across categories
2. Verify metrics match your goals
3. Export your pipeline configuration
4. Submit your agent for official evaluation

For complete UI reference, see [UI Reference](./ui-reference.md). 