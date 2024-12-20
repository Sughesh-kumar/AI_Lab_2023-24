# Ex.No: 13 Fake News Detector 
### DATE: 24/10/2024                                                                           
### REGISTER NUMBER : 212222060261
### AIM: To write a program to train the classifier for detecting fake news using machine learning techniques.

###  Algorithm:
Data Collection:

The dataset used for this task consists of labeled news articles, classified as either "fake" or "true". The data is divided into two categories: Fake News and True News.
The dataset is loaded using Pandas and preprocessed to ensure clean and usable data.
Data Preprocessing:

Convert all text to lowercase to maintain uniformity.
Remove any punctuation marks from the text.
Tokenize the text into words for further processing.
Feature Extraction:

Use CountVectorizer to convert text into a numeric form (Bag-of-Words representation).
This helps the model understand the frequency of terms in the news articles.
Model Training:

Train a Multinomial Naive Bayes classifier using the processed text data.
Split the data into training and testing sets using an 80-20 split.
Train the model on the training set and evaluate it on the test set.
Model Evaluation:

Measure the model's accuracy on both the training and testing datasets.
Display the accuracy of the classifier to evaluate its performance.
Prediction:

Once the model is trained, it is used to predict whether a given news article is "Fake News" or "True News".

### Program:
```
# Install necessary library (if not installed)
!pip install gradio

# Import necessary libraries
import gradio as gr
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score
import string

# Load datasets
fake_data = pd.read_csv('/content/Fake.csv')
true_data = pd.read_csv('/content/True.csv')

# Label datasets
fake_data['label'] = 0  # Fake news labeled as 0
true_data['label'] = 1  # True news labeled as 1

# Combine and shuffle datasets
data = pd.concat([fake_data, true_data], ignore_index=True)
data = data.sample(frac=1).reset_index(drop=True)

# Preprocessing function
def preprocess_text(text):
    text = text.lower()  # Convert to lowercase
    text = ''.join([char for char in text if char not in string.punctuation])  # Remove punctuation
    return text

# Apply preprocessing to the text data
data['text'] = data['text'].apply(preprocess_text)

# Split data into features and labels
X = data['text']
y = data['label']

# Split the dataset: 80% for training, 20% for testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Vectorize text data using CountVectorizer
vectorizer = CountVectorizer(stop_words='english')
X_train_vec = vectorizer.fit_transform(X_train)
X_test_vec = vectorizer.transform(X_test)

# Train model (Naive Bayes)
model = MultinomialNB()
model.fit(X_train_vec, y_train)

# Predict on both training and test sets
y_train_pred = model.predict(X_train_vec)
y_test_pred = model.predict(X_test_vec)

# Calculate accuracy for both training and test sets
train_accuracy = accuracy_score(y_train, y_train_pred)
test_accuracy = accuracy_score(y_test, y_test_pred)

print(f'Training Accuracy: {train_accuracy:.2f}')
print(f'Test Accuracy: {test_accuracy:.2f}')

# Prediction function for Gradio
def predict_fake_news(news_article):
    # Preprocess the input text
    news_article = preprocess_text(news_article)
    
    # Vectorize the input text
    news_vec = vectorizer.transform([news_article])
    
    # Make the prediction
    prediction = model.predict(news_vec)
    
    # Return the prediction label
    return "Fake News" if prediction == 0 else "True News"

# Create Gradio interface
iface = gr.Interface(fn=predict_fake_news, 
                     inputs=gr.Textbox(lines=3, placeholder="Enter news article..."), 
                     outputs="text", 
                     live=True)

# Launch the interface
iface.launch()
```

### Output:
![WhatsApp Image 2024-11-13 at 16 52 04_b9f29c09](https://github.com/user-attachments/assets/666dee01-da86-4014-8f56-d3aa99800e44)



### Result:
Thus the system was trained successfully and the prediction was carried out.
