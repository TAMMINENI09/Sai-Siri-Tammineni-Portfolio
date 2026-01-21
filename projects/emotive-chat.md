# EmotiveChat — Emotion Classification with BERT for Sentiment-Aware Conversations

## 1. Problem Statement
Emotion detection in short text is difficult because inputs are brief, context is limited, and meaning often depends on subtle phrasing, negation, or implied tone. Ambiguity and semantic overlap are common (e.g., guilt vs. shame, fear vs. sadness), which makes separability challenging. Classical ML approaches tend to rely on surface features and struggle with these nuances, while contextual transformers can model meaning in relation to surrounding words.

## 2. Project Goal
The goal is to build a BERT-based multi-class emotion classifier and integrate it into a chatbot so responses can adapt based on detected emotional context. Emotion labels guide response tone and strategy (e.g., supportive, concise, neutral), with the model acting as an augmentation layer rather than any form of diagnosis.

## 3. Why BERT
TF-IDF, Word2Vec, and classical ML models underperformed because they flatten context and lose the cues needed to distinguish adjacent emotions. BERT’s bidirectional attention captures how meaning changes based on surrounding tokens, which improves separation between semantically close classes. Even with BERT, overlapping emotions remain a challenge and require careful evaluation.

## 4. Dataset & Methodology
The model is trained on the ISEAR dataset, which contains short, self-reported emotion descriptions labeled across multiple classes (e.g., joy, fear, anger, sadness, disgust, shame, guilt). Preprocessing includes normalization, tokenization with the BERT tokenizer, and encoding into input IDs and attention masks. Class imbalance is monitored to avoid overfitting to dominant labels. Dataset limitations include cultural and linguistic bias and the gap between elicited descriptions and real chat messages.

## 5. Model Architecture & Training
The system fine-tunes `bert-base-uncased` with a task-specific classification head. Training is implemented in PyTorch using AdamW with cross-entropy loss. Learning rate scheduling, early stopping, and regularization are used to stabilize training and reduce overfitting. Reported results reflect measured metrics only.

## 6. Evaluation & Results
Overall accuracy was approximately 92% and used as a supporting metric. Macro F1-score was used to account for class imbalance and to avoid overestimating performance on dominant labels. Per-class behavior was reviewed qualitatively via the confusion matrix; semantically close emotions (e.g., guilt vs. shame, fear vs. sadness) showed the most overlap, while more distinct emotions were easier to separate. This reinforced that accuracy alone was insufficient for judging model quality.

## 7. System Integration
The classifier is integrated into a Flask-based backend. Incoming messages are preprocessed, run through the model for real-time inference, and mapped to an emotion label with a confidence score. The chatbot uses this signal to adapt response tone or strategy, without framing outputs as diagnostic or therapeutic.

## 8. Limitations & Risks
- Dataset size and cultural bias can skew predictions toward the training distribution
- Short inputs are inherently ambiguous and can be misclassified without broader context
- Overconfidence and poor calibration can lead to misleading labels
- Inappropriate tone matching can create false empathy or user discomfort

## 9. Key Learnings
- Fine-tuning transformers requires careful preprocessing, class balance tracking, and regularization
- Evaluation beyond accuracy is necessary to understand per-class behavior and failure modes
- Emotion predictions should be treated as probabilistic signals, not ground truth

## 10. Future Improvements (Agentic AI Direction)
- Multi-turn emotional context tracking to reduce single-message ambiguity
- Vector memory for emotional history to adapt over time
- RAG for grounding responses in trusted sources when appropriate
- Goal-driven or agentic response planning for better conversational control
- User controls and confidence scoring to reduce misalignment