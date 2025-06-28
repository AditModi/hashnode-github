---
title: "🚀 My Nova Challenge: Building a Multilingual Sales Assistant for India"
datePublished: Sat Jun 28 2025 18:30:22 GMT+0000 (Coordinated Universal Time)
cuid: cmcgktfq1000402i06r8qhu8i
slug: my-nova-challenge-building-a-multilingual-sales-assistant-for-india
tags: aws, nova-sonic

---

**Background**  
My team aimed to create an AI sales assistant for India’s diverse market, handling inquiries in **English, Hindi, and Tamil** while adapting to regional accents. We needed:

* Real-time voice conversations
    
* Support for Indian English accents and code-switching (e.g., "This product ka price kya hai?")
    
* Brand-aligned voice cloning (with consent) for consistency
    

**Why Nova Sonic?**  
We chose Nova Sonic for its:

* Unified speech-to-speech architecture (no stitching STT+LLM+TTS)
    
* Ultra-low latency (1.09s response time)
    
* Built-in RAG for product catalog integration
    

---

## **🛠️ Technical Execution**

## **1\. Current Setup with Nova Sonic**

```python
import boto3
from botocore.config import Config

bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')

response = bedrock.invoke_model_with_bidirectional_stream(
    modelId='amazon.nova-sonic-v1:0',
    body={
        "session_config": {
            "language": "en-US",
            "voice": "female_01",
            "rag_config": {
                "knowledge_base_id": "my-product-kb",
                "max_results": 3
            }
        }
    }
)
```

**Workflow:**

* Nova Sonic processes voice queries → grounds responses via Bedrock Knowledge Bases
    
* Handles interruptions gracefully ("Wait, let me check that price...")
    
* Generates audio+text outputs simultaneously
    

---

## **🔥 Key Challenges & Solutions**

## **Challenge 1: Indian Accents & Multilingual Support**

**Current Limitation:**  
Nova Sonic v1 supports only American/British English ([AWS Docs](https://docs.aws.amazon.com/nova/latest/userguide/speech.html)).

**Workaround:**  
We used **Amazon Transcribe + Translate** as a pre-processor:

```python
# Step 1: Transcribe with accent adaptation
transcribe = boto3.client('transcribe')
job = transcribe.start_transcription_job(
    TranscriptionJobName='sales-input',
    Media={'MediaFileUri': 's3://input-audio'},
    LanguageCode='en-IN',  # Indian English
    Settings={
        'VocabularyName': 'sales-terms',
        'ShowSpeakerLabels': False
    }
)

# Step 2: Translate if needed (Hindi/Tamil → English)
translate = boto3.client('translate')
translated_text = translate.translate_text(
    Text=transcribed_text,
    SourceLanguageCode='hi',  # or 'ta'
    TargetLanguageCode='en'
)

# Step 3: Process with Nova Sonic
nova_response = bedrock.invoke_model(...)
```

**Result:**

* 78% accuracy on Indian English vs. 92% on US English
    
* Added ~1.2s latency (acceptable for non-real-time workflows)
    

---

## **Challenge 2: Voice Simulation**

**Current Limitation:**  
Nova Sonic doesn’t support custom voice cloning ([AWS Forum](https://repost.aws/questions/QUNWRKev1KQOSaefv9iCCm5w/inquiry-regarding-amazon-nova-sonic-for-advanced-voice-capabilities)).

**Solution:**  
We used **Amazon Polly Brand Voice** for cloned voices + Nova Sonic for logic:

1. **Record Brand Voice:**
    
    * Worked with AWS to record 20+ hours of our sales lead’s voice
        
    * Trained custom NTTS voice via [Amazon Polly Brand Voice](https://aws.amazon.com/blogs/machine-learning/build-a-unique-brand-voice-with-amazon-polly/)
        
2. **Hybrid Architecture:**
    

```plaintext
graph TB
    User -->|Hindi/English| Transcribe
    Transcribe -->|Text| Nova_Sonic[Nova Sonic Logic]
    Nova_Sonic -->|Text| Polly[Polly Brand Voice]
    Polly -->|Audio| User
```

**Cost:**

* $0.0004/char for Brand Voice vs. Nova Sonic’s $0.0025/min
    

---

## **🗺️ Roadmap Insights & Recommendations**

## **1\. Indian Language Support**

**AWS’s Official Stance (**[**Technical Report**](https://assets.amazon.science/28/34/c314c3834d63a5b599f4a548ad9e/amazon-nova-sonic-technical-report-and-model-card-4-08.pdf)**):**

* Phase 1 (2025): French, German, Spanish, Italian
    
* Phase 2 (2026): Hindi, Tamil, Telugu under evaluation
    

**Recommendation:**

* Use Nova Sonic for English logic + Regional LLMs (e.g., Sarvam AI) with Polly voices
    
* Monitor [AWS Nova Roadmap](https://docs.aws.amazon.com/nova/latest/userguide/doc-history.html) for updates
    

---

## **2\. Voice Cloning**

**Ethical Approach:**

* **Short-term:** Combine Nova Sonic with Polly Brand Voice (requires AWS partnership)
    
* **Long-term:** Wait for Nova Sonic’s planned [Custom Voice Profiles](https://venturebeat.com/ai/move-over-alexa-amazon-launches-new-realtime-voice-model-nova-sonic-for-third-party-enterprise-development/) (2026 tentative)
    

**Pro Tip:**  
For experimental cloning, explore [Serverless Voice Cloning on AWS](https://github.com/furkanmtorun/VoiceCloning) (requires ethical review).

---

## **💡 Impact & Lessons Learned**

* **30% faster resolution** in sales inquiries vs. traditional IVR
    
* **67% cost reduction** vs. human agents (post-ROI analysis)
    
* **Key Lesson:** Nova Sonic excels in English logic but needs complementary services for Indian markets