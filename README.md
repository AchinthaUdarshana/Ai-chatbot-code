!pip install langchain -qU //import this lib
!pip install langchain_community -qU
!pip install langchain_google_genai -qU
!pip install google-ai-generativelanguage

from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model='gemini-1.5-flash', // Ai model vashan
    api_key = 'API-KEY', //genaret tha api key
    max_output_tokens=500,
    temperature=0.8,
)

from langchain_core.prompts import ChatPromptTemplate
from langchain_core.prompts import MessagesPlaceholder
from langchain_core.messages import SystemMessage
from langchain_core.messages import HumanMessage
from langchain_core.messages import AIMessage

prompt = ChatPromptTemplate(
    [
        SystemMessage(content="""You are an intelligent chatbot."""),
        MessagesPlaceholder(variable_name="history"),
        MessagesPlaceholder(variable_name="question")
    ]
)

history = [
    HumanMessage(content="Hi! I'm Achintha"),
    AIMessage(content="How can I help you Achintha?")
]

from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()
chain = prompt | llm | parser

while True:
  question = input("User: ")

  if(question in ('bye','Bye')):
    print("AI Bot: Okay. Good Bye! Come again.")
    break;
  result = chain.invoke({
      "question":[HumanMessage(content=question)],
      "history":history
  })

  history.extend(
      [HumanMessage(content=question),
      AIMessage(content=result)]
  )

  print("AI Bot: ",result)
