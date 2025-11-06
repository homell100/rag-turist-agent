# Introduction:
Turist Support is a chatbot designed to enchange people's experience while searching for turist experiences around the area of Penedés, Spain. Leveraging a langgraph-driven workflow, a turist agent accesses the official turist guide while supported by a weather API.

# Features:
- Multiconversational chatbot
- Agent supported by RAG system feeded with local official guides
- Agent supported with local weather forecasts

# How it works:
1. Agent receivex messsage from user
2. A first llm refines the prompt to enhance search with the RAG system.
3. The RAG system extracts the most useful information from the official guide and delivers it to a second llm.
4. This second llm  decides if it is necessary to use the weather tool
5. With all the information, the second llm provides the user with a proper answer

# System worflow

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embedded/a650924b-44d1-4024-be98-186dfa426f40" id="Obw6gmoZ7G57"></iframe></div>

# How to Run:
1.  Clone this repository.
2.  Create a virtual environment: `python -m venv venv`
3.  Activate it: `source venv/bin/activate` for Linux or `venv\Scripts\activate` for Windows
4.  Install dependencies: `pip install -r requirements.txt`
5.  Create a `.env` file in the root folder and add your API key (either Google or OpenAI):
    `GOOGLE_API_KEY="your_api_key_here"`
6.  Download the PDF from [here](https://www.gencat.cat/territori/informacio_publica/PTP_Penedes_AP/A_09_Turisme_Penedes.pdf] and save it in the root folder as `data.pdf`)
7.  Run the cells in `main.ipynb` from top to bottom.

