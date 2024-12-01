# README: Summarize Text From YouTube or Website Using LangChain

This **Streamlit** application is designed to extract and summarize text from YouTube videos or web pages. It uses LangChain components, Groq's **Gemma-7b-It** model, and loaders for fetching and processing content.

---

## Features:
1. **Input URL**: Accepts YouTube video links or website URLs for summarization.
2. **Groq API Integration**: Leverages the Groq **Gemma-7b-It** LLM for generating summaries.
3. **Validation**: Ensures inputs are valid URLs and API keys are provided.
4. **Friendly Interface**: Intuitive sidebar and input fields for ease of use.

---

## Dependencies:
The application uses the following Python libraries:
- **validators**: For URL validation.
- **streamlit**: For building the web app interface.
- **langchain**: For prompt templates and summarization chains.
- **langchain_groq**: For Groq API integration.
- **langchain_community.document_loaders**: For extracting content from YouTube and websites.

---

## How to Use:
1. Clone this repository.
2. Install dependencies:
   ```bash
   pip install streamlit langchain langchain_groq validators
   ```
3. Run the application:
   ```bash
   streamlit run app.py
   ```
4. Enter the following:
   - **Groq API Key**: Required to access the Gemma model.
   - **URL**: Paste a valid YouTube or website URL.
5. Click **"Summarize the Content from YT or Website"** to generate the summary.

---

## Code Walkthrough:

### Page Configuration:
- Sets the app title and page icon for a polished user experience:
  ```python
  st.set_page_config(page_title="LangChain: Summarize Text From YT or Website", page_icon="🦜")
  ```

### Sidebar:
- Collects the **Groq API Key** via a password-protected input field:
  ```python
  groq_api_key = st.text_input("Groq API Key", value="", type="password")
  ```

### Input Fields:
- Accepts URLs with input validation:
  ```python
  generic_url = st.text_input("URL", label_visibility="collapsed")
  if not validators.url(generic_url):
      st.error("Please enter a valid URL.")
  ```

### Summarization Workflow:
1. **Loader**: Detects if the input is a YouTube video or a general web page.
   - For YouTube:
     ```python
     loader = YoutubeLoader.from_youtube_url(generic_url, add_video_info=True)
     ```
   - For websites:
     ```python
     loader = UnstructuredURLLoader(urls=[generic_url], ssl_verify=False, headers={...})
     ```
2. **Chain**: Loads a summarization chain using LangChain:
   ```python
   chain = load_summarize_chain(llm, chain_type="stuff", prompt=prompt)
   output_summary = chain.run(docs)
   ```
3. Displays the summarized content.

---

## Error Handling:
- Provides error messages for missing inputs or invalid URLs.
- Displays detailed exceptions to help debug issues.

---

## Future Enhancements:
- Support for multiple languages.
- Expanded output formats (e.g., PDF or text file downloads).
- Additional summarization customization (word count, tone, etc.).

---

## Example Output:
1. **Input**:
   - URL: `https://www.youtube.com/watch?v=example`
   - Groq API Key: (your key)
2. **Output**:
   ```
   Summary: This video covers the essential concepts of X, explaining the importance of Y with clear examples...
   ```

---

Feel free to contribute enhancements or report bugs via the repository's issue tracker!