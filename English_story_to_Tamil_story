import streamlit as st
from PyPDF2 import PdfReader
from googletrans import Translator
from gtts import gTTS
import tempfile

def extract_text_from_pdf(file):
    pdf_reader = PdfReader(file)
    text = ""
    for page in pdf_reader.pages:
        text += page.extract_text()
    return text

def translate_text_to_tamil(text):
    translator = Translator()
    translation = translator.translate(text, dest='ta')
    return translation.text

def narrate_text_in_tamil(text):
    tts = gTTS(text, lang='ta')
    with tempfile.NamedTemporaryFile(delete=False, suffix='.mp3') as temp_file:
        tts.save(temp_file.name)
        return temp_file.name

st.title("Story Narration in Tamil")


num_files = st.number_input("Enter the number of PDF files you want to upload:", min_value=1, step=1)

uploaded_files = []
for i in range(num_files):
    uploaded_file = st.file_uploader(f"Upload PDF file {i + 1}", type="pdf")
    if uploaded_file:
        uploaded_files.append(uploaded_file)

if uploaded_files:
    st.write("Uploaded files:")
    for idx, file in enumerate(uploaded_files):
        st.write(f"{idx + 1}. {file.name}")

    selected_file_index = st.selectbox("Select a file to narrate:", options=range(len(uploaded_files)))
    if selected_file_index is not None:
        selected_file = uploaded_files[selected_file_index]
        pdf_text = extract_text_from_pdf(selected_file)
        translated_text = translate_text_to_tamil(pdf_text)
        
        st.subheader("Translated Story in Tamil:")
        st.write(translated_text)
        
        narration_file = narrate_text_in_tamil(translated_text)
        
        st.audio(narration_file, format='audio/mp3')
