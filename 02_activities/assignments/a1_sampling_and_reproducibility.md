# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Laura Siracusa

```
Please write your explanation here...
The `whitby_covid_tracing.py` script implements a simulation of COVID-19 infection and contact tracing across two different types of events: weddings and brunches. After careful analysis, I identified the following sampling stages in the model:

1. **Event Type Assignment**:
   - Function: Initial setup in `simulate_event()`
   - Sample size: Fixed at 1000 people total (200 for weddings, 800 for brunches)
   - Sampling frame: N/A (deterministic assignment)
   - Distribution: Fixed 1:4 ratio of wedding:brunch attendees
   - This reflects the blog post's scenario of different types of gatherings with varying attendance sizes

2. **Initial Infection Sampling**:
   - Function: `np.random.choice()` in `simulate_event()`
   - Sample size: 10% of the population (100 people) determined by `ATTACK_RATE = 0.10`
   - Sampling frame: All 1000 individuals
   - Distribution: Uniform (simple random sampling without replacement)
   - This models the random spread of infection with the same attack rate across both event types

3. **Primary Contact Tracing Sampling**:
   - Function: `np.random.rand()` comparison in `simulate_event()`
   - Sample size: Approximately 20% of infected individuals, per `TRACE_SUCCESS = 0.20`
   - Sampling frame: Only infected individuals
   - Distribution: Bernoulli trials (binomial distribution)
   - This represents the random success of contact tracing for individual cases

4. **Secondary Contact Tracing**:
   - Function: Conditional logic in `simulate_event()`
   - This is not true random sampling but a deterministic process
   - If an event has at least 2 traced cases (`SECONDARY_TRACE_THRESHOLD = 2`), all infected people at that event are marked as traced
   - This models the "cluster identification" described in the blog post, where events with multiple cases get more attention

5. **Repetition Sampling**:
   - Function: List comprehension calling `simulate_event()` multiple times
   - Sample size: 1000 independent simulation runs
   - This creates a distribution of outcomes to analyze the statistical properties of the model

The simulation demonstrates how selective contact tracing can create a biased picture of infection sources, just as described in the blog post. While both event types have the same infection rate (10%), the smaller weddings are more likely to be fully contact-traced due to the secondary tracing threshold, leading to an overrepresentation of wedding cases in the traced dataset.

**Reproducibility Analysis**

When running the original script multiple times, I observed that the results vary between runs. This non-reproducibility occurs because the script uses random number generation without setting a fixed seed. Specifically:

1. The infection process uses `np.random.choice()` to randomly select which individuals become infected
2. The primary contact tracing uses `np.random.rand()` to determine which infected individuals are traced

When changing the simulation runs from 1000 to 100, the non-reproducibility becomes even more apparent as there's greater variability in the histograms between runs due to smaller sample size.

**Code Modifications for Reproducibility**

To make the code reproducible, I added a fixed random seed at the beginning of the script:
# Set a fixed random seed for reproducibility
np.random.seed(42)
```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 09/04/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
