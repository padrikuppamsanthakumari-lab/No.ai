Python

import streamlit as st
from openai import OpenAI

st.set_page_config(page_title="My AI", page_icon="🤖")

st.title("🤖 My AI")
st.write("Welcome! I am your personal AI assistant.")

api_key = st.text_input("Enter your API key", type="password")

if "messages" not in st.session_state:
    st.session_state.messages = []

for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.write(message["content"])

prompt = st.chat_input("Ask me anything...")

if prompt:
    st.session_state.messages.append(
        {"role": "user", "content": prompt}
    )

    with st.chat_message("user"):
        st.write(prompt)

    if api_key:
        client = OpenAI(api_key=api_key)

        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {
                    "role": "system",
                    "content": "You are My AI, a helpful assistant. Help users with questions, Python and studies in simple language."
                },
                *st.session_state.messages
            ]
        )

        answer = response.choices[0].message.content

        st.session_state.messages.append(
            {"role": "assistant", "content": answer}
        )

        with st.chat_message("assistant"):
            st.write(answer)
    else:
        st.warning("Please enter your API key.")
        
