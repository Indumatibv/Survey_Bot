Survey Bot - PoC Documentation

📌 Overview

The Survey Bot Proof of Concept (PoC) aims to automate multilingual voice-based survey collection. This system is designed to handle complex conversational inputs in Kannada, Tamil, Malayalam, and English, transforming them into structured responses through speech recognition, translation, and contextual understanding.

🎯 Objective

To evaluate the feasibility of a real-time, multilingual, speech-driven survey bot by integrating:

Voice-based input capture.

Multilingual transcription and translation.

Contextual understanding of natural language.

Structured answer extraction from unstructured speech.

Phase 1: Capabilities Demonstrated

1. Speech Recognition

Handled Kannada input from:

🎤 Live Microphone
📁 Uploaded Audio Files
📝 Direct Text Input

Chosen Model:

speech_recognition (Google Web Speech API)

✅ Fast and responsive for live transcription.

❌ Whisper by OpenAI was rejected due to latency in real-time use.

2. Translation

Model Used: IndicTrans2 (ai4bharat/indictrans2-indic-en-1B).

✅ Accurate Kannada-to-English translation.

✅ Supports long and colloquial sentences.

3. Punctuation Restoration

Library: deepmultilingualpunctuation.

Restores punctuation for better structure and readability before feeding into downstream QA models.

4. Semantic Evaluation

Why Not Word Error Rate (WER)?

❌ JiWER lacked contextual understanding.

Chosen Approach:

✅ sentence-transformers for semantic similarity between generated and reference outputs.

5. Question Answering

Final Model: distilbert-base-cased-distilled-squad

✅ Accurate answer extraction from long, translated responses.

❌ Other models (T5, FLAN, Mistral) were less consistent or resource-heavy.

6. Custom Enhancements

Vocabulary Mapping: Ensures model responses align with predefined answer options.

Engineer Intervention Flag: Raised for new questions with unseen options.

Keyword Context Detection: Uses SpaCy to validate if relevant context is present.

Text Normalization: Converts textual numbers (e.g., “two”) to numerical equivalents (“2”).

🔄 Example Workflow

Input (Kannada Audio):
“ನಾನು ವಾರಕ್ಕೆ ಎರಡು ಬಾರಿ ಮದ್ಯಪಾನ ಮಾಡುತ್ತೇನೆ…”

STT Output:
“I drink alcohol two times a week…”

QA Question: “How many times a week do you consume alcohol?”

Final Answer: "2"

If context is missing (e.g., no mention of “walk”), system returns: “No answer found”

Dependency Versions-

| Domain            | Libraries / Tools Used                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------- |
| Speech Processing | `speech_recognition`, `pydub`, `scipy.io.wavfile`                                                       |
| NLP / ML          | `transformers`, `sentence-transformers`, `IndicTrans2`, `deepmultilingualpunctuation`, `spacy`, `torch` |
| Utilities         | `tempfile`, `os`, `threading`, `json`, `pandas`, `numpy`                                                |

✅ Outcome

The PoC demonstrates a robust pipeline for:

Multilingual voice input handling.

Accurate transcription and translation.

Semantic QA with structured answer mapping.

Custom rule-based processing for real-world survey logic.

This lays a strong foundation for a scalable, AI-powered, real-time survey assistant.

✅ Phase 2: Enhancements & Fixes

Standardized question format to match Confluence documentation and updated the codebase accordingly.

Added new test questions to expand QA coverage.

Faced challenges with questions having predefined options, where model predictions (e.g., “200 ml”) didn’t exactly match options (e.g., “< 250 ml”).

Example fixes:

Oil Consumption: “200 ml” → mapped to "< 250 ml"

Meal Timing: “around seven in the evening” → mapped to "Before 7:00 PM".

Achieved higher accuracy by combining model outputs with rule-based mapping and contextual cues.
