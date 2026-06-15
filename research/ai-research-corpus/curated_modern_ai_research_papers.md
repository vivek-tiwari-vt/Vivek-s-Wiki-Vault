# Curated Modern AI Research Papers and Blogs

Scope: LLMs, multimodal models, agents, architecture tweaks, prompt engineering, context engineering, harness/eval engineering, loop engineering, memory, and broadly impactful modern AI papers.

Format: Paper/blog links + category tags + one-line impact note. Blog is marked unavailable where I did not find a reliable technical blog or official explainer.

Verification note: key links were checked live where possible. Some official company pages return 403 to scripts while being valid in a browser; those are retained only when they are stable official URLs.

---

## OpenAI

- Language Models are Few-Shot Learners / GPT-3
  - Paper: https://arxiv.org/abs/2005.14165
  - Blog: https://openai.com/research/language-models-are-few-shot-learners
  - Tags: [LLM, scaling, in-context learning, prompting]
  - Impact: Made scale-driven few-shot and in-context learning a central foundation-model paradigm.

- WebGPT: Browser-assisted question-answering with human feedback
  - Paper: https://arxiv.org/abs/2112.09332
  - Blog: https://openai.com/research/webgpt
  - Tags: [agents, web browsing, RLHF, retrieval, tool use]
  - Impact: Early high-impact blueprint for citation-backed browser agents trained with human feedback.

- InstructGPT: Training language models to follow instructions with human feedback
  - Paper: https://arxiv.org/abs/2203.02155
  - Blog: https://openai.com/research/instruction-following
  - Tags: [post-training, RLHF, instruction tuning, alignment]
  - Impact: Established the RLHF/instruction-following recipe behind ChatGPT-style assistants.

- GPT-4 Technical Report
  - Paper: https://arxiv.org/abs/2303.08774
  - Blog: https://openai.com/research/gpt-4
  - Tags: [LLM, multimodal, scaling, eval]
  - Impact: Set the reference frontier-model benchmark for broad reasoning, coding, and multimodal performance.

- Improving mathematical reasoning with process supervision
  - Paper: https://arxiv.org/abs/2305.20050
  - Blog: https://openai.com/research/improving-mathematical-reasoning-with-process-supervision
  - Tags: [reasoning, supervision, eval, post-training]
  - Impact: Shifted reasoning supervision toward evaluating intermediate steps rather than only final answers.

- Function calling and structured outputs
  - Paper: unavailable
  - Blog/docs: https://platform.openai.com/docs/guides/function-calling
  - Tags: [agents, tool use, structured output, harness engineering]
  - Impact: Popularized typed tool-calling APIs as the practical interface layer for LLM agents.

---

## Anthropic

- Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback
  - Paper: https://arxiv.org/abs/2204.05862
  - Blog: unavailable
  - Tags: [RLHF, alignment, assistant training]
  - Impact: Important early Anthropic alignment work on helpful/harmless assistant post-training.

- Constitutional AI: Harmlessness from AI Feedback
  - Paper: https://arxiv.org/abs/2212.08073
  - Blog: https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback
  - Tags: [alignment, RLAIF, post-training, safety]
  - Impact: Introduced constitution-guided AI feedback as a scalable alternative/complement to human preference labeling.

- Discovering Language Model Behaviors with Model-Written Evaluations
  - Paper: https://arxiv.org/abs/2212.09251
  - Blog: unavailable
  - Tags: [eval harness, safety, scalable oversight]
  - Impact: Showed how models can help generate targeted evaluations for hidden behaviors and risks.

- Monosemanticity / Toy Models of Superposition
  - Paper/article: https://transformer-circuits.pub/2022/toy_model/index.html
  - Blog: unavailable
  - Tags: [interpretability, sparse features, safety]
  - Impact: Became a core conceptual frame for mechanistic interpretability in large models.

- Mapping the Mind of a Large Language Model
  - Paper/article: https://www.anthropic.com/research/mapping-mind-language-model
  - Blog: same
  - Tags: [interpretability, sparse autoencoders, safety]
  - Impact: Demonstrated interpretable feature maps at frontier-model scale.

- The Claude 3 Model Family: Opus, Sonnet, Haiku
  - Paper/model card: https://www.anthropic.com/news/claude-3-family
  - Blog: same
  - Tags: [LLM, multimodal, eval, safety]
  - Impact: Important public system/model-card source for Claude’s multimodal and safety-oriented deployment approach.

- Scaling Monosemanticity
  - Paper/article: https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html
  - Blog: same
  - Tags: [interpretability, sparse autoencoders, scaling]
  - Impact: Moved sparse-feature interpretability from toy systems toward production-scale models.

---

## Google Research / Google Brain

- Attention Is All You Need
  - Paper: https://arxiv.org/abs/1706.03762
  - Blog: https://research.google/blog/transformer-a-novel-neural-network-architecture-for-language-understanding/
  - Tags: [architecture, LLM, attention, scaling]
  - Impact: Introduced the Transformer architecture underlying modern LLMs and multimodal models.

- BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding
  - Paper: https://arxiv.org/abs/1810.04805
  - Blog: https://research.google/blog/open-sourcing-bert-state-of-the-art-pre-training-for-natural-language-processing/
  - Tags: [LLM, pretraining, NLP, encoders]
  - Impact: Made masked-language-model pretraining dominant for encoder NLP systems.

- T5: Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer
  - Paper: https://arxiv.org/abs/1910.10683
  - Blog: https://research.google/blog/exploring-transfer-learning-with-t5-the-text-to-text-transfer-transformer/
  - Tags: [LLM, text-to-text, transfer learning]
  - Impact: Popularized a unified text-to-text interface for broad NLP task transfer.

- GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding
  - Paper: https://arxiv.org/abs/2006.16668
  - Blog: unavailable
  - Tags: [MoE, architecture, distributed training]
  - Impact: Early large-scale sparse Transformer system that shaped later MoE scaling.

- PaLM: Scaling Language Modeling with Pathways
  - Paper: https://arxiv.org/abs/2204.02311
  - Blog: https://research.google/blog/pathways-language-model-palm-scaling-to-540-billion-parameters-for-breakthrough-performance/
  - Tags: [LLM, scaling, reasoning, code]
  - Impact: Landmark 540B dense model showing strong emergent reasoning and code abilities.

- Chain-of-Thought Prompting Elicits Reasoning in Large Language Models
  - Paper: https://arxiv.org/abs/2201.11903
  - Blog: https://research.google/blog/language-models-perform-reasoning-via-chain-of-thought/
  - Tags: [prompt engineering, reasoning, test-time compute]
  - Impact: Established step-by-step prompting as a simple, powerful LLM reasoning method.

- Self-Consistency Improves Chain of Thought Reasoning in Language Models
  - Paper: https://arxiv.org/abs/2203.11171
  - Blog: https://research.google/pubs/self-consistency-improves-chain-of-thought-reasoning-in-language-models/
  - Tags: [prompt engineering, reasoning, sampling]
  - Impact: Showed that sampling and voting over reasoning traces substantially improves reliability.

- ReAct: Synergizing Reasoning and Acting in Language Models
  - Paper: https://arxiv.org/abs/2210.03629
  - Blog: https://research.google/blog/react-synergizing-reasoning-and-acting-in-language-models/
  - Tags: [agents, prompting, tool use, loop engineering]
  - Impact: Became a canonical agent pattern interleaving reasoning with environment/tool actions.

---

## Google DeepMind / DeepMind

- Chinchilla: An Empirical Analysis of Compute-Optimal Large Language Model Training
  - Paper: https://arxiv.org/abs/2203.15556
  - Blog: https://www.deepmind.com/blog/an-empirical-analysis-of-compute-optimal-large-language-model-training
  - Tags: [scaling laws, training, data, LLM]
  - Impact: Reset scaling practice by showing many large models were undertrained relative to available data.

- RETRO: Improving Language Models by Retrieving from Trillions of Tokens
  - Paper: https://arxiv.org/abs/2112.04426
  - Blog: unavailable
  - Tags: [retrieval, memory, context engineering, architecture]
  - Impact: Influential retrieval-augmented LM architecture using external corpora as scalable memory.

- Memorizing Transformers
  - Paper: https://arxiv.org/abs/2203.08913
  - Blog: unavailable
  - Tags: [memory, architecture, retrieval, long context]
  - Impact: Added kNN-style memory to Transformers for better long-range memorization.

- Flamingo: A Visual Language Model for Few-Shot Learning
  - Paper: https://arxiv.org/abs/2204.14198
  - Blog: https://www.deepmind.com/blog/tackling-multiple-tasks-with-a-single-visual-language-model
  - Tags: [multimodal, VLM, few-shot, architecture]
  - Impact: Major early few-shot vision-language model combining frozen language and vision modules.

- Gato: A Generalist Agent
  - Paper: https://arxiv.org/abs/2205.06175
  - Blog: https://www.deepmind.com/blog/a-generalist-agent
  - Tags: [agents, multimodal, robotics, generalist models]
  - Impact: Demonstrated a single Transformer policy across text, games, vision, and robotic control.

- AlphaCode
  - Paper: https://arxiv.org/abs/2203.07814
  - Blog: https://deepmind.google/discover/blog/competitive-programming-with-alphacode/
  - Tags: [code LLM, sampling, eval, agents]
  - Impact: Showed LLM code generation could reach competitive-programming performance with sampling/filtering.

- Gemini: A Family of Highly Capable Multimodal Models
  - Paper: https://arxiv.org/abs/2312.11805
  - Blog: https://blog.google/technology/ai/google-gemini-ai/
  - Tags: [multimodal, LLM, reasoning, code]
  - Impact: Flagship natively multimodal model family across text, image, audio, video, and code.

- Gemini 1.5: Multimodal Understanding across Millions of Tokens
  - Paper: https://arxiv.org/abs/2403.05530
  - Blog: https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/
  - Tags: [long context, context engineering, multimodal, MoE]
  - Impact: Pushed practical million-token context and long-context multimodal reasoning.

- RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control
  - Paper: https://arxiv.org/abs/2307.15818
  - Blog: https://deepmind.google/discover/blog/rt-2-new-model-translates-vision-and-language-into-action/
  - Tags: [agents, robotics, multimodal, action grounding]
  - Impact: Connected web-scale VLM pretraining to robotic action generation.

- SIMA: Scalable Instructable Multiworld Agent
  - Paper: https://arxiv.org/abs/2404.10179
  - Blog: https://deepmind.google/discover/blog/sima-generalist-ai-agent-for-3d-virtual-environments/
  - Tags: [agents, multimodal, environments, instruction following]
  - Impact: Advanced language-instructed agents operating across multiple 3D game worlds.

---

## Meta AI / FAIR

- LLaMA: Open and Efficient Foundation Language Models
  - Paper: https://arxiv.org/abs/2302.13971
  - Blog: https://ai.meta.com/blog/large-language-model-llama-meta-ai/
  - Tags: [LLM, open weights, training recipe]
  - Impact: Catalyzed the open-weight LLM ecosystem and downstream fine-tuning explosion.

- Llama 2
  - Paper: https://arxiv.org/abs/2307.09288
  - Blog: https://ai.meta.com/llama/
  - Tags: [LLM, open weights, alignment, safety]
  - Impact: Became a widely used open baseline for commercial and research LLM systems.

- Toolformer: Language Models Can Teach Themselves to Use Tools
  - Paper: https://arxiv.org/abs/2302.04761
  - Blog: unavailable
  - Tags: [agents, tool use, self-supervision]
  - Impact: Influential self-supervised approach for teaching LMs API/tool use.

- ImageBind: One Embedding Space to Bind Them All
  - Paper: https://arxiv.org/abs/2305.05665
  - Blog: https://ai.meta.com/blog/imagebind-six-modalities-binding-ai/
  - Tags: [multimodal, embeddings, representation learning]
  - Impact: Showed six modalities could be aligned into one shared embedding space.

- Segment Anything
  - Paper: https://arxiv.org/abs/2304.02643
  - Blog: https://ai.meta.com/blog/segment-anything-foundation-model-image-segmentation/
  - Tags: [multimodal, vision foundation model, prompting]
  - Impact: Made promptable image segmentation a foundation-model capability with broad downstream use.

- JEPA / I-JEPA
  - Paper: https://arxiv.org/abs/2301.08243
  - Blog: https://ai.meta.com/blog/yann-lecun-ai-model-i-jepa/
  - Tags: [architecture, self-supervised learning, world models]
  - Impact: Advanced non-generative predictive representation learning as an alternative foundation-model direction.

---

## Microsoft Research / Microsoft AI

- DeepSpeed / ZeRO
  - Paper: https://arxiv.org/abs/1910.02054
  - Blog: https://www.microsoft.com/en-us/research/project/deepspeed/
  - Tags: [training systems, scaling, harness engineering]
  - Impact: Core distributed training infrastructure enabling large-model training at scale.

- Orca: Progressive Learning from Complex Explanation Traces of GPT-4
  - Paper: https://arxiv.org/abs/2306.02707
  - Blog: unavailable
  - Tags: [distillation, reasoning, post-training]
  - Impact: Popularized explanation-trace distillation from frontier models into smaller models.

- AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation
  - Paper: https://arxiv.org/abs/2308.08155
  - Blog/project: https://www.microsoft.com/en-us/research/project/autogen/
  - Tags: [agents, multi-agent, loop engineering, framework]
  - Impact: Became a prominent practical framework for multi-agent LLM orchestration.

- Guidance
  - Paper: https://arxiv.org/abs/2307.09702
  - Blog/GitHub: https://github.com/guidance-ai/guidance
  - Tags: [prompt programming, structured generation, harness engineering]
  - Impact: Influenced constrained/structured LLM generation and prompt-as-program workflows.

- Phi-2 / Phi small language models
  - Paper/blog: https://www.microsoft.com/en-us/research/blog/phi-2-the-surprising-power-of-small-language-models/
  - Blog: same
  - Tags: [small LLM, data quality, training]
  - Impact: Strengthened the case that curated data can produce capable small models.

---

## NVIDIA Research

- Megatron-LM
  - Paper: https://arxiv.org/abs/1909.08053
  - Blog: https://developer.nvidia.com/blog/scaling-language-model-training-to-a-trillion-parameters-using-megatron/
  - Tags: [training systems, tensor parallelism, LLM scaling]
  - Impact: Foundational large-scale Transformer training system for tensor/model parallelism.

- FlashAttention
  - Paper: https://arxiv.org/abs/2205.14135
  - Blog: https://developer.nvidia.com/blog/accelerating-large-language-models-with-flashattention/
  - Tags: [architecture/system tweak, attention, inference, training]
  - Impact: Made exact attention far more IO-efficient, becoming a standard LLM kernel family.

- NeMo / Megatron-Core
  - Paper/docs: https://docs.nvidia.com/nemo-framework/user-guide/latest/overview.html
  - Blog: https://developer.nvidia.com/blog/nvidia-nemo-framework-training/
  - Tags: [training harness, LLM systems, deployment]
  - Impact: Major industrial stack for training and adapting large language/multimodal models.

- Nemotron-4 340B
  - Paper: https://arxiv.org/abs/2406.11704
  - Blog: unavailable
  - Tags: [LLM, synthetic data, reward models, alignment]
  - Impact: Strong open model/reward-model release emphasizing synthetic-data generation and alignment tooling.

---

## Apple Machine Learning Research

- Ferret: Refer and Ground Anything Anywhere at Any Granularity
  - Paper: https://arxiv.org/abs/2310.07704
  - Blog: https://machinelearning.apple.com/research/ferret
  - Tags: [multimodal, VLM, grounding, referring]
  - Impact: Notable open multimodal grounding model from Apple ML research.

- LLM in a Flash: Efficient Large Language Model Inference with Limited Memory
  - Paper: https://arxiv.org/abs/2312.11514
  - Blog: https://machinelearning.apple.com/research/llm-in-a-flash
  - Tags: [inference, memory, on-device LLMs]
  - Impact: Important on-device inference work for running models under tight memory constraints.

- Apple Intelligence Foundation Language Models
  - Paper: https://arxiv.org/abs/2407.21075
  - Blog: https://machinelearning.apple.com/research/apple-intelligence-foundation-language-models
  - Tags: [LLM, on-device, private cloud, deployment]
  - Impact: Technical source for Apple’s on-device/server foundation-model architecture and evaluation.

---

## Amazon Science

- AlexaTM 20B: Few-Shot Learning Using a Large-Scale Multilingual Seq2Seq Model
  - Paper: https://arxiv.org/abs/2208.01448
  - Blog: https://www.amazon.science/blog/alexatm-20b-parameter-model-sets-new-marks-in-few-shot-learning
  - Tags: [LLM, multilingual, seq2seq, few-shot]
  - Impact: Important Amazon multilingual LLM showing strong few-shot seq2seq behavior.

- Chain-of-Thought Hub / language model reasoning work
  - Paper: https://arxiv.org/abs/2305.16582
  - Blog: unavailable
  - Tags: [reasoning, eval, prompt engineering]
  - Impact: Contributed to systematic reasoning evaluation and prompting practice.

---

## Cohere / Cohere for AI

- Aya: An Open Science Initiative for Multilingual Generative Language Models
  - Paper: https://arxiv.org/abs/2402.07827
  - Blog: https://cohere.com/research/aya
  - Tags: [multilingual LLM, open science, data]
  - Impact: Major open multilingual instruction-tuning effort across many languages.

- Command R / R+ retrieval-augmented models
  - Paper/model card: https://huggingface.co/CohereForAI/c4ai-command-r-plus
  - Blog: https://cohere.com/blog/command-r-plus-microsoft-azure
  - Tags: [RAG, tool use, enterprise LLM, context engineering]
  - Impact: Practical model line optimized for retrieval-augmented and tool-using enterprise workflows.

---

## Databricks / MosaicML

- MPT-7B
  - Paper/model: https://www.mosaicml.com/blog/mpt-7b
  - Blog: same
  - Tags: [LLM, open model, training recipe]
  - Impact: Early commercially usable open LLM showing transparent training and long-context variants.

- DBRX
  - Paper/blog: https://www.databricks.com/blog/introducing-dbrx-new-state-art-open-llm
  - Blog: same
  - Tags: [LLM, MoE, open model, training systems]
  - Impact: High-profile open MoE model release from Databricks/Mosaic.

---

## Salesforce AI Research

- BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models
  - Paper: https://arxiv.org/abs/2301.12597
  - Blog: unavailable
  - Tags: [multimodal, VLM, architecture, instruction tuning]
  - Impact: Influential VLM architecture connecting frozen vision encoders to frozen LLMs through a query transformer.

- LAVIS
  - Paper: https://arxiv.org/abs/2209.09019
  - Blog/GitHub: https://github.com/salesforce/LAVIS
  - Tags: [multimodal, harness, VLM toolkit]
  - Impact: Widely used library for language-vision research and evaluation.

---

## Allen Institute for AI / AI2

- ELMo: Deep contextualized word representations
  - Paper: https://arxiv.org/abs/1802.05365
  - Blog: https://allenai.org/blog/elmo
  - Tags: [pretraining, contextual embeddings, NLP]
  - Impact: Pre-Transformer but highly influential contextual representation work leading into modern pretraining.

- OLMo: Accelerating the Science of Language Models
  - Paper: https://arxiv.org/abs/2402.00838
  - Blog: https://allenai.org/olmo
  - Tags: [open LLM, data, reproducibility, training]
  - Impact: One of the most transparent fully open LLM training stacks.

- Tulu 2
  - Paper: https://arxiv.org/abs/2311.10702
  - Blog: https://allenai.org/blog/tulu-2
  - Tags: [instruction tuning, alignment, open LLM]
  - Impact: Practical open post-training recipe for aligned instruction-following models.

---

## Stability AI / Runway / Generative Media

- High-Resolution Image Synthesis with Latent Diffusion Models / Stable Diffusion basis
  - Paper: https://arxiv.org/abs/2112.10752
  - Blog: https://stability.ai/news/stable-diffusion-public-release
  - Tags: [multimodal, diffusion, generative models]
  - Impact: Enabled the open text-to-image model ecosystem and modern diffusion workflows.

- Stable Video Diffusion
  - Paper: https://arxiv.org/abs/2311.15127
  - Blog: https://stability.ai/news/stable-video-diffusion-open-ai-video-model
  - Tags: [multimodal, video generation, diffusion]
  - Impact: Important open video-generation model release from the diffusion ecosystem.

---

## Together AI / Nous Research / Open Collectives

- RedPajama
  - Paper/blog: https://www.together.ai/blog/redpajama
  - Blog: same
  - Tags: [data engineering, open LLM, reproducibility]
  - Impact: Helped reproduce LLaMA-style pretraining data openly and accelerated open LLM work.

- OpenChatKit
  - Paper/blog: https://www.together.ai/blog/openchatkit
  - Blog: same
  - Tags: [open assistant, instruction tuning]
  - Impact: Early open-source chat assistant package following the ChatGPT wave.

- Hermes models / Nous Research
  - Paper/model cards: https://huggingface.co/NousResearch
  - Blog: https://nousresearch.com/
  - Tags: [open LLM, instruction tuning, tool use, alignment]
  - Impact: Influential open model family and alignment/data recipes in the community LLM ecosystem.

---

## Stanford

- HELM: Holistic Evaluation of Language Models
  - Paper: https://arxiv.org/abs/2211.09110
  - Blog/benchmark: https://crfm.stanford.edu/helm/latest/
  - Tags: [eval harness, benchmark, model governance]
  - Impact: Established a broad multi-metric evaluation framework for foundation models.

- Alpaca
  - Paper/blog: https://crfm.stanford.edu/2023/03/13/alpaca.html
  - Blog: same
  - Tags: [instruction tuning, open LLM, data generation]
  - Impact: Showed low-cost instruction-following adaptation of LLaMA and sparked many open fine-tunes.

- DSPy
  - Paper: https://arxiv.org/abs/2310.03714
  - Blog/docs: https://dspy.ai/
  - Tags: [prompt programming, context engineering, harness engineering, optimization]
  - Impact: Reframed prompting/RAG pipelines as optimizable programs rather than hand-written prompts.

- STORM: Writing Wikipedia-like Articles from Scratch with LLMs
  - Paper: https://arxiv.org/abs/2402.14207
  - Blog/GitHub: https://github.com/stanford-oval/storm
  - Tags: [agents, RAG, planning, writing loops]
  - Impact: Influential agentic research-writing workflow using retrieval, outline planning, and multi-perspective synthesis.

- Generative Agents: Interactive Simulacra of Human Behavior
  - Paper: https://arxiv.org/abs/2304.03442
  - Blog: https://hai.stanford.edu/news/computational-agents-exhibit-believable-humanlike-behavior
  - Tags: [agents, memory, simulation, planning]
  - Impact: Canonical paper for memory-enabled believable LLM agents.

---

## UC Berkeley / LMSYS / BAIR

- Vicuna
  - Paper/blog: https://lmsys.org/blog/2023-03-30-vicuna/
  - Blog: same
  - Tags: [open LLM, instruction tuning, eval]
  - Impact: Early high-profile open ChatGPT-style model and comparative evaluation release.

- Chatbot Arena / LMSYS Arena
  - Paper: https://arxiv.org/abs/2403.04132
  - Blog/leaderboard: https://lmarena.ai/
  - Tags: [eval harness, human preference, leaderboard]
  - Impact: Became a central public human-preference leaderboard for LLMs.

- Gorilla: Large Language Model Connected with Massive APIs
  - Paper: https://arxiv.org/abs/2305.15334
  - Blog/GitHub: https://github.com/ShishirPatil/gorilla
  - Tags: [agents, tool use, API calling]
  - Impact: Important early work on reliable API selection and tool invocation.

- LLaVA: Large Language and Vision Assistant
  - Paper: https://arxiv.org/abs/2304.08485
  - Blog/GitHub: https://llava-vl.github.io/
  - Tags: [multimodal, VLM, instruction tuning]
  - Impact: Highly influential open recipe for visual instruction tuning.

- Direct Preference Optimization
  - Paper: https://arxiv.org/abs/2305.18290
  - Blog: unavailable
  - Tags: [alignment, post-training, preference optimization]
  - Impact: Simplified preference optimization by avoiding explicit RL training loops.

---

## Cornell

- Reflexion: Language Agents with Verbal Reinforcement Learning
  - Paper: https://arxiv.org/abs/2303.11366
  - Blog/GitHub: https://github.com/noahshinn024/reflexion
  - Tags: [agents, loop engineering, memory, self-improvement]
  - Impact: Canonical reflective agent loop using verbal feedback/memory to improve future trials.

- Self-Refine: Iterative Refinement with Self-Feedback
  - Paper: https://arxiv.org/abs/2303.17651
  - Blog/GitHub: https://selfrefine.info/
  - Tags: [loop engineering, self-feedback, prompting]
  - Impact: Popularized generate-critique-revise loops for improving LLM outputs without extra training.

- ToolLLM / ToolBench collaborators where applicable
  - Paper: https://arxiv.org/abs/2307.16789
  - Blog/GitHub: https://github.com/OpenBMB/ToolBench
  - Tags: [agents, tool use, eval harness]
  - Impact: Canonical dataset/benchmark for API-tool-using LLM agents.

---

## Harvard

- The Platonic Representation Hypothesis
  - Paper: https://arxiv.org/abs/2405.07987
  - Blog: unavailable
  - Tags: [representation learning, multimodal, foundation models]
  - Impact: Influential thesis that representations across modalities/models converge as scale and data grow.

- Sparse Autoencoders / interpretability work from Harvard-linked researchers
  - Paper: https://arxiv.org/abs/2406.04093
  - Blog: unavailable
  - Tags: [interpretability, sparse features, safety]
  - Impact: Contributes to the fast-moving sparse-feature interpretability literature relevant to frontier LLMs.

---

## MIT

- The Lottery Ticket Hypothesis
  - Paper: https://arxiv.org/abs/1803.03635
  - Blog: unavailable
  - Tags: [architecture, pruning, efficiency]
  - Impact: Foundational pruning/sparsity result influencing efficient-model thinking.

- Prompting / program synthesis / code-model work
  - Paper: https://arxiv.org/abs/2107.03374
  - Blog: unavailable
  - Tags: [code LLM, program synthesis, prompting]
  - Impact: Contributed to modern neural program-synthesis and code-generation evaluation traditions.

---

## CMU

- SWE-agent
  - Paper: https://arxiv.org/abs/2405.15793
  - Blog/GitHub: https://github.com/SWE-agent/SWE-agent
  - Tags: [code agents, loop engineering, software engineering, tool use]
  - Impact: High-impact software-engineering agent architecture for solving real GitHub issues.

- Visual ChatGPT
  - Paper: https://arxiv.org/abs/2303.04671
  - Blog/GitHub: https://github.com/microsoft/visual-chatgpt
  - Tags: [multimodal agents, tool use, model orchestration]
  - Impact: Early example of using ChatGPT as a controller over visual foundation models.

---

## Princeton

- SWE-bench: Can Language Models Resolve Real-World GitHub Issues?
  - Paper: https://arxiv.org/abs/2310.06770
  - Blog/benchmark: https://www.swebench.com/
  - Tags: [agent eval, code agents, harness engineering]
  - Impact: Became the central benchmark for real-world software-engineering agents.

- Tree of Thoughts
  - Paper: https://arxiv.org/abs/2305.10601
  - Blog: unavailable
  - Tags: [reasoning, prompt engineering, search, loop engineering]
  - Impact: Framed LLM reasoning as explicit search over candidate thought trajectories.

---

## University of Washington / AllenNLP ecosystem

- Self-Instruct
  - Paper: https://arxiv.org/abs/2212.10560
  - Blog/GitHub: https://github.com/yizhongw/self-instruct
  - Tags: [instruction tuning, synthetic data, alignment]
  - Impact: Established a practical synthetic-instruction generation recipe for open instruction tuning.

- ToolQA
  - Paper: https://arxiv.org/abs/2306.13304
  - Blog/GitHub: https://github.com/night-chen/ToolQA
  - Tags: [tool use, eval, agents]
  - Impact: Evaluates whether LLMs can use external tools for question answering.

---

## NYU

- Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks
  - Paper: https://arxiv.org/abs/2005.11401
  - Blog: unavailable
  - Tags: [RAG, retrieval, memory, context engineering]
  - Impact: Canonical RAG paper combining parametric and non-parametric memory for generation.

- Mamba: Linear-Time Sequence Modeling with Selective State Spaces
  - Paper: https://arxiv.org/abs/2312.00752
  - Blog/GitHub: https://github.com/state-spaces/mamba
  - Tags: [architecture, state-space models, long context]
  - Impact: Renewed interest in non-attention sequence architectures for efficient long-context modeling.

---

## UIUC

- Voyager: An Open-Ended Embodied Agent with Large Language Models
  - Paper: https://arxiv.org/abs/2305.16291
  - Blog/GitHub: https://voyager.minedojo.org/
  - Tags: [agents, memory, skill library, loop engineering]
  - Impact: Canonical open-ended agent that builds and reuses a skill library in Minecraft.

- AgentBench
  - Paper: https://arxiv.org/abs/2308.03688
  - Blog/GitHub: https://github.com/THUDM/AgentBench
  - Tags: [agent eval, harness engineering]
  - Impact: Broad benchmark for evaluating LLMs as interactive agents across environments.

---

## DeepSeek

- DeepSeek-V2
  - Paper: https://arxiv.org/abs/2405.04434
  - Blog: unavailable
  - Tags: [LLM, MoE, MLA, architecture, efficiency]
  - Impact: Introduced the efficiency stack that later underpinned DeepSeek-V3/R1.

- DeepSeek-V3 Technical Report
  - Paper: https://arxiv.org/abs/2412.19437
  - Blog: https://api-docs.deepseek.com/news/news1226
  - Tags: [LLM, MoE, architecture, training, efficiency]
  - Impact: Made MLA, DeepSeekMoE, and FP8 training a reference design for lower-cost frontier-scale LLMs.

- DeepSeek-R1
  - Paper: https://arxiv.org/abs/2501.12948
  - Blog: https://api-docs.deepseek.com/news/news250120
  - Tags: [reasoning, RL, loop engineering, eval]
  - Impact: Popularized open reasoning-RL pipelines and long-CoT distillation.

- DeepSeek-Coder
  - Paper: https://arxiv.org/abs/2401.14196
  - Blog/GitHub: https://github.com/deepseek-ai/DeepSeek-Coder
  - Tags: [code LLM, data engineering, eval]
  - Impact: One of the strongest open code-specialist model families before the reasoning-model wave.

- Janus-Pro
  - Paper: https://arxiv.org/abs/2501.17811
  - Blog/GitHub: https://github.com/deepseek-ai/Janus
  - Tags: [multimodal, image generation, unified VLM]
  - Impact: Strong open unified multimodal model separating visual understanding and generation pathways.

---

## Moonshot AI / Kimi

- Kimi k1.5
  - Paper: https://github.com/MoonshotAI/Kimi-k1.5/blob/main/Kimi_k1.5.pdf
  - Blog/GitHub: https://github.com/MoonshotAI/Kimi-k1.5
  - Tags: [reasoning, RL, long context, multimodal, eval]
  - Impact: High-profile long-context reasoning model emphasizing RL scaling with long-CoT.

- Kimi-VL Technical Report
  - Paper: https://arxiv.org/abs/2504.07491
  - Blog: unavailable
  - Tags: [multimodal, VLM, long context]
  - Impact: Extended Kimi’s long-context strengths into vision-language reasoning.

- Mooncake: KVCache-centric Disaggregated Architecture for LLM Serving
  - Paper: https://arxiv.org/abs/2407.00079
  - Blog: unavailable
  - Tags: [serving, context engineering, KV cache, systems]
  - Impact: Influential production architecture for high-throughput long-context LLM serving.

---

## MiniMax

- MiniMax-01: Scaling Foundation Models with Lightning Attention
  - Paper: https://arxiv.org/abs/2501.08313
  - Blog/GitHub: https://github.com/MiniMax-AI/MiniMax-01
  - Tags: [LLM, long context, architecture, linear attention]
  - Impact: Demonstrated a large-scale hybrid/linear-attention model targeting million-token context.

- MiniMax-M1
  - Paper: https://arxiv.org/abs/2506.13585
  - Blog/GitHub: https://github.com/MiniMax-AI/MiniMax-M1
  - Tags: [reasoning, test-time compute, long context, RL]
  - Impact: Connected efficient attention with long-horizon reasoning and test-time scaling.

---

## Alibaba / Qwen / DAMO / ModelScope

- Qwen2.5 Technical Report
  - Paper: https://arxiv.org/abs/2412.15115
  - Blog: https://qwenlm.github.io/blog/qwen2.5/
  - Tags: [LLM, multilingual, code, math, open models]
  - Impact: Became one of the most widely used open model families for instruction, code, and math.

- Qwen2.5-Coder
  - Paper: https://arxiv.org/abs/2409.12186
  - Blog: https://qwenlm.github.io/blog/qwen2.5-coder/
  - Tags: [code LLM, agents, tool use, eval]
  - Impact: Strong open baseline for coding agents and repo-level programming workflows.

- Qwen2.5-VL
  - Paper: https://arxiv.org/abs/2502.13923
  - Blog: https://qwenlm.github.io/blog/qwen2.5-vl/
  - Tags: [multimodal, VLM, document AI, GUI agents]
  - Impact: High-impact open VLM for OCR, visual grounding, video, documents, and UI understanding.

- Qwen3
  - Paper/model: https://github.com/QwenLM/Qwen3
  - Blog: https://qwenlm.github.io/blog/qwen3/
  - Tags: [LLM, reasoning, MoE, hybrid thinking]
  - Impact: Mainline Qwen reasoning-capable model family with broad open ecosystem adoption.

- ModelScope-Agent
  - Paper: https://arxiv.org/abs/2309.00986
  - Blog/GitHub: https://github.com/modelscope/modelscope-agent
  - Tags: [agents, tool use, agent framework]
  - Impact: Early practical open-source agent framework from Alibaba/ModelScope.

---

## Tencent / Hunyuan

- Hunyuan-Large
  - Paper: https://arxiv.org/abs/2411.02265
  - Blog/GitHub: https://github.com/Tencent-Hunyuan/Tencent-Hunyuan-Large
  - Tags: [LLM, MoE, open model, training]
  - Impact: Tencent’s major open MoE LLM release.

- Hunyuan-TurboS
  - Paper: https://arxiv.org/abs/2505.15431
  - Blog: unavailable
  - Tags: [LLM, Mamba, Transformer, adaptive CoT]
  - Impact: Explores Mamba-Transformer hybridization plus adaptive chain-of-thought for lower-latency reasoning.

- HunyuanVideo
  - Paper/GitHub: https://github.com/Tencent-Hunyuan/HunyuanVideo
  - Blog: unavailable
  - Tags: [multimodal, video generation, diffusion-transformer]
  - Impact: One of the strongest open Chinese video-generation model releases.

---

## Baidu / ERNIE

- ERNIE 3.0
  - Paper: https://arxiv.org/abs/2107.02137
  - Blog: unavailable
  - Tags: [LLM, pretraining, knowledge]
  - Impact: Influential Chinese knowledge-enhanced pretraining architecture feeding later ERNIE systems.

- ERNIE 3.0 Titan
  - Paper: https://arxiv.org/abs/2112.12731
  - Blog: unavailable
  - Tags: [LLM, knowledge-enhanced pretraining]
  - Impact: Important pre-ChatGPT Chinese large-scale knowledge-enhanced LLM line.

---

## Huawei Noah / PanGu

- PanGu-alpha
  - Paper: https://arxiv.org/abs/2104.12369
  - Blog: unavailable
  - Tags: [Chinese LLM, distributed training, scaling]
  - Impact: Landmark early Chinese autoregressive LLM trained with large-scale auto-parallel computation.

- PanGu-Sigma
  - Paper: https://arxiv.org/abs/2303.10845
  - Blog: unavailable
  - Tags: [LLM, sparse MoE, trillion-parameter systems]
  - Impact: Early trillion-parameter sparse Chinese LLM showing heterogeneous sparse-compute scaling.

---

## ByteDance Seed / Doubao

- Seed-Coder
  - Paper: https://arxiv.org/abs/2506.03524
  - Blog/GitHub: https://github.com/ByteDance-Seed/Seed-Coder
  - Tags: [code LLM, data curation, self-improvement]
  - Impact: Strong example of model-driven data curation loops for code-model scaling.

- UI-TARS
  - Paper: https://arxiv.org/abs/2501.12326
  - Blog/GitHub: https://github.com/bytedance/UI-TARS
  - Tags: [GUI agents, multimodal agents, loop engineering]
  - Impact: Important open GUI-agent work for computer-use and app/web automation.

- Seed1.5-VL
  - Paper: https://arxiv.org/abs/2505.07062
  - Blog: unavailable
  - Tags: [multimodal, VLM, reasoning]
  - Impact: ByteDance Seed’s major VLM technical report for general visual reasoning.

---

## Zhipu AI / GLM / THUDM

- GLM-130B
  - Paper: https://arxiv.org/abs/2210.02414
  - Blog/GitHub: https://github.com/THUDM/GLM-130B
  - Tags: [LLM, bilingual, pretraining]
  - Impact: Landmark open bilingual 130B model and precursor to ChatGLM/GLM-4.

- ChatGLM / GLM-4 All Tools
  - Paper: https://arxiv.org/abs/2406.12793
  - Blog/GitHub: https://github.com/zai-org/GLM-4
  - Tags: [LLM, agents, tool use, all-tools]
  - Impact: Documents the ChatGLM-to-GLM-4 progression and integrated tool-use capabilities.

- CogVLM
  - Paper: https://arxiv.org/abs/2311.03079
  - Blog/GitHub: https://github.com/THUDM/CogVLM
  - Tags: [multimodal, VLM, visual expert]
  - Impact: Influential open VLM using a visual-expert design.

- CogAgent
  - Paper: https://arxiv.org/abs/2312.08914
  - Blog/GitHub: https://github.com/THUDM/CogVLM
  - Tags: [GUI agents, multimodal agents, planning]
  - Impact: Early strong visual-language model specialized for GUI interaction and grounding.

---

## Shanghai AI Lab / InternLM / OpenCompass

- InternLM2
  - Paper: https://arxiv.org/abs/2403.17297
  - Blog/GitHub: https://github.com/InternLM/InternLM
  - Tags: [LLM, code, math, agents, open models]
  - Impact: Strong open LLM family with broad tooling via LMDeploy, XTuner, and OpenCompass.

- InternVL
  - Paper/GitHub: https://github.com/OpenGVLab/InternVL
  - Blog: unavailable
  - Tags: [multimodal, VLM, vision-language scaling]
  - Impact: Major open VLM line used widely for multimodal benchmarks and downstream systems.

- OpenCompass
  - Paper/GitHub: https://github.com/open-compass/opencompass
  - Blog: unavailable
  - Tags: [harness engineering, eval, benchmarking]
  - Impact: One of the most widely used open LLM/VLM evaluation harnesses from China.

---

## BAAI / FlagOpen

- BGE / C-Pack
  - Paper: https://arxiv.org/abs/2309.07597
  - Blog/GitHub: https://github.com/FlagOpen/FlagEmbedding
  - Tags: [retrieval, embeddings, RAG, context engineering]
  - Impact: BGE became a default open embedding/reranker stack for RAG systems.

- BGE-M3
  - Paper: https://arxiv.org/abs/2407.19669
  - Blog/GitHub: https://github.com/FlagOpen/FlagEmbedding
  - Tags: [retrieval, long context, multilingual, RAG]
  - Impact: Advanced multilingual and long-context retrieval for production RAG pipelines.

---

## Tsinghua University

- ToolLLM / ToolBench
  - Paper: https://arxiv.org/abs/2307.16789
  - Blog/GitHub: https://github.com/OpenBMB/ToolBench
  - Tags: [agents, tool use, eval harness]
  - Impact: Canonical dataset/benchmark for API-tool-using LLM agents.

- AgentBench
  - Paper: https://arxiv.org/abs/2308.03688
  - Blog/GitHub: https://github.com/THUDM/AgentBench
  - Tags: [agent eval, harness engineering, environments]
  - Impact: Early broad benchmark for evaluating LLMs as interactive agents.

- AgentVerse
  - Paper: https://arxiv.org/abs/2308.10848
  - Blog/GitHub: https://github.com/OpenBMB/AgentVerse
  - Tags: [multi-agent, simulation, collaboration]
  - Impact: Influential framework for studying multi-agent collaboration and emergent behavior.

- ChatDev
  - Paper: https://arxiv.org/abs/2307.07924
  - Blog/GitHub: https://github.com/OpenBMB/ChatDev
  - Tags: [software agents, multi-agent, loop engineering]
  - Impact: Popularized role-based multi-agent software-development workflows.

- MiniCPM
  - Paper: https://arxiv.org/abs/2404.06395
  - Blog/GitHub: https://github.com/OpenBMB/MiniCPM
  - Tags: [small LLM, efficient training, edge AI]
  - Impact: Strong small-model line showing careful data/training can rival larger baselines.

---

## Peking University

- CMMLU
  - Paper: https://arxiv.org/abs/2306.09212
  - Blog/GitHub: https://github.com/haonan-li/CMMLU
  - Tags: [Chinese eval, harness engineering, benchmark]
  - Impact: Standard Chinese multitask benchmark for measuring knowledge and reasoning.

- Open-Sora-Plan
  - Paper/GitHub: https://github.com/PKU-YuanGroup/Open-Sora-Plan
  - Blog: unavailable
  - Tags: [multimodal, video generation, open replication]
  - Impact: Important open Chinese effort to reproduce Sora-like video generation.

---

## Shanghai Jiao Tong University

- MME
  - Paper: https://arxiv.org/abs/2306.13394
  - Blog/GitHub: https://github.com/BradyFU/Awesome-Multimodal-Large-Language-Models/tree/Evaluation
  - Tags: [multimodal eval, VLM benchmark]
  - Impact: Widely cited early benchmark for perception/cognition evaluation in MLLMs.

- MM-Vet
  - Paper: https://arxiv.org/abs/2308.02490
  - Blog/GitHub: https://github.com/yuweihao/MM-Vet
  - Tags: [multimodal eval, integrated reasoning]
  - Impact: Important benchmark for integrated multimodal reasoning beyond isolated visual skills.

---

## Fudan University

- MOSS
  - Paper/model: https://github.com/OpenMOSS/MOSS
  - Blog: unavailable
  - Tags: [LLM, Chinese assistant, open model]
  - Impact: Early open Chinese ChatGPT-like assistant effort.

- AgentScope
  - Paper: https://arxiv.org/abs/2402.14034
  - Blog/GitHub: https://github.com/modelscope/agentscope
  - Tags: [multi-agent, agent framework, systems]
  - Impact: Practical multi-agent orchestration framework for robust agent applications.

- SuperCLUE
  - Paper: https://arxiv.org/abs/2307.15020
  - Blog: unavailable
  - Tags: [Chinese eval, benchmark, alignment]
  - Impact: Widely referenced Chinese LLM benchmark suite.

---

## Zhejiang University

- StructGPT
  - Paper: https://arxiv.org/abs/2305.09645
  - Blog: unavailable
  - Tags: [structured data reasoning, prompting, agents]
  - Impact: Influential framework for making LLMs reason over tables, graphs, and structured databases.

- HuggingGPT
  - Paper: https://arxiv.org/abs/2303.17580
  - Blog/GitHub: https://github.com/microsoft/JARVIS
  - Tags: [agents, tool use, model orchestration]
  - Impact: Canonical “LLM as controller over expert models” agent architecture with Zhejiang-affiliated contributors.

---

## HIT / HKUST / CUHK / CASIA / HKU / Renmin

- C-Eval
  - Paper: https://arxiv.org/abs/2305.08322
  - Blog/GitHub: https://github.com/hkust-nlp/ceval
  - Tags: [Chinese eval, benchmark, harness engineering]
  - Impact: Default Chinese exam-style evaluation benchmark for foundation models.

- LongBench
  - Paper: https://arxiv.org/abs/2308.14508
  - Blog/GitHub: https://github.com/THUDM/LongBench
  - Tags: [long context, eval, context engineering]
  - Impact: Canonical bilingual benchmark for long-context understanding and context-window stress testing.

- CAMEL
  - Paper: https://arxiv.org/abs/2303.17760
  - Blog/GitHub: https://github.com/camel-ai/camel
  - Tags: [multi-agent, role-playing agents, simulation]
  - Impact: One of the earliest influential open multi-agent role-playing frameworks.

- LLaMA-Factory
  - Paper: https://arxiv.org/abs/2403.13372
  - Blog/GitHub: https://github.com/hiyouga/LLaMA-Factory
  - Tags: [fine-tuning, training harness, alignment]
  - Impact: De facto practical fine-tuning/alignment harness for open LLMs.

- RecAgent
  - Paper/GitHub: https://github.com/RUCAIBox/RecAgent
  - Blog: unavailable
  - Tags: [agents, recommender simulation, memory]
  - Impact: Representative Renmin/RUCAI agent work for simulating recommendation environments and user behavior.

---

## Oxford / Cambridge / UCL / UK-linked work

- Universal Transformers
  - Paper: https://arxiv.org/abs/1807.03819
  - Blog: unavailable
  - Tags: [architecture, recurrence, adaptive computation, Transformers]
  - Impact: Added recurrence and adaptive computation to Transformers, influential for iterative reasoning variants.

- Neural Turing Machines
  - Paper: https://arxiv.org/abs/1410.5401
  - Blog: unavailable
  - Tags: [memory, differentiable computing, architecture]
  - Impact: Foundational differentiable external-memory architecture relevant to memory-augmented models.

- Differentiable Neural Computer
  - Paper: https://www.nature.com/articles/nature20101
  - Blog: unavailable
  - Tags: [memory, architecture, differentiable computers]
  - Impact: Extended neural memory systems with structured read/write mechanisms for algorithmic reasoning.

- One-shot Learning with Memory-Augmented Neural Networks
  - Paper: https://arxiv.org/abs/1605.06065
  - Blog: unavailable
  - Tags: [memory, meta-learning, few-shot learning]
  - Impact: Important early result showing external memory can support rapid adaptation and few-shot learning.

---

## Kyutai

- Moshi: A Speech-Text Foundation Model for Real-Time Dialogue
  - Paper: https://arxiv.org/abs/2410.00037
  - Blog: https://kyutai.org/blog/2024-09-18-moshi-release
  - Tags: [multimodal, speech, real-time agents, dialogue]
  - Impact: Open real-time speech-text foundation model for low-latency spoken interaction and voice-agent loops.

---

## Thinking Machines Lab

- Public technical essays
  - Paper: unavailable
  - Blog: https://thinkingmachines.ai/blog/
  - Example: https://thinkingmachines.ai/blog/interaction-models/
  - Tags: [AI systems, interaction design, agents]
  - Impact: Public technical writing exists, but I found no lab-attributed peer-reviewed/arXiv technical paper yet.

---

## Amii / Alberta / Sutton-linked research

- The Bitter Lesson
  - Paper/blog: http://incompleteideas.net/IncIdeas/BitterLesson.html
  - Blog: same
  - Tags: [scaling, architecture philosophy, agents, compute]
  - Impact: Highly influential thesis that scalable search/learning with compute beats hand-engineered knowledge.

- Reward Is Enough
  - Paper: https://deepmind.google/research/publications/reward-is-enough
  - Blog: unavailable
  - Tags: [agents, RL, objectives, general intelligence]
  - Impact: Sutton/Silver et al. argument that reward-driven RL is sufficient in principle for broad intelligent behavior.

- The Era of Experience
  - Paper: https://storage.googleapis.com/deepmind-media/Era-of-Experience%20/The%20Era%20of%20Experience%20Paper.pdf
  - Blog: unavailable
  - Tags: [agents, RL, continual learning, experience]
  - Impact: Argues for moving beyond static human data toward agents learning from streams of self-generated experience.

---

## Cross-lab papers that are too important to omit

- LoRA: Low-Rank Adaptation of Large Language Models
  - Paper: https://arxiv.org/abs/2106.09685
  - Blog/GitHub: https://github.com/microsoft/LoRA
  - Tags: [fine-tuning, efficiency, architecture tweak]
  - Impact: Became a standard low-cost adaptation method for LLMs and multimodal models.

- QLoRA
  - Paper: https://arxiv.org/abs/2305.14314
  - Blog/GitHub: https://github.com/artidoro/qlora
  - Tags: [fine-tuning, quantization, efficiency]
  - Impact: Made high-quality LLM fine-tuning feasible on consumer/prosumer GPUs.

- vLLM / PagedAttention
  - Paper: https://arxiv.org/abs/2309.06180
  - Blog/GitHub: https://github.com/vllm-project/vllm
  - Tags: [inference, serving, KV cache, context engineering]
  - Impact: Became a standard high-throughput serving engine and popularized PagedAttention.

- Speculative Decoding
  - Paper: https://arxiv.org/abs/2211.17192
  - Blog: unavailable
  - Tags: [inference, decoding, efficiency]
  - Impact: Major test-time acceleration technique using draft models to speed generation.

- Lost in the Middle
  - Paper: https://arxiv.org/abs/2307.03172
  - Blog/GitHub: https://github.com/nelson-liu/lost-in-the-middle
  - Tags: [context engineering, long context, eval]
  - Impact: Demonstrated that LLMs often fail to use information placed in the middle of long contexts.

- MemGPT
  - Paper: https://arxiv.org/abs/2310.08560
  - Blog/GitHub: https://github.com/cpacker/MemGPT
  - Tags: [memory, agents, context management]
  - Impact: Influential operating-system metaphor for managing limited context with long-term memory.

- Mamba
  - Paper: https://arxiv.org/abs/2312.00752
  - Blog/GitHub: https://github.com/state-spaces/mamba
  - Tags: [architecture, state-space models, long context]
  - Impact: Renewed interest in efficient non-attention sequence models for long-context tasks.

- RWKV
  - Paper: https://arxiv.org/abs/2305.13048
  - Blog/GitHub: https://github.com/BlinkDL/RWKV-LM
  - Tags: [architecture, RNN, Transformer alternative]
  - Impact: Influential open recurrent architecture line positioned as an attention alternative.

- BIG-bench
  - Paper: https://arxiv.org/abs/2206.04615
  - Blog/GitHub: https://github.com/google/BIG-bench
  - Tags: [eval harness, benchmark, emergent abilities]
  - Impact: Major collaborative benchmark suite for probing broad LLM capabilities.

- MMLU
  - Paper: https://arxiv.org/abs/2009.03300
  - Blog: unavailable
  - Tags: [eval, benchmark, knowledge]
  - Impact: Became a default benchmark for broad multitask knowledge evaluation.

- HumanEval
  - Paper: https://arxiv.org/abs/2107.03374
  - Blog/GitHub: https://github.com/openai/human-eval
  - Tags: [code LLM, eval harness]
  - Impact: Became the standard initial benchmark for code-generation models.

- GSM8K
  - Paper: https://arxiv.org/abs/2110.14168
  - Blog/GitHub: https://github.com/openai/grade-school-math
  - Tags: [reasoning, math, eval]
  - Impact: Became a core benchmark for grade-school mathematical reasoning and CoT prompting.

- RAG: Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks
  - Paper: https://arxiv.org/abs/2005.11401
  - Blog: unavailable
  - Tags: [RAG, retrieval, memory, context engineering]
  - Impact: Canonical retrieval-augmented generation paper combining parametric and non-parametric memory.

- HyDE
  - Paper: https://arxiv.org/abs/2212.10496
  - Blog: unavailable
  - Tags: [RAG, retrieval, prompt engineering]
  - Impact: Influential retrieval trick using hypothetical generated documents as query representations.

---

## Gaps and caveats

- Thinking Machines Lab: I found public technical writing but no original paper yet.
- Some commercial Chinese assistants, including Doubao, ERNIE Bot, SenseNova/SenseChat, and iFlytek Spark, have official product/model pages but limited reliable public technical papers.
- Midjourney-style image labs and some product-first AI companies publish little formal research; they are omitted unless a reliable technical paper/blog exists.
- Some entries are cross-lab or university/company collaborations; I grouped by the most useful research identity rather than trying to perfectly assign institutional credit.
