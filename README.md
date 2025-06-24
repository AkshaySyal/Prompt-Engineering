# Prompt-Engineering
Performed prompt engineering on a dialogue summarization task using the Flan-T5 model and the dialogsum dataset, exploring the impact of different prompts, zero-shot and few-shot inferences, and generative configuration parameters on the model's output

# Problem Statement
1. Set up Required Dependencies
  - First, the necessary Python libraries were installed. This specifically included the datasets library, which was installed using a pip install command. Additionally, modules such as AutoModelForSeq2SeqLM, AutoTokenizer, and GenerationConfig from transformers, along with load_dataset from datasets, were imported to facilitate interaction with the models and data.

2. Explore the Dataset
  - The knkarthick/dialogsum dataset was loaded to begin the process. To gain an understanding of its content, several dialogues from the dataset, along with their corresponding baseline human summaries, were printed for examination. 

3. Summarize Dialogues without Prompt Engineering
  - Subsequently, the Flan-T5-large model and its associated tokenizer were loaded. The pre-trained model was then used to summarize the example dialogues without any explicit prompt engineering. The model.generate() function was called with max_new_tokens=50. The generated summaries in this stage were notably distinct from the human baselines. For Example 1, the generated summary was <pad> #Person1: Ms. Dawson, please take dictation for me.</s>. For Example 2, it was <pad> #Person1#: Thank you, #Person2#.</s>, and for Example 3, <pad> #Person1#: That's great.</s>. It was observed that while these generations made some sense, the model did not appear to grasp the summarization task and frequently generated what seemed to be the next sentence in the dialogue. This outcome underscored the critical need for prompt engineering.

4. Summarize Dialogues with Instruction Prompts (Zero-Shot Inference)
  - This section introduced zero-shot inference, a technique where the dialogue is converted into an instruction prompt to guide the model towards a specific task like summarization. The dialogues were framed within descriptive instructions, such as "Summarize the following conversation:". This approach led to improved summaries compared to the unprompted attempts, although the model still did not fully capture the nuance of the conversations.
    
5. Summarize Dialogues with a Few-Shot Inference
  - This step involved implementing few-shot inference, also known as "in-context learning". This technique provides the Large Language Model (LLM) with several examples of prompt-response pairs that exemplify the desired task before the actual prompt that needs completion. This process aims to put the model into a state where it understands the specific task better. A function named make_prompt was developed to construct a prompt that included in-context examples followed by the test example, with each section separated by "\n\n\n". The in-context examples' prompts also specified a word limit of "less than 50 words". 

6. Generative Configuration Parameters for Inference
  - Finally, the GenerationConfig class was employed to adjust the parameters of the generate() method, thereby influencing the LLM's output. The max_new_tokens=50 parameter was consistently used to define the maximum number of tokens to generate. It was noted that setting do_sample = True activates various decoding strategies, allowing for output adjustment using parameters like temperature, top_k, and top_p. Three distinct GenerationConfig objects (config1, config2, config3) were defined and applied to the same few-shot prompt for test_example_index = 800.
    - config1 utilized max_new_tokens=50, do_sample=True, temperature=0.2, and top_k=20.
    - config2 incorporated max_new_tokens=50, do_sample=True, temperature=0.2, top_p=0.85, and num_beams=3.
    - config3 set max_new_tokens=50, do_sample=False, num_beams=4, num_beam_groups=2, and diversity_penalty=0.2.
    The summaries generated with these different configurations for the same input showed variations, demonstrating the impact of these generative parameters on the model's output. A warning message was also observed indicating that both max_new_tokens and max_length appeared to be set, with max_new_tokens taking precedence
