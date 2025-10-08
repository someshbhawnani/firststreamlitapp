#streamlit and LLMs test
from openai import OpenAI
import streamlit as st
import pandas as pd

st.title ("⚕️ Welcome to MediBot ⚕️")
st.subheader("🤖 Your Reliable Medical Assistant 🤖", divider="blue")
st.text("I am here to help, please provide the details below accurately.")
st.text("None of your personal information will be saved, don't worry.")

#parameters to be used in prompt later
forwho = st.selectbox('You here for self or others?',('Self','Others'), index=None)
age = st.selectbox("Age of patient", ('<10 years','>10,<50 years','> 50 years'), index=None)
#country = st.text_input("Country of residence", key="Country")
language = st.text_input("Preferred Output Language", key="Language")
allery = st.selectbox("Are you allergic to any medication?", ('Yes','No'), index=None)

st.sidebar.markdown("Some Important Notes")
st.sidebar.divider()

client = OpenAI(
 base_url = "https://api.deepseek.com",
 api_key = st.secrets["DEEPSEEK_API_KEY"]
  )

if "openai_model" not in st.session_state:
 st.session_state["openai_model"] = "deepseek-chat"

if "messages" not in st.session_state:
 st.session_state.messages = []

for message in st.session_state.messages:
 with st.chat_message(message["role"]):
  st.markdown(message["content"])

if prompt := st.chat_input("Relax, I am here to help, type your query..."):
 st.session_state.messages.append({"role": "user", "content": prompt})

 with st.chat_message("user"):
  st.markdown(prompt)

  with st.chat_message("assistant"):
   stream = client.chat.completions.create(
    model=st.session_state["openai_model"],
    messages=[
        {"role": m["role"], "content": m["content"]}
        for m in st.session_state.messages
    ],
    stream=True,
   )
   response = st.write_stream(stream)

st.text("I am an AI and not a substitute for a doctor.I would not be able to produce 100% correct results which will work, I just take the information of previous medical reports and their medicines. Best if you consult a doctor if any serious case that might kill you")

outputtxt=st.text_area("This is the output text which i wish to save in a file")

st.download_button(
 label='Download the results in a text file',
 data=outputtxt,
 file_name="medibot.txt",
 mime="text/plain"
)

st.write("Click above button to download details as text file")
