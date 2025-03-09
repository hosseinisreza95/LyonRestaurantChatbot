# Lyon Restaurant Chatbot (Flan-T5-Large + RAG)

A minimal guide to building a **restaurant chatbot** specialized for **Lyon**, combining:
- **Flan-T5-Large** (fine-tuned on **CamRest676** for domain style).  
- **Retrieval-Augmented Generation** (RAG) with an **OSM-based** restaurant dataset.

## Overview

1. **Fine-Tune Flan-T5 on CamRest**  
   - Extract `(user, system)` pairs from `dialogues.json` (CamRest).  
   - Treat them as `(input_text, target_text)` for the T5 model.  
   - Train in a GPU environment (e.g., Colab).

2. **Query OSM for Lyon Restaurants**  
   - Fetch name, cuisine, phone, website, lat/lon, etc. using Overpass (`overpy`).  
   - Convert each record into a text description.

3. **Embed & Retrieve**  
   - Encode each restaurant description with Sentence Transformers.  
   - On user query, compute similarity to find the top matches.  
   - Feed these matches as context to the **fine-tuned T5** model.

4. **Chat Interface**  
   - Use **Gradio** to build a simple “chatbot” UI.  
   - Each user query triggers RAG retrieval → appended to a T5 prompt → final answer.  

## Steps to Reproduce

1. **Install**  
   ```bash
   pip install transformers datasets accelerate sentencepiece overpy gradio sentence-transformers
2. **Fine-Tune Flan-T5**
  - Load CamRest dialogues.
  - Tokenize and train.
  - Save model to `camrest_finetuned_model/.`

3. **Query OSM**
  - Use `overpy` to retrieve restaurants in Lyon.
  - Store data (CSV, JSON, DataFrame).

4. **Embed**
  - Use Sentence Transformers (`all-MiniLM-L6-v2`) to embed each restaurant.
  - Save embeddings for fast retrieval.
    
5. **Build Chatbot**
  - Load fine-tuned T5 + embeddings.
  - Implement a retrieval function.
  - Construct a Gradio UI that:
    1. Accepts user queries.
    2. Finds top-k OSM matches.
    3. Appends them to the T5 prompt.
    4. Returns the final T5-generated response.
       
## Example Queries
  - “Recommend a cheap French restaurant in Lyon.”
  - “How much will it cost me to eat at Milprée?”
  - “Phone number for Bistrot Bouille?”
  - “Sushi places open on Sunday near lat=45.76, lon=4.83?”
    
## Acknowledgments
  - Hugging Face for Transformers and the CamRest references.
  - OpenStreetMap for crowd-sourced restaurant data in Lyon.
  - Sentence Transformers for quick embedding solutions.
