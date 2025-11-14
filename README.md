## Hotel Booking Management & Reviews Analysis – Project Report
#### _By Roshan Nikam_
**Project Overview** <br>
This end-to-end project analysis hotel booking trends, hotel performance, user demographics, traveller types, and review sentiments using:<br>•	PostgreSQL database (schema, database, queries, data loading) <br> •	Python for EDA, clustering, ML classification & sentiment analysis <br> •	Power BI Dashboard <br>The goal is to help hotels understand guest behaviour, improve service quality, identify top hotels, and extract insights from user-generated reviews.<br>
### 1. Database Design (SQL Schema).<br>Tables Created.<br>
**1. hotels.<br>**
create table hotels( <br>
 hotel_id INT PRIMARY KEY, <br>
 hotel_name VARCHAR(50), <br>
 city VARCHAR(50), <br>
 country VARCHAR(50), <br>
 star_rating INT, <br>
 lat REAL, <br>
 lon REAL, <br>
 cleanliness_base DECIMAL(10,2), <br>
 comfort_base DECIMAL(10,2), <br>
 facilities_base DECIMAL(10,2), <br>
 location_base DECIMAL(10,2), <br>
 staff_base DECIMAL(10,2), <br>
 value_for_money_base DECIMAL(10,2) <br>
); <br>
<br>
**2. users.<br>**
create table users( <br>
 user_id INT PRIMARY KEY, <br>
 user_gender VARCHAR(10), <br>
 country VARCHAR(50), <br>
 age_group TEXT, <br>
 traveller_type VARCHAR(50), <br>
 join_date DATE <br>
); <br>
<br>
**3. reviews.<br>**
create table reviews( <br>
 review_id INT PRIMARY KEY, <br>
 user_id INT REFERENCES users(user_id), <br>
 hotel_id INT REFERENCES hotels(hotel_id), <br>
 review_date DATE, <br>
 score_overall DECIMAL(10,2), <br>
 score_cleanliness DECIMAL(10,2), <br>
 score_comfort DECIMAL(10,2), <br>
 score_facilities DECIMAL(10,3), <br>
 score_location DECIMAL(10,2), <br>
 score_staff DECIMAL(10,2), <br>
 score_value_for_money DECIMAL(10,2), <br>
 review_text TEXT <br>
);<br>
### 2. Data Import (CSV Load into PostgreSQL).
COPY hotels FROM 'D:\postgres\Projects\hotel_booking\hotels.csv' DELIMITER ',' CSV HEADER; <br>COPY users FROM 'D:\postgres\Projects\hotel_booking\users.csv' DELIMITER ',' CSV HEADER; <br>COPY reviews FROM 'D:\postgres\Projects\hotel_booking\reviews.csv' DELIMITER ',' CSV HEADER; <br>
<br>
### 3. SQL Queries  overview Used in Analysis.
**1. List hotels in a specific city.** <br>
SELECT hotel_name, star_rating FROM hotels WHERE city='London';<br>
<br>
**2. Top 5 youngest users who wrote reviews.** <br>
SELECT u.user_id, u.age_group, r.review_text <br>FROM users u <br>LEFT JOIN reviews r ON u.user_id = r.user_id<br>ORDER BY u.age_group ASC <br>LIMIT 5;<br>
<br>
**3. All reviews for a given hotel.** <br>
SELECT h.hotel_name, r.review_date, r.review_text<br>FROM hotels h<br>LEFT JOIN reviews r ON h.hotel_id = r.hotel_id<br>ORDER BY r.review_date ASC;<br>
<br>
**4. Count users per country.** <br>
SELECT country, COUNT(user_id) <br>FROM users <br>GROUP BY country;<br>
<br>
**5. Most common traveler type.** <br>
SELECT traveller_type, COUNT(user_id)<br>FROM users <br>GROUP BY traveller_type<br>ORDER BY COUNT(user_id) DESC;<br>
<br>
**6. Top 10 hotels with highest average overall ratings.** <br>
SELECT h.hotel_name, AVG(r.score_overall) AS avg_score<br>FROM hotels <br>JOIN reviews r ON h.hotel_id = r.hotel_id<br>GROUP BY h.hotel_name<br>ORDER BY avg_score DESC <br>LIMIT 10;<br>
<br>
**7. City with highest cleanliness score.** <br>
SELECT h.city, AVG(r.score_cleanliness) AS avg_cleanliness<br>FROM hotels h<br>JOIN reviews r ON h.hotel_id=r.hotel_id<br>GROUP BY h.city<br>ORDER BY avg_cleanliness DESC;<br>
**8. Users who wrote more than 5 reviews.** <br>
SELECT u.user_id, COUNT(r.review_text)<br> FROM users u<br>JOIN reviews r ON u.user_id=r.user_id<br>GROUP BY u.user_id<br>HAVING COUNT(r.review_text) > 5;<br>
<br>
**9. Compare hotel star rating with actual user experience.** <br>
SELECT h.hotel_id, h.hotel_name, h.star_rating,<br>ROUND(AVG(r.score_overall),2) AS review_score,<br>(h.star_rating - ROUND(AVG(r.score_overall),2)) AS difference<br>FROM hotels h<br>JOIN reviews r ON h.hotel_id=r.hotel_id<br>GROUP BY h.hotel_id, h.hotel_name, h.star_rating<br>ORDER BY difference DESC;<br>
<br>
**10. Hotels where guests feel value for money is lower than expected.** <br>
SELECT h.hotel_id, h.hotel_name, h.value_for_money_base,<br>ROUND(AVG(r.score_value_for_money),2) AS avg_value<br>FROM hotels h<br>JOIN reviews r ON h.hotel_id=r.hotel_id<br>GROUP BY h.hotel_id, h.hotel_name, h.value_for_money_base<br>HAVING ROUND(AVG(score_value_for_money),2) < h.value_for_money_base;<br>
<br>
**11. Most frequently reviewed hotel in each country.** <br>
WITH hotel_review_count AS (<br>SELECT h.hotel_id, h.hotel_name, h.country,<br>COUNT(r.review_id) AS total_review,<br>RANK() OVER(PARTITION BY h.country ORDER BY COUNT(r.review_id) DESC) AS rank_no<br>FROM hotels h<br>JOIN reviews r ON h.hotel_id=r.hotel_id<br>GROUP BY h.hotel_id, h.hotel_name, h.country<br>
)<br>SELECT hotel_id, hotel_name, country, total_review<br>FROM hotel_review_count<br>WHERE rank_no=1;<br>
<br>
**12. Top 5 hotels highly rated by Family travelers.** <br>
SELECT h.hotel_name, u.traveller_type,<br>ROUND(AVG(r.score_overall),2) AS avg_score, <br>COUNT(r.review_id) AS total_reviews<br>FROM hotels h <br>JOIN reviews r ON h.hotel_id=r.hotel_id<br>JOIN users u ON r.user_id=u.user_id<br>WHERE u.traveller_type='Family'<br>GROUP BY h.hotel_name, u.traveller_type<br>ORDER BY avg_score DESC, total_reviews DESC LIMIT 5;<br>
<br>
**13. Compare cleanliness scores by gender.** <br>
SELECT u.user_gender, ROUND(AVG(r.score_cleanliness),2) AS avg_cleanliness<br>FROM users u <br>JOIN reviews r ON u.user_id=r.user_id<br>GROUP BY u.user_gender;<br>
<br>
**14. Hotels where foreigners rate higher than locals.** <br>
WITH score AS (<br>SELECT h.hotel_name, h.country AS hotel_country,<br> u.country AS user_country,<br>CASE WHEN h.country = u.country THEN 'local' ELSE 'foreign' END AS user_type,<br>AVG(score_overall) AS avg_score<br>FROM reviews r<br>JOIN users u ON u.user_id=r.user_id<br>JOIN hotels h ON h.hotel_id=r.hotel_id<br>GROUP BY h.hotel_name, h.country, u.country<br>
),<br>pivot AS (<br>SELECT hotel_name,<br>MAX(CASE WHEN user_type='local' THEN avg_score END) AS local_score,<br>MAX(CASE WHEN user_type='foreign' THEN avg_score END) AS foreign_score<br>FROM score<br>GROUP BY hotel_name<br>
)<br>SELECT hotel_name,<br>ROUND(local_score,2), ROUND(foreign_score,2),<br>ROUND(foreign_score-local_score,2) AS difference<br>FROM pivot<br>WHERE foreign_score > local_score<br>ORDER BY difference DESC;<br>

### 4. SQL Analysis Insights:
**Insights overview:** <br>
•	User id’s 710,1489,1042,588,1752 these are youngest users to write reviews.<br>•	Most users belong from United state then followed by United Kingdom, Germany.<br>•	In traveller type most common users belong to couple followed by family then solo, business.<br>•	The golden oasis hotel has 9.92 highest overall score then canal house grand, marina bay zenith<br>•	Tokyo city stands first in score cleanliness <br>•	User ID 1758 wrote review more than five times.<br>•	Hotel "Tango Boutique” got most frequently reviews.<br>•	The Savannah House hotel received most rating by family travel type.<br>•	"The Gateway Royale" this hotel got good score from foreigners compared to locals.<br>•	City of Amsterdam Hotel Canal House Grand got rant first for overall score.<br>
**Like this there is so many insights.**
<br>
### 5. Power BI Dashboard.
A single dashboard was designed with the following visuals:<br>•	Hotel performance overview. <br>•	Geographical map.<br> •	Traveller type analysis.<br>•	Review trends.<br>•	Value for money comparison.<br>•	Gender comparison.<br>•	Top 10 hotels.<br>•	Users rushed by months.<br>•	Using aged group wise analysis.<br>•	Monthly comparisons. <br>
<br>
This project analyzes hotel booking patterns, customer demographics, hotel performance, and user review behaviors using an interactive dashboard.The insights help hotel management understand:<br>
<br>
-Who their customers are.<br>-Which hotels perform best.<br> - Seasonal booking patterns.<br> - Traveler behavior.<br> - Service quality performance.
<br>
**The dashboard provides a complete 360° view of hotel booking activity.**<br>
<br>
**1. Key Metrics.<br>**
-Total Hotels	25 <br>-Average Rating	5 <br>-Overall Average Score	8.94 <br>-Total Users	50K <br>-Total Reviews	50K <br>-Avg Cleanliness Score	9.05 <br>-Avg Staff Score	8.97 <br>-Avg Comfort Score	9.02 <br> 
These metrics show that the hotels maintain very high service quality, with overall scores above 8.9. <br>
<br>
**2. Top 10 Hotels by User Reviews.<br>Based on number of users who reviewed:**<br>
Nile Grandeur – 2104<br>Han River Oasis – 2071<br>The Kiwi Grand – 2060<br>The Orchid Palace – 2055<br>The Savannah House – 2049<br>The Bund Palace – 2040<br>Gaudi's Retreat – 2039<br>The Golden Oasis – 2035<br>Berlin Mitte Elite – 2022<br>Marina Bay Zenith – 2015<br>
**These hotels are the most popular and attract the most customer engagement.**<br>
<br>
**3. Customer Demographics.** <br>
Gender Split<br>Male: 51.92%<br>Female: 48.08% <br>A very balanced user base indicates equal hotel usage by both genders.<br>
<br>
**4. Traveler Type.** <br>
**Breakdown of users by travel intention:** <br>
Couple: 34.48%<br>Family: 23.89%<br>Business: 20.74%<br>Solo: 20.89%<br>Couple and Family travelers form over 58% of total bookings, meaning hotels must focus on family-friendly and romantic experiences.<br>
<br>
**5. Monthly Review Trend.** <br>
The monthly review chart shows:<br>Gradual increase from January to July Peak customer activity around June–August A decline towards end of year (post-travel season).This suggests summer is the busiest travel period.<br>
<br>
**6. Hotel Booking Rush.** <br>
The booking rush chart shows:<br>Monthly bookings fluctuate between 3,000 — 5,000+ Lowest booking activity appears early in the year Demand rises around mid-year.This helps hotels predict staffing and inventory requirements.<br>
<br>
**7. Location-Based Hotel Ratings.** <br>
The map visual displays average hotel ratings across regions:<br>Highly rated hotels are concentrated in Europe and North America Moderate ratings observed in Asia, Africa, and South America.This reveals geographic trends in hotel service quality and customer expectations.<br>
<br>
**8. Key Insights.** <br>
1.Hotels maintain strong cleanliness, comfort, and staff performance With average scores above 8.9, customer satisfaction is consistently high.<br>
2. Couple travelers are the largest user segmentHotels can design couple packages and offers.<br>
3. Summer months show highest customer engagementMarketing campaigns should target June–August.<br>
4. Top hotels consistently receive high engagement Properties like Nile Grandeur and Han River Oasis are customer favorites.<br>
5. Global distribution shows variation in ratings Hotel quality varies by region, helping brands understand where to improve.<br>
<br>
****9. Conclusion.****  <br>
The Hotel Booking Insights dashboard provides a comprehensive view of customer demographics, hotel popularity, review trends, and service quality.
With this analysis, hotel management can:<br>
Identify best-performing properties Improve low-performing regions Understand traveler behavior Enhance customer experience<br>Optimize seasonal strategies Overall, the data suggests that hotels deliver excellent service quality, attracting consistent engagement throughout the year.

### Dashboard.

<img width="940" height="528" alt="image" src="https://github.com/user-attachments/assets/7e4709dc-6972-456f-9d1c-709adb6e2ae4" />


## Data analysis using python.
This section of the project focuses on data analysis, machine learning, and sentiment analysis performed on hotel review data. The goal is to understand:<br>The relationship between base hotel ratings and real user ratings<br>Distribution of scores<br>Clustering hotels based on review patterns<br>Predicting traveler types<br>Analyzing polarity of guest feedback<br>Extracting keywords from reviews

**1. Correlation Analysis Between Base Ratings and User Ratings** <b>
**Key Findings**<b>Cleanliness_base strongly correlates with facilities_base (0.79) and staff_base (0.70).<b>Hotels that maintain cleanliness also excel in facilities and staff quality.<b>Comfort_base has a high correlation with staff_base (0.83).<b>Comfortable hotels tend to have better staff interactions.<b>Location_base has weak or negative correlations with most metrics.<b>Location does not influence user perception of internal facilities.<b>User Scores (score_cleanliness, score_comfort, etc.) correlate moderately with base scores, showing that actual user experiences align with expected ratings.
