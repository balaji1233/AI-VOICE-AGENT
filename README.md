# 🎙️ Voice-Powered RAG Agent 

Build a complete end-to-end AI agent that accepts voice input, retrieves multimodal context, reasons with long-context models, and enforces safety guardrails—all using the latest NVIDIA Nemotron open models.

## 🌟 Features

- **Voice Input**: Nemotron Speech ASR for real-time speech-to-text
- **LangChain 1.0 Agent**: Uses `langgraph.prebuilt.create_react_agent` with automatic looping
- **RAG as a Tool**: On-demand retrieval - agent decides when to search knowledge base
- **Automatic Agent Loop**: Can call tools multiple times until it has enough information
- **Multimodal RAG**: Embed and retrieve both text and document images
- **Smart Reranking**: Improve retrieval accuracy by 6-7% with cross-encoder reranking
- **Image Understanding**: Describe visual content in context using vision-language models
- **Long-Context Reasoning**: Generate responses with 1M token context window
- **Safety Guardrails (Always On)**: PII detection and content moderation enforced on all inputs/outputs

## 📦 Models Used

| Component | Model | Parameters | Deployment |
|-----------|-------|------------|------------|
| **Speech-to-Text** | `nvidia/nemotron-speech-streaming-en-0.6b` | 600M | Self-hosted (NeMo) |
| **Embeddings** | `nvidia/llama-nemotron-embed-vl-1b-v2` | 1.7B | Self-hosted (Transformers) |
| **Reranking** | `nvidia/llama-nemotron-rerank-vl-1b-v2` | 1.7B | Self-hosted (Transformers) |
| **Vision-Language** | `nvidia/nemotron-nano-12b-v2-vl` | 12B | NVIDIA API |
| **Reasoning** | `nvidia/nemotron-3-nano-30b-a3b` | 30B | NVIDIA API |
| **Safety** | `nvidia/Llama-3.1-Nemotron-Safety-Guard-8B-v3` | 8B | Self-hosted (Transformers) |

## 🔧 Requirements

### Hardware
- **GPU**: NVIDIA GPU with at least 24GB VRAM recommended (for self-hosted models)
- **CUDA**: 11.8 or later

### Software
- Python 3.10+
- PyTorch 2.0+
- NVIDIA API Key (for cloud-hosted models)

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/balaji1233/AI-VOICE-AGENT.git

```

### 2. Set Up Environment

**Option A: Standard CUDA (RTX, A100, etc.):**
```bash
uv sync --extra cuda --index-url https://download.pytorch.org/whl/cu124
```

**Option B: DGX Spark (GB10):**
```bash
uv sync --extra cuda --index-url https://download.pytorch.org/whl/cu130
```

**Note:** Since `nemo_toolkit[asr]` may have specific PyTorch requirements, if you encounter dependency conflicts, install PyTorch first:

```bash
# For Spark/GB10 systems
uv pip install torch torchvision --index-url https://download.pytorch.org/whl/cu130
uv sync

# For standard CUDA systems
uv pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124
uv sync
```

### 3. Configure API Key

```bash
export NVIDIA_API_KEY="your-nvidia-api-key"
```

Get your API key from [NVIDIA NGC](https://ngc.nvidia.com/).

### 4. Run the Tutorial

```bash
jupyter notebook voice_rag_agent_tutorial.ipynb
```

## 📁 Project Structure

```
nemotron-voice-rag-agent-example/
├── voice_rag_agent_tutorial.ipynb  # Main tutorial notebook
├── README.md                        # This file
├── requirements.txt                 # Python dependencies
└── BlogSkeleton/                    # Blog content and model docs
    ├── BLOG.md
    ├── BLOG_UPDATED.md
    ├── Code Snippets/
    └── Model Information/
```

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│           Voice-Powered LangChain 1.0 Agent with RAG Tool           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  🎤 Voice Input → Nemotron Speech ASR → Text Query                  │
│                           ↓                                         │
│  🛡️ Input Safety Check (ALWAYS ENFORCED)                            │
│                           ↓                                         │
│  ┌─────────────────────────────────────────────────────┐            │
│  │        LangGraph ReAct Agent Loop                   │            │
│  │        (langgraph.prebuilt.create_react_agent)      │            │
│  │                                                     │            │
│  │  Agent (nemotron-3-nano-30b-a3b)                    │            │
│  │     │                                               │            │
│  │     ├─> Decide: Need more info?                     │            │
│  │     │                                               │            │
│  │     ├─> YES: Call RAG Tool ──┐                      │            │
│  │     │   ├── Embed            │                      │            │
│  │     │   ├── Vector Search    │                      │            │
│  │     │   ├── Rerank           │  LOOP                │            │
│  │     │   └── Describe Images  │  UNTIL               │            │
│  │     │                        │  SATISFIED           │            │
│  │     └─< Tool Result ─────────┘                      │            │
│  │     │                                               │            │
│  │     └─> NO: Generate final answer                   │            │
│  │                                                     │            │
│  └─────────────────────────────────────────────────────┘            │
│                           ↓                                         │
│  🛡️ Output Safety Check (ALWAYS ENFORCED)                           │
│                           ↓                                         │
│  📝 Safe Text Output                                                │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## 📖 Get Started

1. **Environment Setup**: Install dependencies and configure API keys
2. **Multimodal RAG**: Build embeddings and vector store for text + images
3. **Speech Input**: Add real-time speech transcription with Nemotron ASR
4. **Safety Guardrails**: Implement PII detection and content moderation
5. **Reasoning LLM**: Configure Nemotron for agent decision-making
6. **LangChain 1.0 Agent**: Create ReAct agent with automatic looping
   - Define RAG as a tool (not a fixed workflow step)
   - Use `langgraph.prebuilt.create_react_agent`
   - Agent automatically loops until it can answer
   - Safety enforced on all inputs and outputs

## 🎯 Use Cases

- **Enterprise Q&A**: Answer questions over documents with charts, tables, and images
- **Voice Assistants**: Build conversational AI with voice input
- **Compliance**: Detect PII and enforce content policies
- **Research**: Query scientific papers with visual content


## 📄 License

This project uses NVIDIA open models. Each model is governed by its respective license:
- [NVIDIA Open Model License](https://www.nvidia.com/en-us/agreements/enterprise-software/nvidia-open-model-license/)
- [Llama 3.1 Community License](https://www.llama.com/llama3_1/license/)



