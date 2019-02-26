# Diagnose and Explain
This repository contains code for the paper: **"Diagnose and Explain: A Multi-Level Attention Model for Automated Radiology Reporting from Chest X-Ray"** (will update with link when a preprint is available)

Our model takes a Chest X-ray image as input and generates a complete radiology report using Natural Langugage. The generated report contains 2 sections:
* **Findings:** observations regarding each part of the chest examined. Generally a paragraph with 6+ sentences.
* **Impression:** generally a one-sentence diagnostic based on findings reported. Can contain multiple sentences.

### Sample
Ground truth on the left, model output on the right.
<image src="samples/ground_truth_3707.png" width="300px"/> <image src="samples/generated_3707.png" width="300px"/>

### Visual Attention Plot
* Findings
  <image src="samples/findings_attention_plot_3707.png" width="300px"/>
* Impression
  <image src="samples/impression_attention_plot_3707.png" width="300px"/>
  
## Dataset
We trained our model on the Indiana University [Chest X-Ray collection](https://openi.nlm.nih.gov/faq.php). The dataset
comes with **3955** chest radiology reports from various hospital systems and **7470** associated chest x-rays 
(most reports are associated with 2 or more images representing frontal and lateral views).

## Model architecture
Our model features a CNN-LSTM with a CNN Encoder, Sentence Encoder (attention-based), Paragraph Encoder (attention-based) and Word Decoder (visual attention). The LSTM is an hierarchical RNN that generates paragraphs (findings) sentence by sentence, and uses an attention model to
encode each sentence. The "most important" words in the previous sentence are used as a semantics context in the generation of each word of the next sentence. 

We use the same approach for generating sentences of the "impression" section with the only difference that we encode the "findings" paragraph using attention. This is used to guide as a context for generating impression sentences.

More details on our model architecture and proposed approach will be present in the soon-to-be-released preprint of our paper.

## Training on Cloud TPU
Our code was designed for training on Google Cloud TPU.

* Head over to TensorFlow's quickstart on [setting up a TPU instance](https://cloud.google.com/tpu/docs/quickstart) to get started with running models on Cloud TPU.
* Clone this repository and `cd` into directory 
  ```
	git clone https://github.com/wisdal/diagnose-and-explain && cd diagnose-and-explain
  ```
* Start training
  ```
    export STORAGE_BUCKET=<Your Storage Bucket>
    python main.py --tpu=$TPU_NAME --model_dir=${STORAGE_BUCKET}/tpu --train_steps=20 --iterations_per_loop=100 --batch_size=512
  ```
  
## Acknowledgements
Parts of these experiments were possible thanks to the TensorFlow Research Cloud Program which offers free TPUs for training and running models on Google Cloud for a limited period of time.