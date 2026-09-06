# Understanding Prompts

Improving how you prompt a foundation model directs generative artificial intelligence behaviour without adjusting model parameters or using fine tuning. Prompting supplies domain knowledge, equips external tools, reinforces safety measures, and increases output quality.

```mermaid
mindmap
  root((Prompt Structure))
    Instructions
      Task description
      Behaviour rules
    Context
      Background detail
      Domain guidance
    Input Data
      Raw content
      Queries or records
    Output Indicator
      Format rules
      Target structure
    Negative Prompting
      Excluded content
      Restricted behaviours
```

## Core Elements of a Prompt

|**Element**|**Purpose**|**Example from Scenario**|
|---|---|---|
|Instructions|The explicit task the model must run|"Given a list of customer orders and available inventory, determine which orders can be fulfilled and which items have to be restocked."|
|Context|External background guiding model reasoning|"This task is essential for inventory management and order fulfillment processes in ecommerce or retail businesses."|
|Input Data|Specific records processed by the model|"Orders: Order 1: Product A (5 units)... Inventory: Product A: 8 units..."|
|Output Indicator|Target shape, type, or formatting tag|"Fulfillment status:"|

## Negative Prompting

Negative prompting explicitly defines outputs the model must avoid.

  

- Steers responses away from targeted behaviours, unsafe outputs, explicit material, or biased phrasing.
    
      
    
- Uses direct restrictions or counter examples to keep outputs within technical or safety boundaries.
    
      
    

## Prompt Scenario Comparison

|**Prompt Text**|**Included Elements**|**Missing Elements**|**Technical Assessment**|
|---|---|---|---|
|"Generate a market analysis report for a new product category."|Instructions|Context, Input Data, Output Indicator|Too generic. Without parameters, source records, or a target format, the model generates unfocused, generic prose.|
|Order Fulfilment Sample|Instructions, Context, Input Data, Output Indicator|None|Complete. Supplies raw data, operating scenario, task logic, and an explicit final marker for generation.|

```mermaid
graph TD
    A[Prompt Request] --> B{Contains Core Elements?}
    B -->|Instructions Only| C[Generic Output]
    B -->|All Elements Present| D[Precise Production Output]
    D --> E[Negative Prompt Applied]
    E --> F[Safe Compliant Result]
```

## Questions You Might Have Missed

**How does negative prompting work in foundation models that do not have a separate negative prompt field?**

  

You append negative rules directly into the instruction block using clear constraints, such as explicit lists of excluded phrases, formats, or topics.

  

**Why is an output indicator useful when automating pipelines with large language models?**

  

It sets an exact completion trigger (such as a trailing tag or JSON key) so the model begins producing the required schema immediately without conversational filler.

# Modifying Prompts and Inference Parameters

Foundation model outputs depend directly on prompt construction and inference parameter configuration. Adjusting these settings shapes response structure, token limits, and probability distributions without altering underlying model weights.

```mermaid
mindmap
  root((Model Inference Control))
    Randomness and Diversity
      Temperature
        Low: Conservative and deterministic
        High: Creative and varied
      Top P Nucleus Sampling
        Cumulative probability cut off
        Dynamic candidate pool
      Top K
        Fixed count cut off
        Static candidate pool
    Length Controls
      Maximum Length
        Token ceiling
        Prevents resource waste
      Stop Sequences
        Halt triggers
        Custom delimiters
    Prompt Design
      Clarity and Conciseness
      Context Delivery
      Explicit Directives
      Example Responses Few Shot
      Task Decomposition
      Standard Templates
```

## Inference Parameters

Inference parameters are runtime configurations passed alongside the prompt to control generation dynamics.


```mermaid
flowchart TD
    Prompt[Prompt Input] --> Vocab[Vocabulary Probability Distribution]
    Vocab --> Temp[Temperature Scaling]
    Temp --> Filter{Sampling Strategy}
    Filter -->|Top K| TK[Retain K Highest Probability Tokens]
    Filter -->|Top P| TP[Retain Smallest Set Exceeding Cumulative Probability P]
    TK --> Sample[Final Token Selection]
    TP --> Sample
    Sample --> Check{Termination Check}
    Check -->|Token Limit Reached| Stop[Halt Generation]
    Check -->|Stop Sequence Encountered| Stop
    Check -->|Next Token| Vocab
```

### Randomness and Diversity Parameters

|**Parameter**|**Type**|**Value Range**|**Operational Mechanism**|**Exam Scenario Target**|
|---|---|---|---|---|
|**Temperature**|Probability flatter / peak|0.0 to 1.0|Adjusts the probability distribution curve. Lower values steepen the curve, making top tokens more dominant. Higher values flatten the distribution, increasing the chance of lower probability tokens.|Use 0.0 to 0.2 for deterministic tasks like code generation, maths, or classification. Use 0.7 to 1.0 for creative drafting.|
|**Top P**(Nucleus)|Cumulative probability threshold|0.0 to 1.0|Selects tokens dynamically from the smallest group whose cumulative probability sums to P. Words outside this threshold are discarded.|Use low values (0.2 to 0.3) for strict factual responses. Use high values (0.9 to 1.0) to allow rich vocabulary variety.|
|**Top K**|Absolute candidate count|Integer (e.g. 1 to 500+)|Limits token selection strictly to the top K most probable words, regardless of the individual or cumulative percentage probabilities.|Set to 10 for tight, focused outputs. Set to 500 to expand the token candidate pool across broader responses.|

### Length and Termination Parameters

|**Parameter**|**Function**|**Failure Mode If Misconfigured**|
|---|---|---|
|**Maximum Length**|Sets the ceiling for total tokens generated during the inference cycle. Protects compute resources from runaways.|Set too low: Truncated code or mid sentence cut offs. Set too high: Infinite generation loops or excessive billing cost.|
|**Stop Sequences**|Defines specific tokens or strings that command the model to halt processing immediately.|Missing: Model generates trailing conversation, hallucinates extra turns, or ignores prompt bounds.|

## Prompt Design Framework

Structured prompt authoring removes ambiguity and reduces validation overhead in automated pipelines.

  

|**Technique**|**Implementation Strategy**|**Deficient Example**|**Production Example**|
|---|---|---|---|
|**Direct Language**|Cut wordiness. Use explicit instruction.|"Compute the sum total of the subsequent sequence of numerals: 4, 8, 12, 16."|"What is the sum of these numbers: 4, 8, 12, 16?"|
|**Context Insertion**|Provide domain metadata and operational boundaries.|"Summarise this article: [text]"|"Provide a summary of this article to be used in a blog post: [text]"|
|**Explicit Directives**|Specify constraints, length, and exact response type.|"What is the capital?"|"What is the capital of New York? Provide the answer in a full sentence."|
|**Terminal Placement**|Place output parameters at the absolute end of the input string.|"Calculate the area of a circle."|"Calculate the area of a circle with a radius of 3 inches (7.5 cm). Round your answer to the nearest integer."|
|**Interrogative Leads**|Start prompts directly with who, what, where, when, why, or how.|"Summarise this event."|"Why did this event happen? Explain in three sentences."|
|**In Prompt Demonstrations**|Wrap reference demonstrations within brackets to define input to output relationships.|"Determine the sentiment of this social media post: [post]"|"Determine the sentiment using these examples:\npost: 'great pen' => Positive\npost: 'I hate when my phone battery dies' => Negative\n[post] =>"|
|**Task Decomposition**|Split complex procedures into linked sequential queries or request step by step reasoning.|"Evaluate this architecture, review costs, and draft migration steps."|Split into three chained steps: 1. Evaluate architecture. 2. Assess cost drivers. 3. Produce migration sequence.|
|**Template Scaffolding**|Maintain structural uniformity using reusable variables for instructions, context, and data.|Ad hoc unformatted prompts sent to APIs.|Standardised templates containing fixed input keys and system roles across workloads.|

## Production Scenario Evolution

```plantuml
@startuml
skinparam handwritten false
skinparam monochrome true
skinparam packageStyle rectangle
skinparam shadowing false

card "Original Input" as P1 {
  rectangle "Generate a market analysis report for a new product category." as T1
}

card "Refined Production Configuration" as P2 {
  rectangle "Inference Parameters:\n• Temperature: High (0.8)\n• Top P: High (0.95)\n• Max Length: 5000 tokens" as C1
  rectangle "Context Injection:\n• Target Market: Finance Sector\n• Product Scope: Corporate Cash Management" as C2
  rectangle "Structural Directives:\n• Section 1: Executive Summary\n• Section 2: Market Competitors\n• Section 3: Regulatory Factors" as C3
}

P1 --> P2 : Apply Best Practice Framework
@enduml
```

### Analysis of Scenario Improvements

- **Parameter Selection:** Temperature and Top P set high to widen candidate selection across market trends, allowing the model to surface edge case considerations. Max length set to 5000 tokens to ensure the report does not terminate mid section.
    
      
    
- **Context Provision:** Explicitly specifies the finance sector, ensuring the model avoids generic commercial retail assumptions.
    
      
    
- **Structural Directives:** Splits the report into named headings, ensuring predictable parsing when feeding downstream systems.
    
      
    

## Questions You Might Have Missed

**How do Top P and Top K interact when both are enabled concurrently?**

  

When both parameters are active, Top K applies first to truncate the pool to the K most likely tokens. Top P then evaluates that reduced pool, selecting the smallest subset whose cumulative probability meets the P threshold.

  

**Why does increasing Temperature risk hallucinations in structured data outputs?**

  

Higher temperature flattens token probabilities. This allows low probability tokens to be selected, frequently breaking strict syntax structures like JSON or YAML brackets and inventing invalid key names.

  

**When should you prefer Stop Sequences over setting a strict Maximum Length?**

  

Stop sequences should be used when the exact size of the payload is dynamic or variable (such as interactive chat turns or script generations), whereas maximum length serves as an absolute hard boundary to prevent runaway token spend.

# Prompt Engineering Techniques

Prompt engineering techniques structure input text to steer generative foundation models toward accurate results.

```mermaid
mindmap
  root((Prompt Engineering))
    Zero Shot
      No prior examples
      Relies on foundation model capability
      Enhanced by instruction tuning
    Few Shot
      Provides contextual demonstrations
      Single shot variant
      Diverse representative inputs
    Chain of Thought
      Intermediate reasoning steps
      Multi step logic problems
      Triggered by Think step by step
```

## Technique Comparison

|**Technique**|**Description**|**Ideal Use Case**|**Key Mechanism**|
|---|---|---|---|
|Zero Shot|Submitting a task without reference samples|Straightforward queries on large capability models|Model generalisation, instruction tuning, RLHF|
|Few Shot|Providing one or more context examples|Structured outputs, format matching, domain adaptation|In context learning via input and output pairs|
|Chain of Thought (CoT)|Splitting complex logic into intermediary steps|Arithmetic reasoning, multi step deductions|Step by step sequential generation|

## Zero Shot Prompting

Zero shot prompting presents a prompt to the model without explicit training instances or demonstration samples.

  

### Core Concepts

- Relies on the foundation model's pre trained capabilities to interpret intent.
    
      
    
- Model scale correlates with success: larger foundation models execute zero shot instructions with greater reliability.
    
      
    
- Reinforced by instruction tuning and reinforcement learning from human feedback (RLHF) to align generation with task directions.
    
      
    

### Example

> **Prompt:**
> 
>   
> 
> Tell me the sentiment of the following social media post and categorise it as positive, negative, or neutral:
> 
>   
> 
> Huge shoutout to the amazing team at AnyCompany! Your top notch customer service continues to blow me away. Proud to be a loyal customer!
> 
>   
> 
> **Output:**
> 
>   
> 
> Positive
> 
>   

## Few Shot Prompting

Few shot prompting supplies explicit input and output demonstration pairs inside the context window before asking for the final completion.

```mermaid
flowchart TD
    A[Instruction] --> B[Demonstration 1: Input + Output]
    B --> C[Demonstration 2: Input + Output]
    C --> D[Target Input]
    D --> E[Model Output]
```

### Core Concepts

- Samples condition the model to generate the specified format and reasoning style.
    
      
    
- Single shot (or one shot) uses one demonstration; few shot uses two or more.
    
      
    
- Samples must be representative, diverse, and clean. Excessive samples risk introducing context noise.
    
      
    

### Example

> **Prompt:**
> 
>   
> 
> Tell me the sentiment of the following news headline and categorise it as positive, negative, or neutral. Here are some examples:
> 
>   
> 
> Investment firm fends off allegations of corruption
> 
>   
> 
> Answer: Negative
> 
>   
> 
> Local teacher awarded with national prize
> 
>   
> 
> Answer: Positive
> 
>   
> 
> Community organisation exceeds fundraising goal, to provide meals for thousands in need
> 
>   
> 
> Answer:
> 
>   
> 
> **Output:**
> 
>   
> 
> Positive
> 
>   

### Application Scenario

Generating market documentation using existing organisation assets:

```
Generate a comprehensive market analysis report for a new product category in the finance industry. The target audience is small and medium businesses (SMBs). Use the attached template to structure the report into categories. [attach report template]

The following examples are market analysis reports for previously released products.
Example 1: [insert example market analysis report]
Example 2: [insert example market analysis report]
```

## Chain of Thought (CoT) Prompting

Chain of thought prompting directs the model to output intermediate deduction steps before arriving at a final answer. It addresses problems where direct generation leads to arithmetic or reasoning failure.

```mermaid
flowchart LR
    A[Input Query] --> B[Intermediary Step 1]
    B --> C[Intermediary Step 2]
    C --> D[Final Deduction]
```

### Core Concepts

- Breaks complex reasoning tasks into linear stages.
    
      
    
- Compatible with both zero shot and few shot approaches.
    
      
    
- Typically initiated by appending instructions such as `"Think step by step"`.
    
      
    

### Zero Shot CoT Example

> **Prompt:**
> 
>   
> 
> Which service requires a larger deposit based on the following information?
> 
>   
> 
> The total cost of service A is $50,000, and it requires a 30 percent deposit.
> 
>   
> 
> The total cost of service B is $40,000, and it requires a 40 percent deposit.
> 
>   
> 
> Think step by step.
> 
>   
> 
> **Output:**
> 
>   
> 
> The deposit for service A is 30 percent of $50,000, which is 0.3 * 50,000 = $15,000
> 
>   
> 
> The deposit for service B is 40 percent of $40,000, which is 0.4 * 40,000 = $16,000
> 
>   
> 
> Therefore, based on the given information, service B requires a larger deposit.
> 
>   

### Few Shot CoT Example

> **Prompt:**
> 
>   
> 
> Question: If there are 2 bags with 3 oranges each, how many oranges are there in total?
> 
>   
> 
> Answer: 2 bags, 3 oranges each. 2 * 3 = 6 oranges.
> 
>   
> 
> Question: If there are 4 cars with 2 passengers each, how many passengers are there in total?
> 
>   
> 
> Answer: 4 cars, 2 passengers each. 4 * 2 = 8 passengers.
> 
>   
> 
> Question: If there are 3 baskets, and each basket has 5 apples, how many apples are there in total?
> 
>   
> 
> Answer: Think step by step.
> 
>   
> 
> **Output:**
> 
>   
> 
> Answer: 3 baskets, 5 apples each. 3 * 5 = 15 apples.
> 
>   

## Exam Review Points

**What is the primary operational difference between zero shot and few shot prompting?**

  

Zero shot relies entirely on pre trained parameters without demonstrations, whereas few shot provides explicit target examples within the prompt context.

  

**How does reinforcement learning from human feedback (RLHF) benefit zero shot tasks?**

  

It aligns the base model with natural instructions, allowing it to interpret task goals without requiring explicit demonstrations.

  

**What specific phrase triggers reasoning breakdowns in zero shot Chain of Thought?**

  

The directive `"Think step by step."`

  

**When should few shot examples be limited?**

  

When the context length reaches token limits or when superfluous examples introduce noise that degrades output precision.