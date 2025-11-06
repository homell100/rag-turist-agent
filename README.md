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

<img width="545" height="946" alt="Captura" src="https://github.com/user-attachments/assets/a756cbaa-d148-462f-a91d-c9d2d0c37e5b" />


# Intructions:
Source of information (save in pdf format, inside main folder with name data.pdf): https://www.gencat.cat/territori/informacio_publica/PTP_Penedes_AP/A_09_Turisme_Penedes.pdf

# Solution design:
The goal of this project was to design a functional agent which would assist us regarding questions about a turist area. To be able to provide such turist information to the agent we have built a RAG system supported on a pdf with extensive turist information about the area of Penedes (An area of Spain). The agent is stateful and conversational, created using LangGraph. 

The default state of LangChain is improved by providing a way of remembering the context retrieved by the RAG, as it is fundamental to allow us to mesure the quality of the responses, and the transformed version of the user’s original prompt for better usage within the RAG system.

The graph is mainly composed of the reqriting node (assigned with transforming the original user’s prompt), the rag node (responsible of retriving useful context considering such user’s transformed prompt), agent node (the main brain of the solution) and the tool node (weather tool).

For simplicity, the weather tool has been mocked and does not make any real API call but returns different results depeneding on the day of the week, while mantaining error control.

Technical decisions:

Rag implementation:

Since the pdf information is in catalan, the model used to embed its information needed to be multilingual. In the system message that defines the role of the llm it has been stated clearly that the model has to answer in the same language as the question was posed, to enforce the llm does not choose the pdf’s language to answer the user.

FAISS was used as our vector database because of its simplicity: t is a lightweight, in-memory library, not a full database server. For our application, which would be considered to be in a development phase, this is an advantage. It does not require any setup (no separate database installation, no docker container or API key managment). Since it runs in the notebook’s memory, its provides useful speed for prototyping. The only downside is that since it works on memory, every single time the notebook is restarted, the index needs to be build again, which would be unacceptable in a production enviroment.

Three chuncks of information are retrieved by our RAG since the pdf document is detailed enough to contain useful information in specific sections, but not spread across whole sections, therefore a small k works better.

Model implementation:

To allow our RAG tool to search with greater efficiency, an extra model is used to modify the user’s original query, which sole purpose is to transform uncertain terms or references to previous concepts with their specific keywords to increase efficiency while searching in the vector database.

The system prompt used to define the role of the agent was detailed due to hallucinations where the weather tool was not used and the result was made up by the llm itself. A date is passed to allow it to search for the weather forecast in the upcoming days.

The weather tool API was mocked due to simplicity and billing issues, as the free tier models are strict enough with rate limits we did not want to add an extra overhead.

Results:

Four main metrics were used to evaluate the quality of our solution:
    • Context precision: A manual mesure comparing the amount of useful chuncks of information regarding the user prompt over the total amount of retrieved chuncks. With a final score of 43% we deduce that our RAG is returning many chuncks of information to the context which do not provide useful information for our answer.

    • Faithfulness: An additional llm was used to automatically assess whether each sentence in the agent’s answers was based in any retrieved chunk from the context. TBC
    • RAG delay: A visual clue regarding the comparing of ammount of tokens in the user prompts vs the amount of time needed for the agent to give an answer. TBC
    • Tool call precision: A mesure for the correctness of our agent regarding the need to make a tool call.
Limitations:
Future improvements:

Improve context precision: A 43% can be improved by using different chuncking strategies (i.e., lowering the amount of retrieved chuncks or the maximum threshold in chunck_size) or a better mebedding model.

Improve RAG strategy: Instead of retrieving on every message, a router node could be added to the graph, allowing the agent to decide whether to call the RAG or just answer from memory.

Improve our knowledge base: Instead of a single pdf, another source of data could be use. In suc
