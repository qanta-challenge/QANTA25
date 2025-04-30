# Best Practices for Quizbowl Agents

## System Prompt Design

### Effective Prompt Structure
- Start with a clear role definition ("You are an expert quizbowl player")
- List specific tasks in order of execution
- Include formatting requirements with examples
- Add domain-specific knowledge for key categories

### Category-Specific Instructions
- **Literature**: Include author recognition patterns
- **History**: Specify date formatting and dynasty conventions
- **Science**: Define technical term conventions
- **Fine Arts**: Clarify requirements for artist/work identification

### Example: Strong Tossup Prompt
```
You are an expert quizbowl player specializing in accurately answering tossup questions.

For each question:
1. Identify key clues, people, places, and events
2. Match clues to potential answers
3. Select the most precise answer based on available information
4. Format your answer according to standard conventions
5. Assess confidence based on clue specificity and your knowledge

Follow these formatting rules:
- Person: Full name (e.g., "Albert Einstein")
- Work: Complete title (e.g., "The Great Gatsby")
- Scientific terms: Full technical name (e.g., "Photosynthesis")

Confidence levels:
- 0.9-1.0: Use ONLY when clues uniquely identify one answer
- 0.7-0.8: Strong evidence but multiple possible answers
- 0.5-0.6: Reasonable guess based on limited clues
- 0.1-0.4: Uncertain, based on vague connections
```

## Confidence Calibration

### Threshold Selection
- Start with 0.75-0.8 as initial threshold
- Decrease for aggressive buzzing (more points, more errors)
- Increase for conservative buzzing (fewer errors, fewer points)
- Optimal setting typically falls between 0.7-0.85

### Testing Across Question Types
- Test on early clues (hard, specific)
- Test on middle clues (moderate difficulty)
- Test on late clues (easy, well-known)
- Ensure consistent performance across categories

### Signs of Poor Calibration
- Agent buzzes correctly but too late (threshold too high)
- Agent buzzes incorrectly too often (threshold too low)
- Wide confidence swings between adjacent clues (unstable confidence)

## Multi-Step Pipeline Design

### Effective Role Separation
- **Analyzers**: Focus on understanding question structure and clues
- **Generators**: Focus on producing precise answers
- **Evaluators**: Focus on confidence assessment and justification

### Information Flow
- Pass only necessary information between steps
- Structure intermediate outputs clearly
- Use consistent variable naming conventions

### Optimizing Step Interactions
- Ensure step A outputs exactly what step B expects
- Consider the strengths of different models for different tasks
- Minimize redundancy between steps

## Model Selection

### Provider Strengths
- **OpenAI (GPT)**: Strong general knowledge, good for answer generation
- **Anthropic (Claude)**: Strong reasoning, good for analysis
- **Cohere (Command)**: Strong for structured outputs, good for evaluation

### Optimizing by Task
- Use knowledge-heavy models for answer generation
- Use reasoning-focused models for confidence evaluation
- Consider smaller models for simple tasks to reduce latency

## Testing and Iteration

### Systematic Approach
1. Test on a diverse set of questions
2. Identify patterns in errors
3. Categorize failures:
   - Knowledge gaps
   - Confidence issues
   - Format problems
4. Make targeted improvements
5. Retest on the same questions

### Common Issues and Solutions

| Problem | Symptom | Solution |
|---------|---------|----------|
| Over-confidence | Incorrect early buzzes | Adjust confidence threshold, improve evaluator |
| Under-confidence | Late buzzes on clear clues | Lower threshold, enhance confidence calculation |
| Format errors | Correct content, wrong format | Add explicit format instructions |
| Category weakness | Poor performance in specific subjects | Add domain-specific knowledge |

## Error Analysis

### Logging and Review
- Save all question runs with their results
- Compare performance across questions
- Identify patterns in missed questions

### Key Metrics to Track
- Accuracy by category
- Average buzz position
- Confidence vs. correctness correlation
- Error types (near-miss vs. completely wrong)

## Final Optimization

### Pre-Submission Checklist
- Test on diverse question set
- Verify confidence calibration
- Check answer formatting consistency
- Ensure explanation quality for bonus questions
- Export and verify pipeline configuration

### Performance Target Benchmarks
- Accuracy: 80%+ on evaluated questions
- Average buzz position: Before 70% of the question
- Confidence correlation: Strong alignment between confidence and correctness 