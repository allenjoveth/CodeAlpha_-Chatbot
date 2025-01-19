# Implementation of Chatbot:

# AIM:
To develop a simple and interactive chatbot that can answer frequently asked questions (FAQs) based on user input using Natural Language Processing (NLP).


## Design Steps:

### Step 1:
Import necessary libraries like nltk, sklearn, and tkinter.

### Step 2:
Download essential NLTK resources such as tokenizers and stopwords for text preprocessing.

### Step 3:
Preprocess the input text by removing stopwords and tokenizing.

### Step 4:
Use the TfidfVectorizer to convert FAQs into vectors and compare the user input with these vectors using cosine_similarity.

### Step 5:
Create a GUI using tkinter, allowing the user to interact with the chatbot.

### Step 6:
Display the chatbot's responses to user inputs.


## Program 
```
# Importing necessary Libraries

import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import tkinter as tk
from tkinter import messagebox


# Download necessary NLTK resources

nltk.download('punkt')
nltk.download('stopwords')


# FAQs and their answers

faqs = {
    "What is CodeAlpha?": "CodeAlpha is a hands-on internship platform offering students real-world tech opportunities.",
    "How can I join the CodeAlpha internship?": "Visit the CodeAlpha website, browse internships, and apply online with a strong resume.",
    "What are the benefits of the internship?": "Gain practical experience, work on projects, and build a professional network to boost your career.",
    "Is there a certificate provided?": "Yes, you'll receive a certificate upon successful completion of the internship.",
    "How can I contact CodeAlpha for more details?": "Email internships@codealpha.com or visit the CodeAlpha website for details."
}


# Preprocess user input

def preprocess(text):
    tokens = word_tokenize(text.lower())
    stop_words = set(stopwords.words('english'))
    filtered_tokens = [word for word in tokens if word.isalnum() and word not in stop_words]
    return ' '.join(filtered_tokens)


# Chatbot function with enhanced GUI

def chatbot():
    def get_response():
        user_input = input_field.get()
        if user_input.lower() == 'exit':
            root.destroy()
            return

        user_input_preprocessed = preprocess(user_input)
        user_vector = vectorizer.transform([user_input_preprocessed])


        # Calculate similarity with FAQ questions

        similarities = cosine_similarity(user_vector, faq_vectors).flatten()
        best_match_index = similarities.argmax()

        if similarities[best_match_index] > 0.1:
            best_match_question = list(faqs.keys())[best_match_index]
            response = faqs[best_match_question]
        else:
            response = "I didn't quite catch that. Could you rephrase your question?"

        chat_history.configure(state=tk.NORMAL)
        chat_history.insert(tk.END, f"You: {user_input}\n", 'user')
        chat_history.insert(tk.END, f"ALLEN'S CHATBOT: {response}\n", 'bot')
        chat_history.configure(state=tk.DISABLED)
        chat_history.yview(tk.END)
        input_field.delete(0, tk.END)


    # Setting up the GUI

    root = tk.Tk()
    root.title("ALLEN'S CHATBOT")
    root.geometry("600x700")
    root.configure(bg="#2B2D42")

    header = tk.Label(root, text="ALLEN'S CHATBOT", font=("Times New Roman", 26, "bold"), bg="#4A4E69", fg="#F2E9E4", pady=10)
    header.pack(fill=tk.X, pady=5)

    chat_frame = tk.Frame(root, bg="#EDF2F4", bd=2, relief=tk.RIDGE)
    chat_frame.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)

    chat_history = tk.Text(chat_frame, bg="#FFFFFF", fg="#2B2D42", font=("Courier New", 12), wrap=tk.WORD, state=tk.DISABLED, relief=tk.FLAT, padx=10, pady=10)
    chat_history.pack(padx=5, pady=5, fill=tk.BOTH, expand=True)
    chat_history.tag_config('user', foreground="#1D3557", font=("Courier New", 12, "bold"))
    chat_history.tag_config('bot', foreground="#2A9D8F", font=("Courier New", 12))

    input_frame = tk.Frame(root, bg="#2B2D42")
    input_frame.pack(fill=tk.X, pady=10)

    input_field = tk.Entry(input_frame, font=("Courier New", 14), bg="#FFFFFF", fg="#2B2D42", insertbackground="#2B2D42", width=40, relief=tk.GROOVE)
    input_field.grid(row=0, column=0, padx=5, pady=5, sticky="we")

    send_button = tk.Button(input_frame, text="Send", command=get_response, font=("Helvetica", 12, "bold"), bg="#4A4E69", fg="#F2E9E4", activebackground="#6A8EAE", relief=tk.RAISED)
    send_button.grid(row=0, column=1, padx=5, pady=5)

    input_frame.columnconfigure(0, weight=1)


    # Initialize the vectorizer and FAQ vectors

    vectorizer = TfidfVectorizer()
    faqs_preprocessed = [preprocess(question) for question in faqs.keys()]
    faq_vectors = vectorizer.fit_transform(faqs_preprocessed)

    root.mainloop()


# Run the chatbot

if __name__ == "__main__":
    chatbot()
```


## Output:

![Screenshot 2025-01-19 190113](https://github.com/user-attachments/assets/69aae762-34ef-4236-9761-4bab7e7c934f)


## Result:

The  Chatbot was successfully implemented, providing accurate and context-aware responses to frequently asked questions, offering users an interactive and intuitive experience.

