import streamlit as st
import re
from collections import Counter
import logging
import nltk
from nltk.tokenize import sent_tokenize

# Download NLTK data
nltk.download("punkt")

# Suppress warnings
logging.getLogger("root").setLevel(logging.ERROR)

# Streamlit App Configuration (Must be at the top)
st.set_page_config(page_title="Text Analysis", page_icon="📝")

# Maintain user input state
if "user_input" not in st.session_state:
    st.session_state.user_input = ""

if "submitted" not in st.session_state:
    st.session_state.submitted = False

# Function to analyze text
def analyze_text(text):
    # Character count
    char_count = len(text)

    # Word count (case-insensitive)
    words = [word.lower() for word in text.split()]
    word_count = len(words)

    # Sentence count (better accuracy using NLTK)
    sentences = sent_tokenize(text)
    sentence_count = len(sentences)

    # Count repeated words
    word_freq = Counter(words)
    single_word_repeated = {word: count for word, count in word_freq.items() if count == 2}
    double_word_repeated = {f"{words[i]} {words[i+1]}": words[i:i+2].count(f"{words[i]} {words[i+1]}") for i in range(len(words)-1)}
    triple_word_repeated = {f"{words[i]} {words[i+1]} {words[i+2]}": words[i:i+3].count(f"{words[i]} {words[i+1]} {words[i+2]}") for i in range(len(words)-2)}

    # Filter results for repeated words
    single_word_repeated = {k: v for k, v in single_word_repeated.items() if v > 1}
    double_word_repeated = {k: v for k, v in double_word_repeated.items() if v > 1}
    triple_word_repeated = {k: v for k, v in triple_word_repeated.items() if v > 1}

    return char_count, word_count, sentence_count, single_word_repeated, double_word_repeated, triple_word_repeated

# First Page (Text Input)
if not st.session_state.submitted:
    st.title(" Enter Your Text Below")
    
    # Text input box
    user_input = st.text_area("Enter your paragraph:", value=st.session_state.user_input, height=250)
    
    # Buttons
    col1, col2 = st.columns([1, 1])
    
    with col1:
        if st.button("Clear"):
            st.session_state.user_input = ""
            st.session_state.submitted = False
            st.rerun()

    with col2:
        if st.button("Submit"):
            st.session_state.user_input = user_input
            st.session_state.submitted = True
            st.rerun()

# Second Page (Analysis Page)
else:
    st.title("Text Analysis")

    # Get the analysis data
    char_count, word_count, sentence_count, single_word_repeated, double_word_repeated, triple_word_repeated = analyze_text(st.session_state.user_input)
    
    # Display results
    st.write(f" *Total Characters:* {char_count}")
    st.write(f" *Total Words:* {word_count}")
    st.write(f" *Total Sentences:* {sentence_count}")

    st.subheader("Repeated Single Words")
    if single_word_repeated:
        st.json(single_word_repeated)
    else:
        st.write("No single word repeated.")

    st.subheader("Repeated Double Words")
    if double_word_repeated:
        st.json(double_word_repeated)
    else:
        st.write("No double words repeated.")

    st.subheader("Repeated Triple Words")
    if triple_word_repeated:
        st.json(triple_word_repeated)
    else:
        st.write("No triple words repeated.")

    # Back Button
    if st.button("Go Back"):
        st.session_state.submitted = False
        st.rerun()
