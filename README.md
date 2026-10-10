# Stylistic Text Generation: Single-Layer LSTM vs. Stacked GRU

A comparative Deep Learning study evaluating **Cell Complexity vs. Network Depth** in Recurrent Neural Networks (RNNs) for character-level stylistic text generation, trained on the *Throne of Glass* fantasy novel series.

---

## Research Overview & Hypothesis

- **Core Question:** Can a multi-layer (Stacked) GRU architecture outperform a Single-Layer LSTM in terms of Perplexity and text generation quality when both models are independently optimized?
- **Initial Hypothesis:** A 2-layer Stacked GRU will capture long-term stylistic dependencies and semantic structure more effectively than a Single-Layer LSTM, operating under the assumption that network depth is more critical than cell complexity.
- **Result:** **Hypothesis Refuted.** While the Stacked GRU performed better at lower capacities (512 units), the Single-Layer LSTM outperformed the GRU at 1024 units, achieving lower perplexity while training **2.1x faster**.

---

## Pipeline & Architecture

1. **Dataset Pipeline:** Built a custom clean corpus (~7.5M characters) from Sarah J. Maas's *Throne of Glass* series using `ebooklib` and `beautifulsoup4` for HTML tag removal and whitespace normalization.
2. **Tokenization:** Character-level tokenization (vocabulary size: 93 unique characters) to handle fantasy-specific vocabulary without Out-Of-Vocabulary (OOV) tokens.
3. **Model Implementations (`PyTorch`):**
   - **Model A (Baseline):** Single-Layer LSTM (1024 hidden units, 256 embedding dim, dropout 0.5).
   - **Model B (Challenger):** 2-Layer Stacked GRU (1024 hidden units, 256 embedding dim, dropout 0.5).

---

## 📊 Quantitative Results

All models were evaluated under identical training conditions (Adam optimizer, LR 0.001, gradient clipping, 10 epochs, 90/10 train/val split).

| Model Configuration | Architecture | Hidden Units | Training Time | Final Val Loss | Perplexity |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **LSTM** | Single-Layer | 512 | 7.50 min | 1.3066 | 3.6936 |
| **GRU** | Stacked (2-Layer) | 512 | 9.51 min | 1.1149 | 3.0493 |
| **GRU** | Stacked (2-Layer) | 1024 | 28.42 min | 1.0708 | 2.9177 |
| **LSTM (Winner)** | Single-Layer | 1024 | **13.84 min** | **1.0563** | **2.8757** |

### Key Analytical Takeaways
- **Low vs. High Capacity:** At 512 hidden units, depth compensated for cell simplicity (GRU won). However, expanding capacity to 1024 units enabled the complex gating mechanism of the LSTM (forget, input, output gates) to outperform the deeper GRU stack.
- **Computational Efficiency:** The Single-Layer LSTM achieved higher prediction precision (Perplexity **2.87**) in half the training time (~13.8 min vs ~28.4 min).

---

## 📝 Qualitative Text Samples

Generated using the seed prompt: `"Aelin Galathynius stood before the "` (top-$k$ sampling, $k=3$):

- **Single-Layer LSTM:** Demonstrated high syntactic and grammatical stability, natural sentence length variation, and accurate punctuation formatting.
  > *"She was a birthday, she could see horrible and so much as the prisoner with her shoulders to her feet and took a step at the stone corner..."*
- **Stacked GRU:** Captured long-range entity relationships (frequently generating relevant character names like *Aedion* and *Sam*), but suffered from minor character-level typographical noise.
  > *"Aedion grabbed his hand and said, 'That’s what you said.' And then she should hear. And she’d had the sound of silence, and her fire was still crept..."*

---

## How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/noeliamate/stylistic-text-generation-lstm-vs-gru.git
   cd stylistic-text-generation-lstm-vs-gru
   ```
2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
3. **Run the Jupyter Notebook:**
   Open `text_generation_lstm_vs_gru.ipynb` in Jupyter, Google Colab, or VS Code.

---

## 👤 Author
- **Noelia** - Final-year Cybersecurity & AI Student at Universidad de Málaga (UMA).
- GitHub: [@noeliamate](https://github.com/noeliamate)