# Netflix_SQL

<h3>Problem Statement:</h3>
<br>
<p>Netflix aims to gain valuable insights into their shows and movies from extensive datasets, comprising around 82,000 rows of data. However, the challenge lies in effectively analyzing and extracting meaningful information from this vast dataset. To address this, Netflix seeks a robust and scalable data analytics solution to handle the large volume of data and reveal important patterns and trends.</p>
<be>
<h3>My Solution:</h3>
<p>To assist Netflix in gaining insights from their extensive movies and shows dataset, I'll use SQL and Tableau. Using SQL functions, I'll extract key metrics like viewer ratings, genre preferences, and viewership patterns. With Tableau, I'll create an interactive dashboard, presenting findings through visually appealing charts and graphs, allowing Netflix stakeholders to explore data dynamically.</p>

<H3>Tools Used:</H3><p>Excel,SQL,Tableau</p>


## Questions I Wanted To Answer From the Dataset:

## 1. Which movies and shows on Netflix ranked in the top 10 and bottom 10 based on their IMDB scores?
- Top 10 Movies
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
AND type = 'MOVIE'
ORDER BY imdb_score DESC
LIMIT 10
```
Result: 

![Q1](https://i.ibb.co/6mQWCw9/Screen-Shot-2023-07-09-at-9-38-11-PM.png)

- Top 10 Shows
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE imdb_score >= 8.0
AND type = 'SHOW'
ORDER BY imdb_score DESC
LIMIT 10
```
Result: 

![Q2](https://i.ibb.co/QppHsN2/Screen-Shot-2023-07-09-at-9-45-58-PM.png)

- Bottom 10 Movies
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE type = 'MOVIE'
ORDER BY imdb_score ASC
LIMIT 10
```
Result: 

![Q3](https://i.ibb.co/tMXV1yp/Screen-Shot-2023-07-09-at-9-47-24-PM.png)

- Bottom 10 Shows
```mysql
SELECT title, 
type, 
imdb_score
FROM shows_movies.titles
WHERE type = 'SHOW'
ORDER BY imdb_score ASC
LIMIT 10
```
Result: 

![Q4](https://i.ibb.co/Y7Qjvg5/Screen-Shot-2023-07-09-at-9-49-36-PM.png)

An IMDB score is a widely recognized measure of the overall quality and popularity of a movie or show. The top 10 movies and shows stood out for their exceptional IMDB scores, indicating that they are highly regarded by viewers. These titles have likely garnered significant acclaim and positive reviews, contributing to their high rankings within the Netflix library. Viewers who are seeking quality content would find these selections very appealing. On the other hand, the bottom 10 movies and shows had lower IMDB scores. While these entries may not have resonated as strongly with audiences, it's important to note that many factors influence these rankings such as individual preferences, weak plot, poor acting, and low-quality production. By uncovering the top and bottom performers based on IMDB scores, this project sheds light on the varying levels of audience reception and highlights titles that are likely to be well-received and those that may have room for improvement. These findings can provide valuable insights for viewers seeking highly-rated content and can serve as a basis for further analysis and decision-making for Netflix's audience recommendations. 

## 2. How many movies and shows fall in each decade in Netflix's library?
```mysql
SELECT CONCAT(FLOOR(release_year / 10) * 10, 's') AS decade,
	COUNT(*) AS movies_shows_count
FROM shows_movies.titles
WHERE release_year >= 1940
GROUP BY CONCAT(FLOOR(release_year / 10) * 10, 's')
ORDER BY decade;
```
Result: 

![Q5](https://i.ibb.co/8dTzVZ3/Screen-Shot-2023-07-09-at-10-02-18-PM.png)

The results of the SQL query provide a fascinating insight into the distribution of movies and shows across different decades in Netflix's library. The data reveals a significant shift in content availability over time, with a notable increase in the number of titles from the 2000s onwards. Starting from the earlier decades, the 1940s-1980s showcase a small fraction of the total entries, suggesting that Netflix's collection from these decades is relatively limited. The 1990s demonstrate a large surge in offerings, with 121 titles. However, the true turning point in Netflix's library occurs in the 2010s with a remarkable 3,304 movies and shows from this decade. This abundance highlights Netflix's dedication to featuring contemporary content that aligns with current trends and audience preferences.

Even though the 2020s are still in progress, the dataset reveals an impressive count of 1,972 movies and shows, indicating a strong focus on acquiring and producing content from recent years. Overall, these findings shed light on Netflix's strategy of curating library that covers a wide range of decades. The significant increase in content availability from the 2000s onwards suggests a concerted effort to offer a diverse selection of titles. This collection spanning multiple decades allows Netflix's audience to explore a variety of movies and shows that reflect different eras. 

## 3. How did age-certifications impact the dataset?
```mysql
SELECT DISTINCT age_certification, 
ROUND(AVG(imdb_score),2) AS avg_imdb_score,
ROUND(AVG(tmdb_score),2) AS avg_tmdb_score
FROM shows_movies.titles
GROUP BY age_certification
ORDER BY avg_imdb_score DESC
```
Result: 

![Q6](https://i.ibb.co/SvJyjgF/Screen-Shot-2023-07-09-at-10-16-52-PM.png)

```mysql
SELECT age_certification, 
COUNT(*) AS certification_count
FROM shows_movies.titles
WHERE type = 'Movie' 
AND age_certification != 'N/A'
GROUP BY age_certification
ORDER BY certification_count DESC
LIMIT 5;
```
Results: 

![Q7](https://i.ibb.co/T0f5cNq/Screen-Shot-2023-07-09-at-10-20-23-PM.png)

The first query focused on the average IMDB scores associated with each age certification, revealing interesting trends in audience ratings. According to the data, TV-14 emerges as the age certification with the highest average IMDB score of 6.71. This suggests that content designated for viewers aged 14 and older tends to receive relatively favorable ratings. The age certification TV-G obtains an average score of 6.01, signaling the appreciation for content suitable for all audiences. On the other hand, the age certifications R and TV-Y, each with average scores of 6, demonstrate that while they may have lower ratings, there is still a substantial audience that finds enjoyment in these respective categories.

When examining the distribution of movies and shows across age certifications, the second query showcases the varying prevalence of different certifications within Netflix's dataset. R emerges as the most prevalent age certification, with 556 titles falling under this category. PG-13 closely follows with 451 titles, reflecting a significant number of movies and shows targeted at mature audiences. The age certification PG accounts for 233 titles, indicating a considerable selection suitable for general audiences. The dataset also includes 124 titles classified as G, which mostly caters to a younger audience. Lastly, the least represented certification is NC-17, with only 16 titles available. These findings highlight the diverse range of age certifications present in Netflix's movies and shows dataset and provide valuable insights into both audience preferences and content distribution. The higher average scores associated with TV-14, TV-MA, and TV-PG certifications suggest that content aligned with these age categories tends to resonate positively with viewers. 

## 4. Which genres are the most common? 
- Top 10 most common genres for MOVIES
```mysql
SELECT genres, 
COUNT(*) AS title_count
FROM shows_movies.titles 
WHERE type = 'Movie'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```
Result:

![Q8](https://i.ibb.co/VWrgd8m/Screen-Shot-2023-07-10-at-12-25-40-PM.png)

- Top 10 most common genres for SHOWS
```mysql
SELECT genres, 
COUNT(*) AS title_count
FROM shows_movies.titles 
WHERE type = 'Show'
GROUP BY genres
ORDER BY title_count DESC
LIMIT 10;
```
Result: 

![Q9](https://i.ibb.co/P59s4X7/Screen-Shot-2023-07-10-at-12-27-41-PM.png)

- Top 3 most common genres OVERALL
```mysql
SELECT t.genres, 
COUNT(*) AS genre_count
FROM shows_movies.titles AS t
WHERE t.type = 'Movie' or t.type = 'Show'
GROUP BY t.genres
ORDER BY genre_count DESC
LIMIT 3;
```
Result: 

![Q10](https://i.ibb.co/qMvMBGf/Screen-Shot-2023-07-10-at-12-30-04-PM.png)

Checking what kinds of movies people like helps us know what's popular on Netflix. The first look at movies shows that comedy is the favorite, with 384 movies. Documentaries and drama are also liked, with 230 and 224 movies each. People also enjoy movies that mix genres, like comedy + documentary or comedy + drama. The mix of drama + romance, drama + comedy, and comedy + romance shows that viewers like movies with different flavors. This tells us that Netflix offers lots of different types of movies to suit different tastes.

Now, looking at shows, we find that reality is the most popular genre on Netflix, with 113 shows. Drama comes next with 104 shows, and both comedy and documentaries are also quite common, each having 100 shows. Like in movies, people seem to enjoy shows that mix genres, such as comedy + drama and drama + romance, showing that viewers like variety in their shows too.

Looking at both movies and shows together, the top three most common genres on Netflix are comedy, leading with 484 entries, showing it's very popular. Documentation is next with 329 entries, liked for its informative content. Drama comes in third with 328 entries. These results reveal the genres that rule Netflix, showing how the platform aims to please a wide range of viewer tastes.

<h3>Conclusion:</h3>
<p>
By looking into the dataset, we got a good picture of what's on Netflix. We found the top 10 and bottom 10 movies and shows based on IMDB scores, showing which ones people liked and didn't. This info can help viewers pick what to watch and point out areas where Netflix can make things better. Checking out movies and shows from different decades, we noticed more options from the 2000s onwards. This shows Netflix is dedicated to having newer content that matches today's trends and what people like.

Age certifications significantly influenced the dataset, affecting average IMDB scores and the variety of movies and shows. The analysis showed that TV-14 received the highest average score, indicating it's well-liked by viewers. Different age certifications also highlighted the diversity of content on Netflix. Looking at the most common genres, comedy was the top pick for both movies and shows, followed by documentaries and drama. Many combinations of genres were popular, revealing that viewers enjoy content with multiple genres.</p>
