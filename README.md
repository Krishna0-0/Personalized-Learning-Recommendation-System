# Personalized-Learning-Recommendation-System
Created a mock database schema to store student information like strengths, weaknesses, interests &amp; preferences. Designed an algorithm that uses the mock database to suggest workshops, AI generated daily task and match the student with a potential mentor based on students’ availability, learning preferences and weaknesses. 

Step-by-Step Documentation of the Code

1. Importing Libraries
SQLite3: Used for connecting to and interacting with an SQLite database.
NumPy and Pandas: Used for handling arrays and dataframes respectively, providing efficient data structures.
NLTK: Provides tools for natural language processing tasks such as tokenization, stemming, stopword removal, etc.
FuzzyWuzzy: Used for approximate string matching, which helps in synonym detection and string comparison.
Sentence-Transformers: Used for converting text into vector representations (embeddings) to calculate semantic similarity between texts.
Spacy: For processing natural language text, including tokenization, part-of-speech tagging, and named entity recognition.

2. Database Connection (connect_to_database)
Function Purpose: This function connects to the SQLite database (students_mentors_workshops.db) where data for students, mentors, and workshops is stored.
Output: It returns a database connection object (conn) that is used for executing SQL queries.

3. Getting Student Input (get_new_student_input) 
Function Purpose: Collects new student data through user input (like name, strengths, weaknesses, interests, etc.), inserts this data into the Students table in the database, and fetches the newly inserted student record.
Output: The function returns a Pandas DataFrame (students_df) containing the student's information.

4. Preprocessing Text (preprocess_text)
Function Purpose: This function is used to preprocess text by converting it to lowercase, tokenizing the text into individual words, and removing common stopwords (e.g., "the", "and", "is").
Usage: This function is called on both mentor and workshop profiles to clean the text before applying further NLP techniques.

5. Text Embedding using Sentence-BERT (process_text_with_sentence_bert)
Function Purpose: This function converts input text into vector representations (embeddings) using the Sentence-BERT model.
Usage: The model (paraphrase-MiniLM-L6-v2) generates a dense vector representation for each input text, which captures the semantic meaning of the text.
Output: A vector representing the semantic content of the text.

6. Cosine Similarity (cosine_sim)
Function Purpose: Computes the cosine similarity between two vectors (vec1 and vec2). This measures the cosine of the angle between the two vectors in a multi-dimensional space. The closer the value is to 1, the more similar the two vectors are.
Usage: This is used to measure the similarity between student profiles and mentor/workshop profiles.

7. Synonym Matching (synonym_match)
Function Purpose: This function uses WordNet (from NLTK) to check if two words are synonyms. If they are semantically similar based on WordNet's similarity score, it returns True.
Usage: It is used to compare words in the student’s strengths/weaknesses/interests with workshop focus areas to find semantic matches.

8. Calculating Recommendations (calculate_recommendations)
This is the central function that drives the recommendation process, providing personalized suggestions for mentors, workshops, and tasks based on the student’s profile.
Steps:
Fetching Student Data:
Retrieves the student profile from the database using the student_id.
Combines strengths, weaknesses, and interests into a single string (student_profile), which is then encoded into a vector using Sentence-BERT.
Mentor Recommendations:
Fetches all mentor profiles and preprocesses their expertise and interests.
For each mentor, calculates the cosine similarity between the student profile and the mentor profile.
Filters mentors based on the student's availability.
Sorts mentors based on similarity and returns a DataFrame with the recommended mentors.
Workshop Recommendations:
Fetches all workshops from the database.
Preprocesses the workshop focus areas and encodes them into vectors using Sentence-BERT.
Calculates the cosine similarity between the student profile and the workshop profiles.
Boosts similarity based on the student’s weaknesses, preferred topics, and interests (using synonym matching).
Sorts workshops based on similarity score and returns the recommended workshops.
Task Recommendations:
Uses predefined tasks (e.g., "Complete a robotics quiz", "Write a productivity journal").
Calculates the cosine similarity between the student profile and each task description.
Sorts tasks based on similarity score and returns the recommended tasks.

9. Running the Program
This block is the entry point of the program.
Connects to the Database: Calls connect_to_database.
Gets New Student Input: Collects data from the user and stores it in the database.
Calculates Recommendations: Based on the student's profile, the system provides mentor, workshop, and task recommendations.
Prints Results: The top recommendations are displayed for each category.
Closes the Database: Ensures the database connection is properly closed at the end.



