# Quizbowl Agent Goals and Evaluation

## Objectives

### Tossup Agents
- Respond to questions with the best guess with calibrated confidence
- Buzz at the earliest possible moment with sufficient information
- Avoid incorrect buzzes
- Maintain consistent performance across topics

### Bonus Agents
- Answer parts correctly with accurate confidence estimation
- Provide clear explanation of reasoning which will be used by human team members to validate / pick the suggested answer.
- Adapt to varying difficulty levels (easy, medium, hard)

## Performance Metrics

### Tossup Metrics
- **Avg Score**: Average points earned per question [+10 for correct, 0 for miss, -5 for incorrect]
- **Buzz Accuracy**: Correctness accuracy of buzzes made by the agent
- **Avg Buzz Position**: How early in the question you buzz (earlier is better)
- **Confidence Calibration**: How well confidence score matches actual performance
- **+ve Gap**: For all questions where the agent buzzed correctly, average difference between the agent's buzz position and its optimal buzz position (earliest correct buzz position)
- **-ve Gap**: For all questions where the agent buzzed incorrectly, average difference between the agent's buzz position and its optimal buzz position (earliest correct buzz position), or the last token index.
### Bonus Metrics
- **Accuracy**: Percentage of correct answers across all parts
- **Confidence Calibration**: How well confidence score matches actual performance
- **Explanation Quality**: Relevance and clarity of reasoning

## Evaluating Your Agent

### Testing Baseline Performance
1. Run the default agent configuration
2. Record metrics (accuracy, confidence, buzz position)
3. Identify specific weaknesses in performance

### Validating Improvements
After each enhancement:
1. Run the agent on the same development set of questions
2. Compare metrics to previous version
3. Check for improvements in weak areas

### Final Evaluation Criteria
Your final agent will be evaluated on:
1. Overall accuracy across diverse questions
2. Optimal buzz timing (neither too early nor too late)
3. Confidence threshold calibration
4. Explanation quality (for bonus agents)

<!-- ## Setting Goals for Your Agent

### Minimum Goals
- Accuracy above 60%
- Appropriate confidence threshold (0.7-0.9)
- Reasonable buzz positions

### Advanced Goals
- Multi-step pipelines with specialized components
- Accuracy above 85%
- Strategic early buzzing on familiar topics
- Detailed, accurate explanations for bonus questions  -->