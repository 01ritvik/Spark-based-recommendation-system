# Recommendation-system-using-pyspark

1. Environmental Setup 
a. Install PySpark. 
i. We can download Apache Spark on our local machine from 
https://spark.apache.org/downloads.html. 
ii. We have already downloaded this and configured with our system in 
the class using the link provided on the blackboard. 
iii. https://github.com/charlie-ph/BigDataAnalytics/blob/master/Installations
HowTos/How-To-Install-Spark-On-Windows.md 
b. Install Additional Libraries. 
i. Install pandas, matplotlib, etc into your system. 
ii. We have also used FAISS for similarity search, and if you have a GPU, use 
(pip install faiss-gpu). 
iii. If you don’t have GPU, you can install CPU version only(pip install faiss
CPU). 
c. Start Spark session in your IDE. 
i.  
ii. This is the snippet of our code which enables us to run Spark locally with the 
available cores. 
iii. To validate the components are correctly installed, run a simple pyspark code, 
iv. df = spark.createDataFrame([(1, 'foo'), (2, 'bar')], ["ID", "Data"]) 
df.show() 
v. This code should display the DataFrame without any errors if everything is 
set up correctly. 
2. How to run the code 
a. Set up Google collab (As we have used collab for our code implementation) 
i. Open Google Colab at colab.research.google.com and create a new notebook. 
b. Install Spark in Collab 
i. In the first cell, type (!pip install pyspark). 
c. Install Faiss in Collab 
i. In the next cell, type (!pip install faiss-gpu for GPU), if your system does not 
have GPU type (!pip install fiass-cpu) 
d. After downloading the files, on the left-hand side click on the file section and then 
upload the csv from the local system. 
e. Run the recommender system code. 
3. About the code 
a. Setting Up Spark Session and Reading Data 
b. Data Preparation 
i. Article IDs: A unique ID is assigned to each article using 
monotonically_increasing_id(). 
ii. User Interactions: Simulated user interactions with articles are created. This 
includes random user IDs and ratings for each article to simulate which users 
interacted with which articles and how they rated them. 
c. Text Cleaning and Tokenization 
i. Text Cleaning: Removes punctuation and converts all text to lowercase to 
standardize the input for better natural language processing. 
ii. Tokenization: Splits the cleaned text into individual words. 
iii. Stop Words Removal: Removes common words (stop words) that are 
typically uninformative in text analysis. 
d. Feature Transformation and Data Exploration 
i. Word Counting: Counts the frequency of each word across all articles, which 
can help in understanding the common vocabulary. 
ii. Interaction Summary: Summarizes user interactions per article to see which 
articles are most popular based on user engagement. 
iii. Tag Processing: Processes the 'tags' column to split it into individual tags and 
counts the frequency of each tag. 
e. Recommendation System Preparation 
i. TF-IDF: Converts the tokenized words into numerical features using Term 
Frequency-Inverse Document Frequency (TF-IDF), which reflects the 
importance of a word to an article in the corpus. 
ii. ALS Model: Trains a recommendation model using the Alternating Least 
Squares (ALS) algorithm, based on user-article interactions. This model 
predicts user preferences over articles based on past interactions. 
f. 
Visualization and Further Analysis 
i. Visualization: Visualizes the most frequent tags using a bar chart, providing 
insights into common topics or themes across the articles. 
g. Generating and Displaying Recommendations 
i. Generate Recommendations: The ALS model generates recommendations for 
each user, recommending articles that the user is likely to be interested in. 
ii. Detailing Recommendations: The recommendations are joined with the 
articles DataFrame to fetch the titles of the recommended articles, making the 
output more understandable. 
h. Using Faiss for Efficient Similarity Search 
i. Faiss Library: Uses the Faiss library for efficient similarity search in high
dimensional spaces. The code converts the TF-IDF vectors to a format that 
Faiss can use, creates an index, and adds vectors to this index. 
ii. Similarity Search: Defines a function to find similar articles using the Faiss 
index, returning the most similar articles based on their TF-IDF feature 
vectors. 
i. 
Using Kmeans algorithm for recommendation. 
i. Cluster Analysis: Applies k-means clustering to group similar articles based 
on their textual content, providing a basis for recommending similar articles 
within the same cluster. 
4. About the dataset 
a. Dataset description 
i. The dataset consists of articles collected from Medium.com, providing a 
comprehensive look into various topics published on the platform. It includes 
multiple attributes that describe the content and metadata of each article. 
b. Dataset Size: 1.04 GB 
c. Number of Records: 192,368 
d. Number of Columns: 6 
e. Attributes Description 
i. Title (title): The title of the article. Contains 5 missing values. 
ii. Text Content (text): The full text of the article. No missing values. 
iii. URL (url): The web address where the article can be accessed. No missing 
values. 
iv. Authors (authors): Names of the authors who have contributed to the article. 
No missing values. 
v. Publication Timestamp (timestamp): The date and time when the article was 
published. Contains 2 missing values. 
vi. Tags (tags): Keywords or topics associated with the article. No missing 
values. 
f. 
Data Integrity 
i. The dataset predominantly maintains a high degree of completeness with 
minimal missing data. However, there are minor discrepancies as follows: 
1. Title: There are 5 instances where the article titles are missing. This 
might impact analyses that rely on textual analysis of titles for 
insights. 
2. Publication Timestamp: There are 2 instances missing the publication 
timestamp, which may affect time-series analyses or tracking of 
content trends over time. 
This the image of the most frequently used words. 
5. Results 
▪ ALS Result: 
o Description: This method recommends articles by generating 
recommendations using a recommendation model, associating each 
recommendation with its corresponding user and rating, retrieving the titles of 
the recommended articles, and then displaying the final recommendations 
with their titles. 
▪ Faiss similarity search Result: 
o Description: Faiss is recommending articles by efficiently searching for 
articles that are closest in the feature space to the given article. In this case, the 
features used are the dense vectors obtained from the TF-IDF transformation 
of the article text. 
▪ Kmeans Result: 
o Description: The method recommends articles by leveraging clustering to 
group similar articles together and then selecting distinct articles from the 
same cluster as the input article. This assumes that articles within the same 
cluster share similar content or characteristics, making them potentially 
relevant recommendations for the user.  
To find the best k we ran a loop from 3 to 11 with an interval of 2 and using silhouette method. 
As, we can see in the graph above, k = 3, has the highest silhouette score. 
6. Comparing the results 
▪ ALS (Alternating Least Squares): 
o Precision: 0.124661 - This indicates that about 12.47% of the articles 
recommended by the ALS method are relevant. 
o Recall: 0.022324 - This shows that the ALS method is able to find about 
2.23% of all relevant articles. 
▪ TF-IDF (Term Frequency-Inverse Document Frequency): 
o Precision: 0.100000 - This means that 10% of the recommendations made 
using TF-IDF are relevant. 
o Recall: 0.000097 - This metric suggests that TF-IDF retrieves only about 
0.01% of the relevant articles. 
▪ K-Means (Clustering): 
o Precision: 0.600000 - Indicates a high precision where 60% of the articles 
recommended by the K-Means method are relevant. 
o Recall: 0.000291 - However, it has a very low recall, finding only about 0.03% 
of the relevant articles available. 
GITHUB: https://github.com/01ritvik/Recommendation-system-using
pyspark/blob/main/README.md 
