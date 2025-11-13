### Hotel Booking Management & Reviews Analysis – Project Report
#### By Roshan Nikam
### Project Overview
#### This end-to-end project analysis hotel booking trends, hotel performance, user demographics, traveller types, and review sentiments using:
•	PostgreSQL database (schema, queries, data loading) <br> •	Python for EDA, clustering, ML classification & sentiment analysis <br> •	Power BI Dashboard
The goal is to help hotels understand guest behaviour, improve service quality, identify top hotels, and extract insights from user-generated reviews.































### 1. Introduction
##### This project analyses hotel reviews data stored in a PostgreSQL database with three tables:
  #####  •	Hotels (hotel details & base ratings)
  #####  •	Users (user demographics & traveller type)
  #####  •	Reviews (ratings & review text)
##### The objective was to explore, analyse, and visualize the data to gain business insights, build machine learning models, and create an interactive dashboard.
##### Tools used: Excel,SQL,Powerbi,Python.

### 2. Data Sources
##### •	Database: PostgreSQL
##### •	Tables:
##### hotels: hotel details, star rating, base scores (cleanliness, comfort, facilities, etc.), geo-location
##### users: user demographics, gender, country, traveller type, join date
##### reviews: user-hotel reviews with text & numerical ratings

### 3. Data Preparation
##### •	Excel: Performed initial cleaning (removed duplicates, checked missing values).
##### •	SQL: Wrote 20+ queries to answer business questions.
##### •	Python (Pandas):Merged hotels, users, and reviews into a single dataset.Extracted features (year, month, day name from review date).

### 4. SQL Analysis Insights:
##### Some Insights:
•	User id’s 710,1489,1042,588,1752 these are youngest users to write reviews.
•	Most users belong from United state then followed by United Kingdom, Germany.
•	In traveller type most common users belong to couple followed by family then solo, business.
•	The golden oasis hotel has 9.92 highest overall score then canal house grand, marina bay zenith
•	Tokyo city stands first in score cleanliness 
•	User ID 1758 wrote review more than five times.
•	Hotel "Tango Boutique” got most frequently reviews.
•	The Savannah House hotel received most rating by family travel type.
•	"The Gateway Royale" this hotel got good score from foreigners compared to locals.
•	City of Amsterdam Hotel Canal House Grand got rant first for overall score.
##### Like this there is so many insights.
#### Here Some SQL Queries:


<img width="608" height="400" alt="image" src="https://github.com/user-attachments/assets/348525c2-c78c-437c-870b-978859ab7fc0" />


<img width="608" height="345" alt="image" src="https://github.com/user-attachments/assets/bfb15871-036d-40c9-a2e8-6409fff1dc89" />


<img width="703" height="351" alt="image" src="https://github.com/user-attachments/assets/e80ddc8a-d9c7-4cab-9c02-49b0eaf0fd86" />


<img width="706" height="382" alt="image" src="https://github.com/user-attachments/assets/95ee77f3-c2dd-412a-b601-f1333a39564e" />


<img width="704" height="328" alt="image" src="https://github.com/user-attachments/assets/9a9b18f1-bcec-4a00-a48a-7a5fbe0ed78c" />


<img width="707" height="337" alt="image" src="https://github.com/user-attachments/assets/d6158319-c79a-4b60-86ae-2d27658f69cd" />


### 5. Power BI Dashboard
###### A single dashboard was designed with the following visuals:
•	Hotel performance overview
•	Geographical map
•	Traveller type analysis
•	Review trends
•	Value for money comparison
•	Gender comparison
•	Top 10 hotels 
•	Users rushed by months 
•	Using aged group wise analysis 
•	Monthly comparisons 



<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/7e4709dc-6972-456f-9d1c-709adb6e2ae4" />



## 6. Python Analysis & Machine Learning
##### •	Exploratory Data Analysis (EDA) here some reports:
##### •	Histograms & boxplots for score overall.


<img width="789" height="548" alt="image" src="https://github.com/user-attachments/assets/31406156-670a-4bc9-8158-e95370555e6c" />


##### •	Word cloud of most frequent words in reviews.


<img width="798" height="444" alt="image" src="https://github.com/user-attachments/assets/502ce15d-b0f3-4b8c-b041-4548ed653884" />


##### •	Correlation heatmap between base ratings & actual scores.


<img width="803" height="467" alt="image" src="https://github.com/user-attachments/assets/d9fcc048-ec77-4e5a-aeac-57bec5637080" />



### •	Machine Learning Models:
##### •	Clustering: KMeans clustering of hotels based on review scores → grouped hotels into performance categories.


 <img width="776" height="379" alt="image" src="https://github.com/user-attachments/assets/e6300749-3db3-424f-8432-01e22d1807fc" />



##### •	Sentiment Analysis: Applied VADER/TextBlob on review_text to classify reviews as Positive/Negative/Neutral.


 <img width="765" height="299" alt="image" src="https://github.com/user-attachments/assets/992de9d8-aa86-457d-af43-3868b71a30db" />


##### •	Classification: Predict  traveller type gives higher ratings.


 <img width="765" height="473" alt="image" src="https://github.com/user-attachments/assets/4887e8e8-0724-4f7b-82ef-4f4d4c307f28" />


### 7. Tools & Technologies
##### •	Database: PostgreSQL
##### •	Data Cleaning: Excel, Pandas
##### •	Querying & Analysis: SQL, Python (Pandas, Scikit-learn, NLTK, Surprise)
##### •	Visualization: Power BI, Matplotlib, Seaborn, WordCloud, Plotly
##### •	Machine Learning: Linear Regression, Logistic Regression, KMeans, SVD (collaborative filtering)
________________________________________
### 8. Conclusion
This project demonstrates how structured hotel data can be combined with user reviews to provide:
•	Business insights
•	Customer satisfaction analysis
•	Predictive modeling
•	Interactive dashboards
##### The work highlights the value of integrating SQL + Python + Excel + Power BI for full-stack data analysis.


