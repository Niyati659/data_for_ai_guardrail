# Synthetic Email-Agent Security Dataset

## 1. Overview

This dataset was designed to evaluate machine-learning approaches for detecting prompt-injection attacks and potentially unauthorized actions in LLM-powered email agents.

Unlike datasets containing only isolated prompts, this dataset represents complete email-agent scenarios. Each scenario captures relevant contextual information, including the user's original instruction, email content, sender and recipient details, proposed action, data sensitivity, and authorization context.

The dataset was developed for the research study **"Before the Agent Acts: Mitigating Unauthorized Actions in Artificial Intelligence Email Agents."**

## 2. Dataset Objectives

The dataset was designed to:

- Represent legitimate email operations and malicious email scenarios.
- Capture direct and indirect prompt-injection attempts embedded in email content.
- Evaluate whether models distinguish legitimate requests from attempts to manipulate an email agent.
- Represent authorization conflicts, suspicious recipients, sensitive-data disclosure, impersonation, and credential-exfiltration attempts.
- Provide a standardized input representation for comparing compatible detection models.
- Support evaluation using attack precision, attack recall, F1-score, accuracy, and false-positive rate.

## 3. Sources Used for Scenario Design

The dataset-generation process was informed by existing research and publicly available prompt-injection resources. These sources were used to understand attack patterns and develop scenario templates. The dataset was intended to contain newly synthesized scenarios rather than copied records.

| Source | Purpose in dataset design |
|---|---|
| [Microsoft LLMail-Inject](https://arxiv.org/abs/2506.09956) | Informed email-specific prompt-injection scenarios and attacks targeting email-agent behavior. |
| [AgentDojo](https://github.com/ethz-spylab/agentdojo) | Informed the design of attacks against LLM agents that interact with external tools and untrusted contextual information. |
| [deepset/prompt-injections](https://huggingface.co/datasets/deepset/prompt-injections) | Informed text-level prompt-injection patterns and contrasts between malicious and non-malicious inputs. |
| [Lakera Mosscap prompt-injection dataset](https://huggingface.co/datasets/Lakera/mosscap_prompt_injection) | Informed the diversity of adversarial prompt patterns and attempted instruction manipulation. |

### Source-use statement

These resources informed the threat model and scenario-template design. No source records were intentionally copied into the synthetic corpus, according to the dataset-generation description. The source datasets were not treated as interchangeable or as having identical labels, because their original tasks and annotation schemes differ.

## 4. Dataset Generation Process

The synthetic scenarios were constructed using the following workflow.

### Step 1: Define the scenario

A scenario was defined using the relevant email-agent context, including:

- Original user instruction.
- Sender identity and email context.
- Intended action.
- Relevant data or attachment.
- Intended recipient or destination.
- Authorization conditions.
- Expected security outcome.

### Step 2: Generate the email

An email subject and body were created to represent the scenario. Benign examples represented legitimate communication, while attack examples represented attempts to manipulate the agent into taking an unsafe or unauthorized action.

### Step 3: Assign the label

The expected label was determined from the intended security outcome and scenario context rather than solely from the presence of suspicious keywords.

### Step 4: Standardize the record

Each scenario was converted into a structured representation containing 36 columns. This standardized layout was intended to provide consistent scenario information to compatible evaluation methods.

### Step 5: Prepare evaluation splits

The dataset description specifies an intended 80:20 division between training and test data. The final split should be verified against the actual dataset files before publication.

## 5. Labels

The dataset description defines three labels.

| Label | Meaning |
|---|---|
| `BENIGN` | A legitimate email scenario that is consistent with the user's request and the applicable authorization conditions. |
| `ATTACK` | A scenario intended to manipulate the agent into unsafe behavior, such as unauthorized data disclosure or following malicious instructions embedded in an email. |
| `REVISE` | A scenario designated for correction or further review before the proposed action can be considered acceptable. |

The precise mapping of `REVISE` to binary classification targets is experiment-dependent and must be documented in the evaluation configuration. The label should not be silently merged with either `BENIGN` or `ATTACK`.

## 6. Attack Categories

The scenario design covers the following categories:

1. Direct prompt injection.
2. Indirect prompt injection.
3. Contextual manipulation.
4. Unauthorized data flow.
5. Social engineering.
6. Role impersonation.
7. Obfuscation.
8. Memory poisoning.
9. Mixed attacks.
10. Credential exfiltration.

These categories describe the intended threat patterns represented in the corpus. The presence and frequency of each category should be confirmed against the generated records before reporting category-level statistics.

## 7. Dataset Structure

The main corpus is described as having 36 structured columns. The fields include:

- Email subject and body.
- Sender and recipient details.
- Original user instruction.
- Requested action.
- Data sensitivity.
- Authorization information.
- Intended destination.
- Attack category.
- Expected decision.
- Ground-truth explanation.
- Scenario-pair identifiers.
- Attack-variant and information-flow metadata.

The feature-based experiments use approximately 99 engineered features derived from scenario information. These features represent characteristics of senders, recipients, attachments, links, email structure, addresses in the email body, and textual indicators associated with suspicious behavior.

The exact column names, data types, permitted values, and missing-value conventions should be documented from the final dataset schema.

## 8. Reported Dataset Composition

The research draft reports the following training and test allocation.

| Split | Records | Reported labels |
|---|---:|---|
| Training | 6,000 | 2,200 `BENIGN`, 2,300 `ATTACK`, 1,500 `REVISE` |
| Test | 1,500 | 500 `BENIGN`, 500 `ATTACK`, 500 `REVISE` |
| Total | 7,500 | Requires verification against the final files |

**Important:** These are the counts reported in the research draft, not independently verified counts from the dataset files. Another section of the draft reports different class totals. The definitive counts must be obtained by counting the records and labels in the actual generated dataset before this README is finalized.

The draft also describes 600 paired benign–attack scenarios. Within each pair, contextual elements are intended to remain consistent where applicable, while email content or the proposed destination changes. Whether paired records were kept in the same data split must be verified to rule out data leakage.

## 9. Intended Use

The dataset is intended for research and evaluation involving:

- Prompt-injection detection.
- Email-agent security.
- Feature-based attack classification.
- Comparison of machine-learning classifiers.
- Evaluation of false-positive and false-negative behavior.
- Analysis of model performance across different scenario categories.

The dataset should not be interpreted as proof that a model is secure against all real-world prompt-injection attacks. Synthetic scenarios may not capture the complete diversity, complexity, or distribution of real email traffic.

## 10. Reproducibility and Limitations

For reproducible experiments, the dataset release should include:

- The final dataset files and schema.
- The generation scripts or scenario templates, where available.
- The final class distribution and split sizes.
- The feature-extraction implementation.
- The train, validation, and test partitioning procedure.
- Random seeds and relevant model configurations.
- The exact versions or access dates of external source datasets.
- A description of how paired and near-duplicate scenarios were handled.

Because the corpus is synthetic, measured performance may partly reflect recurring templates or patterns introduced during generation. Evaluation on independently collected or externally constructed email scenarios would help assess generalization beyond the synthetic dataset.

## 11. References

1. Abdelnabi, S., et al. (2025). *LLMail-Inject: A Dataset from a Realistic Adaptive Prompt Injection Challenge*. arXiv:2506.09956. https://arxiv.org/abs/2506.09956

2. Debenedetti, E., et al. (2024). *AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents*. NeurIPS Datasets and Benchmarks. https://github.com/ethz-spylab/agentdojo

3. deepset. *prompt-injections*. Hugging Face Datasets. https://huggingface.co/datasets/deepset/prompt-injections

4. Lakera. *mosscap_prompt_injection*. Hugging Face Datasets. https://huggingface.co/datasets/Lakera/mosscap_prompt_injection

The dataset versions and access dates should be recorded for the final release. Bibliographic details should be verified against the original source publications and dataset cards.
