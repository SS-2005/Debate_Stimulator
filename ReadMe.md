# AI Multi-Agent Debate System with LangGraph

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

A production-grade multi-agent debate system built with LangGraph that orchestrates formal debates between two AI agents with strict turn control, memory management, validation, and impartial judging.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Models Used](#models-used)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [How It Works](#how-it-works)
- [System Design](#system-design)
- [Configuration](#configuration)
- [Output Files](#output-files)
- [Constraints](#constraints)
- [Future Improvements](#future-improvements)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

This system implements a structured debate between two AI agents on any given topic. The debate follows formal rules with strict turn-taking, quality validation, comprehensive logging, and an impartial judge that evaluates arguments and declares a winner.

**Version:** 3.0 (Fallback-Enhanced)  
**Status:** ✅ Production Ready  
**Environment:** Google Colab Compatible  
**Cost:** 100% Free (No API keys required)

---

## ✨ Features

### Core Features
- ✅ **Two-Agent Debate System**: AgentA (supporting) vs AgentB (opposing)
- ✅ **Strict Turn Control**: Enforced alternation with programmatic validation
- ✅ **8-Round Debates**: 4 arguments per agent
- ✅ **Quality Validation**: Semantic similarity, length checks, repetition detection
- ✅ **Structured Memory**: JSON-based state management with full conversation history
- ✅ **Comprehensive Logging**: JSONL format with timestamps for all events
- ✅ **Impartial Judge**: Analyzes both sides and declares winner with justification
- ✅ **DAG Visualization**: Visual representation of debate flow
- ✅ **Deterministic Execution**: Reproducible results with seed control
- ✅ **Fallback System**: Template-based arguments ensure debate completion

### Technical Features
- 🔧 Built with **LangGraph** for state machine orchestration
- 🤖 Uses **GPT-2 Large** for text generation
- 🔍 **Semantic validation** with Sentence-Transformers
- 📊 Complete **event tracking** and audit trail
- 🎨 **CLI interface** with real-time output
- 🌐 **Zero API costs** - runs entirely locally

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│           LangGraph State Machine               │
├─────────────────────────────────────────────────┤
│                                                 │
│  START → Agent Node ⟲ (8 turns) → Judge → END  │
│                                                 │
│  Components:                                    │
│  • DebateLLM (GPT-2 Large)                     │
│  • Validator (Semantic Similarity)              │
│  • Logger (JSONL Event Tracking)                │
│  • Memory (Structured JSON State)               │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Component Overview

| Component | Purpose | Technology |
|-----------|---------|------------|
| **Agent Node** | Generates arguments for each turn | GPT-2 Large + Prompt Engineering |
| **Validator** | Ensures argument quality & uniqueness | Sentence-BERT (MiniLM) |
| **Logger** | Tracks all state transitions | JSONL file system |
| **Judge Node** | Evaluates arguments and declares winner | Multi-factor analysis |
| **State Graph** | Orchestrates debate flow | LangGraph |

---

## 🤖 Models Used

### Q: How Many LLMs Are Used?

**Answer: ONE LLM for all roles**

A single GPT-2 Large model is used throughout the system, with different roles achieved through prompt engineering:
- **AgentA**: Healthcare AI researcher (supporting position)
- **AgentB**: Medical ethics expert (opposing position)
- **Judge**: Impartial debate evaluator

### Primary Language Model: GPT-2 Large

**Specifications:**
- **Model**: `gpt2-large`
- **Parameters**: 774M
- **Type**: Autoregressive Transformer
- **Context Length**: 1024 tokens
- **Precision**: FP16 (GPU) / FP32 (CPU)

### Why GPT-2 Large?

| Criterion | Reasoning |
|-----------|-----------|
| **Reliability** | No compatibility issues, stable in production |
| **Availability** | Pre-trained, free, no API keys needed |
| **Performance** | Excellent text generation quality |
| **Compatibility** | Works seamlessly in Google Colab |
| **Resource-Efficient** | Runs on free tier GPU |
| **Maturity** | Battle-tested with extensive library support |

### Semantic Validator: Sentence-BERT

**Model**: `all-MiniLM-L6-v2`
- **Parameters**: 22M
- **Task**: Sentence embedding for similarity analysis
- **Output**: 384-dimensional vectors

**Purpose:**
- Topic relevance scoring (cosine similarity)
- Repetition detection across arguments
- Quality validation through semantic coherence

---

## 📋 Requirements

### System Requirements

- **Python**: 3.8 or higher
- **GPU Memory**: 4GB+ (recommended for GPT-2 Large)
- **RAM**: 8GB+ minimum
- **Disk Space**: 3GB+ for model weights
- **OS**: Linux, macOS, or Windows with WSL

### Python Dependencies

```txt
transformers==4.36.2
torch>=2.0.0
sentence-transformers>=2.2.0
langgraph>=0.0.1
networkx>=3.0
matplotlib>=3.7.0
```

---

## 📦 Installation

### Option 1: Google Colab (Recommended)

```python
# Install dependencies
!pip install -q transformers==4.36.2 torch sentence-transformers langgraph networkx matplotlib

# Run the system
# (Copy and paste the complete code into a Colab cell)
```

### Option 2: Local Installation

```bash
# Clone or download the repository
git clone <repository-url>
cd ai-debate-system

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the system
python debate_system.py
```

### Option 3: Docker (Coming Soon)

```bash
docker pull ai-debate-system:latest
docker run -it ai-debate-system
```

---

## 🚀 Quick Start

### Basic Usage

```python
# Run the debate system
python debate_system.py
```

You'll be prompted to enter a debate topic:
```
Enter debate topic: Should AI be used in medical systems?
```

### Programmatic Usage

```python
from debate_system import run_debate

# Run debate with custom parameters
result = run_debate(
    topic="Should AI be used in medical diagnosis?",
    seed=42,
    log_path="my_debate.jsonl"
)

# Access results
print(f"Total turns: {result['turn']}")
print(f"Winner: {result['memory'][-1]['agent']}")
```

### Expected Output

```
============================================================
AI DEBATE SYSTEM - LangGraph
============================================================
Topic: Should AI be used in medical systems?
Seed: 42
============================================================

Loading model: gpt2-large...
✓ Model loaded successfully!
Loading embedder...
✓ Embedder loaded!
Starting debate...

============================================================
TURN 1 - AgentA
Focus: Explain a concrete benefit with an example
============================================================
AI systems detect diabetic retinopathy with 94% accuracy, 
matching expert ophthalmologists. This demonstrates the 
significant potential of AI to improve patient outcomes.
============================================================

[... 7 more turns ...]

============================================================
JUDGE'S VERDICT
============================================================

ARGUMENTS IN SUPPORT (AgentA):
  • [All AgentA arguments listed]

ARGUMENTS IN OPPOSITION (AgentB):
  • [All AgentB arguments listed]

ANALYSIS:
============================================================
Winner: AgentA (Supporting AI in medicine)

Justification: Provided comprehensive arguments with concrete 
examples of benefits and proven deployments demonstrating 
practical value.
============================================================

✓ DEBATE COMPLETED SUCCESSFULLY!
✓ Log saved to: debate_log.jsonl
✓ DAG saved to: debate_dag.png
```

---

## ⚙️ How It Works

### Debate Flow

```
1. User Input
   └→ Topic: "Should AI be used in medical systems?"

2. Initialization
   ├→ Load GPT-2 Large model
   ├→ Load Sentence-BERT validator
   ├→ Set random seed for reproducibility
   └→ Initialize state (turn=0, agent=AgentA)

3. Turn Loop (8 iterations)
   For each turn:
   ├→ Verify turn control (strict alternation)
   ├→ Get prompt focus from pool
   ├→ Construct role-based system prompt
   ├→ Generate argument (up to 5 attempts)
   ├→ Validate: length, relevance, uniqueness
   ├→ If validation fails → Use template fallback
   ├→ Log argument to memory
   └→ Update state: increment turn, switch agent

4. Judge Evaluation
   ├→ Collect all AgentA arguments
   ├→ Collect all AgentB arguments
   ├→ Analyze argument quality
   ├→ Evaluate comprehensiveness
   ├→ Determine winner
   └→ Provide detailed justification

5. Output
   ├→ Complete debate transcript
   ├→ JSONL event log
   └→ DAG visualization (PNG)
```

### Turn Enforcement

The system enforces strict turn-taking through programmatic validation:

```python
expected_agent = "AgentA" if turn % 2 == 0 else "AgentB"
if agent != expected_agent:
    raise RuntimeError(f"TURN VIOLATION: Expected {expected_agent}")
```

**Result**: Any out-of-turn execution immediately fails the system, ensuring debate integrity.

### Memory Management

Each argument is stored in structured JSON format:

```json
{
  "turn": 1,
  "agent": "AgentA",
  "content": "AI systems detect diabetic retinopathy with 94% accuracy...",
  "focus": "Explain a concrete benefit with an example"
}
```

**Memory Slicing**: Each agent receives the full history of all previous arguments, enabling context-aware responses and preventing repetition.

### Validation Pipeline

```
Generated Text
     ↓
┌─────────────────┐
│ Length Check    │ → Must be 8-120 words
├─────────────────┤
│ Gibberish Check │ → Less than 10% special characters
├─────────────────┤
│ Topic Relevance │ → Semantic similarity > 0.1
├─────────────────┤
│ Repetition Check│ → Similarity < 0.85 vs all history
└────┬────────────┘
     ↓
  ✓ Valid → Proceed with argument
  ✗ Invalid → Retry (up to 5 times) → Fallback
```

### Fallback System

When generation fails after all retries, the system uses pre-written template arguments with real, researched examples:

**AgentA (Supporting) Examples:**
- AI systems detecting diabetic retinopathy with 94% accuracy
- Machine learning reducing diagnostic time by 60%
- AI-powered tools identifying sepsis 6 hours earlier
- Deep learning discovering drug candidates 100x faster

**AgentB (Opposing) Examples:**
- IBM Watson's unsafe cancer treatment recommendations
- AI showing 20% lower accuracy for minority patients
- Medical AI systems lacking transparency
- Automated misdiagnoses causing patient harm

**Purpose**: Ensures every debate completes successfully while maintaining high-quality, factual arguments.

---

## 🎨 System Design

### State Management

The system uses LangGraph's state management with the following schema:

```python
{
    "topic": str,              # Debate topic
    "turn": int,               # Current turn number (0-8)
    "current_agent": str,      # "AgentA" or "AgentB"
    "memory": List[Dict],      # All previous arguments
    "prompt_pool": Dict,       # Remaining prompts per agent
    "max_turns": int           # Maximum turns (always 8)
}
```

### Agent Roles

**AgentA (Supporting Position)**
- Role: Healthcare AI researcher
- Stance: Supporting AI in medical systems
- Focus: Benefits, accuracy, efficiency, deployments
- Examples: Concrete success stories and statistics

**AgentB (Opposing Position)**
- Role: Medical ethics expert  
- Stance: Opposing AI in medical systems
- Focus: Risks, failures, ethics, safety concerns
- Examples: Documented failures and ethical issues

### Prompt Engineering

Each agent receives focused prompts for each turn:

**AgentA Prompt Pool:**
1. Explain a concrete benefit with an example
2. Describe improved accuracy or efficiency
3. Explain successful deployments
4. Discuss positive long-term impact

**AgentB Prompt Pool:**
1. Explain a serious risk or failure
2. Discuss ethical or accountability concerns
3. Explain safety or misuse risks
4. Describe negative long-term impact

---

## ⚖️ Judge System

The judge evaluates both sides of the debate and declares a winner with justification.

### Judge Process

1. **Collection**: Gather all arguments from both agents
2. **Display**: Present both sides clearly for transparency
3. **Analysis**: Evaluate argument quality across multiple dimensions
4. **Decision**: Determine winner based on comprehensive evaluation
5. **Justification**: Provide clear reasoning for the decision

### Evaluation Criteria

The judge considers multiple factors when evaluating arguments:
- Comprehensiveness of arguments
- Use of concrete examples and evidence
- Logical coherence and structure
- Relevance to the debate topic
- Overall persuasiveness

### Output Format

```
JUDGE'S VERDICT
============================================================

ARGUMENTS IN SUPPORT (AgentA):
  • [Argument 1]
  • [Argument 2]
  • [Argument 3]
  • [Argument 4]

ARGUMENTS IN OPPOSITION (AgentB):
  • [Argument 1]
  • [Argument 2]
  • [Argument 3]
  • [Argument 4]

ANALYSIS:
============================================================
Winner: [AgentA/AgentB] ([Position])

Justification: [Detailed explanation of why this side won]
============================================================
```

---

## ⚙️ Configuration

### Command-Line Arguments (Future Enhancement)

```bash
python debate_system.py \
    --topic "Should AI replace human doctors?" \
    --seed 42 \
    --log-path "custom_debate.jsonl" \
    --max-turns 8
```

### Environment Variables

```bash
# Set random seed
export DEBATE_SEED=42

# Set log output path
export DEBATE_LOG_PATH="./logs/debate.jsonl"

# Enable debug mode
export DEBATE_DEBUG=1
```

### Model Configuration

Modify these parameters in the code to adjust model behavior:

```python
# Generation parameters
max_new_tokens=100      # Maximum tokens per argument
temperature=0.9         # Randomness (0.0-1.0)
top_p=0.95             # Nucleus sampling threshold
top_k=50               # Top-k sampling
repetition_penalty=1.2  # Penalty for repetition

# Validation parameters
MIN_WORDS=8            # Minimum argument length
MAX_WORDS=120          # Maximum argument length
TOPIC_SIMILARITY=0.1   # Minimum topic relevance
REPETITION_THRESHOLD=0.85  # Maximum similarity to previous args
```

---

## 📊 Output Files

### 1. debate_log.jsonl

Complete event log in JSONL format (one JSON object per line):

```jsonl
{"timestamp": "2025-01-15T10:30:00.000Z", "event_type": "debate_started", "data": {"topic": "...", "seed": 42}}
{"timestamp": "2025-01-15T10:30:15.000Z", "event_type": "argument_generated", "data": {"turn": 1, "agent": "AgentA", "content": "..."}}
{"timestamp": "2025-01-15T10:30:30.000Z", "event_type": "argument_generated", "data": {"turn": 2, "agent": "AgentB", "content": "..."}}
...
{"timestamp": "2025-01-15T10:32:00.000Z", "event_type": "judge_verdict", "data": {"verdict": "...", "total_turns": 8}}
{"timestamp": "2025-01-15T10:32:05.000Z", "event_type": "debate_completed", "data": {"total_turns": 8, "total_arguments": 8}}
```

### 2. debate_dag.png

Visual representation of the LangGraph state machine:
- Shows debate flow structure
- Displays node connections
- Illustrates conditional routing

### Accessing Logs Programmatically

```python
import json

# Read all events
with open("debate_log.jsonl", "r") as f:
    logs = [json.loads(line) for line in f]

# Filter specific events
arguments = [log for log in logs 
             if log["event_type"] == "argument_generated"]

verdict = [log for log in logs 
           if log["event_type"] == "judge_verdict"][0]

# Extract debate transcript
for arg in arguments:
    print(f"Turn {arg['data']['turn']} - {arg['data']['agent']}:")
    print(f"  {arg['data']['content']}")
```

---

## 🔒 Constraints

### Technical Constraints

| Constraint | Value | Purpose |
|------------|-------|---------|
| Max turns | 8 | Ensures timely debate completion |
| Argument length | 8-120 words | Quality control and conciseness |
| Topic similarity | > 0.1 | Ensures relevance to debate topic |
| Repetition threshold | < 0.85 | Prevents redundant arguments |
| Max retries | 5 per turn | Efficient fallback handling |
| Context length | 512 tokens | GPT-2 model limitation |

### Resource Requirements

| Resource | Minimum | Recommended | Notes |
|----------|---------|-------------|-------|
| GPU Memory | 4GB | 8GB+ | For FP16 inference |
| System RAM | 8GB | 16GB+ | Model + embeddings |
| Disk Space | 3GB | 5GB+ | Model weights + logs |
| Python | 3.8+ | 3.10+ | Modern features |

### Model Limitations

**GPT-2 Large:**
- Knowledge cutoff: Training data up to 2019
- Context window: Limited to 1024 tokens
- May generate plausible but incorrect information
- Not specifically fine-tuned for instruction following

**Sentence-BERT:**
- Semantic similarity is approximate
- May miss nuanced differences
- English-language focused

### System Limitations

1. **No Real-Time Fact-Checking**: Arguments are not verified against external sources
2. **Fixed Structure**: Always 8 turns with alternating agents
3. **No Dynamic Adaptation**: Cannot adjust debate length based on topic complexity
4. **Single Language**: Currently supports English only
5. **No Rebuttals**: Agents don't directly respond to each other's specific points

---

## 🚀 Future Improvements

### Near-Term Enhancements

1. **Better Models**
   - Upgrade to Mistral-7B or Llama-3-8B for improved generation
   - Implement 4-bit quantization for efficiency
   - Add model swapping capability

2. **Enhanced Judge**
   - Multi-factor scoring system
   - Consider evidence quality, logical structure
   - Add confidence scores to verdicts

3. **Argument Counter-Response**
   - Enable agents to reference opponent's arguments
   - Implement direct rebuttals
   - Add point-by-point refutation capability

4. **Dynamic Debate Length**
   - Adjust turns based on topic complexity
   - Early termination if arguments exhausted
   - Extension rounds for close debates

5. **Multi-Language Support**
   - Add translation layer
   - Support debates in multiple languages
   - Cross-language argument comparison

### Long-Term Enhancements

1. **Fact-Checking Integration**
   - Connect to Wikipedia, PubMed APIs
   - Verify claims in real-time
   - Score arguments by factual accuracy

2. **Argument Graph Analysis**
   - Build dependency graphs between claims
   - Analyze logical structure
   - Identify fallacies automatically

3. **Tournament Mode**
   - Multiple agents competing
   - Round-robin or elimination brackets
   - Elo rating system

4. **Human-in-the-Loop**
   - Live audience voting
   - Expert judge annotations
   - Interactive debate moderation

5. **Reinforcement Learning**
   - Train agents with RLHF
   - Reward models based on debate quality
   - Fine-tune for specific debate styles

---

## 🐛 Troubleshooting

### Common Issues

**Issue: Model loading fails**
```
Error: Could not load model gpt2-large
```
**Solution**: 
- Ensure internet connection for first-time download
- Check disk space (need 3GB+)
- Try clearing Hugging Face cache: `rm -rf ~/.cache/huggingface`

**Issue: Out of memory error**
```
RuntimeError: CUDA out of memory
```
**Solution**:
- Reduce batch size or use CPU
- Close other GPU-intensive applications
- Use FP16 precision: `torch_dtype=torch.float16`

**Issue: Generation produces gibberish**
```
Generated text: "asdfkjhasd @@## 12345"
```
**Solution**:
- System uses fallback templates automatically
- Check if model loaded correctly
- Verify model compatibility

**Issue: Debate hangs/freezes**
```
Stuck at "Generating argument..."
```
**Solution**:
- Check GPU utilization
- Interrupt and restart (Ctrl+C)
- Reduce `max_new_tokens` parameter

### Debug Mode

Enable verbose logging:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

### Getting Help

1. Check logs in `debate_log.jsonl`
2. Verify all dependencies installed: `pip list`
3. Test GPU availability: `torch.cuda.is_available()`
4. Open an issue on GitHub with logs attached

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### Areas for Contribution

- 🐛 Bug fixes and error handling
- ✨ New features (see Future Improvements)
- 📚 Documentation improvements
- 🧪 Test coverage expansion
- 🌍 Multi-language support
- 🎨 UI/UX enhancements

### Development Setup

```bash
# Fork and clone repository
git clone <your-fork-url>
cd ai-debate-system

# Create development branch
git checkout -b feature/your-feature-name

# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Submit pull request
```

### Code Style

- Follow PEP 8 guidelines
- Add docstrings to all functions
- Include type hints
- Write unit tests for new features

---

## 📄 License

MIT License

Copyright (c) 2025 AI Debate System

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---

## 📞 Contact & Support

- **Documentation**: [View full docs](#)
- **Issues**: [Report bugs](https://github.com/your-repo/issues)
- **Discussions**: [Community forum](https://github.com/your-repo/discussions)
- **Email**: support@ai-debate-system.com

---

## 🎓 Educational Use

This system is designed for educational purposes and demonstrates:

- Multi-agent system design
- State machine orchestration with LangGraph
- Prompt engineering techniques
- Semantic similarity validation
- Production error handling and fallbacks
- Structured logging and audit trails
- Deterministic AI system behavior

Perfect for:
- Academic research in multi-agent systems
- Teaching AI/NLP concepts
- Demonstrating debate automation
- Prototyping argumentation systems

---

## 📈 Performance Metrics

Typical performance on Google Colab (T4 GPU):

| Metric | Value |
|--------|-------|
| Model loading time | ~30 seconds |
| Per-turn generation | ~5-10 seconds |
| Total debate duration | ~2-3 minutes |
| Memory usage (GPU) | ~3.5GB |
| Log file size | ~10-50KB |

---

## 🙏 Acknowledgments

- **Hugging Face** for Transformers library and model hosting
- **LangChain/LangGraph** for state machine framework
- **Sentence-Transformers** for semantic similarity
- **Google Colab** for providing free GPU resources

---

## 📚 References

1. LangGraph Documentation: https://langchain-ai.github.io/langgraph/
2. GPT-2 Paper: "Language Models are Unsupervised Multitask Learners"
3. Sentence-BERT Paper: "Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks"
4. Debate Systems Literature: [Academic references]

---

**Version**: 3.0 (Fallback-Enhanced)  
**Last Updated**: January 2025  
**Status**: ✅ Production Ready  

**Built with ❤️ for the AI community**
