<img width="460" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/2b76e5c6-ff10-4b97-930c-4b557f8ef10e">



###  1) List the first 10 ads, sort the results by their names in an ascending order

```
SELECT TOP 10 *                                
FROM ads
ORDER BY ad_name
 ```
<img width="179" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/6f643af6-bab4-40e8-a84f-2c89671a7d11">


###  2) Display all clicks made in Sweden using Chrome browser

```
SELECT *
FROM clicks
WHERE country = 'Sweden' 
AND 
		browser = 'Chrome'
```
<img width="282" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/e8b47dca-095d-46c5-9fbe-759c0a395dd2">


###  3) List all conversions made in 2017

```
SELECT * 
FROM conversions
WHERE YEAR(conversion_date) = 2017
```
<img width="188" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/921d5b7a-4d6d-481c-8c18-056debee711d">


###  4) Using Clicks table, what is the most frequently used browser?

```
SELECT TOP 1 browser
FROM clicks
GROUP BY browser
ORDER BY COUNT(browser) DESC
```
<img width="63" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/3876b6a6-b7a7-4cfd-a5b0-df09561f7482">


###  5) Which ad has the highest amount of clicks ? display the distribution of clicks for each country


```
SELECT a.ad_name, c.country, COUNT(c.ad_id) as num_of_clicks
FROM ads a JOIN clicks c
ON a.ad_id = c.ad_id
WHERE a.ad_id = (
					SELECT TOP 1 ad_id FROM clicks 
					GROUP BY ad_id
					ORDER BY COUNT(ad_id) DESC )
GROUP BY a.ad_name, c.country
```
<img width="212" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/f66247e1-3745-4d02-bbb2-18368d5a656e">


### 6) Conversion rate is calculated using the following formula : SUM(total_conversions) \ SUM(total_clicks) * 100.
   ###   Find out the conversion rate for the ad with the highest amount of clicks 

```
with A as (
SELECT TOP 1 ad_id, COUNT(ad_id) as clicks_num 
FROM clicks
GROUP BY ad_id
ORDER BY COUNT(ad_id) DESC )

SELECT CAST(CAST(COUNT(cv.click_id) as float) / CAST(A.clicks_num as float) AS float) * 100 AS conversion_rate
FROM conversions cv JOIN clicks c 
ON cv.click_id = c.click_id
JOIN A ON A.ad_id = c.ad_id
GROUP BY c.ad_id, clicks_num
```
<img width="103" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/11e37daf-5328-46b7-8685-d7d0d90101c2">


### 7) Display the top-5 ads, having the highest conversion rate

```

SELECT TOP 5 a.ad_name,
CAST((CAST(COUNT(cv.conversion_id) AS float) / CAST(COUNT(c.click_id) AS float) * 100) AS varchar) + '%' as conversion_rate
FROM ads a JOIN clicks c
ON a.ad_id = c.ad_id
LEFT JOIN conversions cv
ON cv.click_id = c.click_id
GROUP BY a.ad_name
ORDER BY CAST((CAST(COUNT(cv.conversion_id) AS float) / CAST(COUNT(c.click_id) AS float) * 100) AS float) DESC 

```

<img width="137" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/931e9370-5f4d-41be-88fb-be9df87638c7">


### 8) Is there any conversion rate differance between the browsers?

``` 
SELECT c.browser,
CAST((CAST(COUNT(cv.conversion_id) as float) / CAST(COUNT(c.click_id) as float) * 100) as varchar) + '%' as conversion_rate
FROM ads a JOIN clicks c
ON a.ad_id = c.ad_id
LEFT OUTER JOIN conversions cv 
ON cv.click_id = c.click_id
GROUP BY c.browser
ORDER BY conversion_rate DESC
 ```
<img width="159" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/56280941-72e2-4fc3-907d-8740e44f1c42">


### 9) In average, for each ad, how many days it took for a click to become a conversion? (to convert)
```
SELECT a.ad_name, AVG(DATEDIFF(DAY, c.click_date, cv.conversion_date)) AS avg_time_to_convert
FROM ads a JOIN clicks c 
ON a.ad_id = c.ad_id
RIGHT JOIN conversions cv
ON c.click_id = cv.click_id
GROUP BY a.ad_name
 ```
<img width="197" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/a5ab4734-c3a1-4b6e-8364-5d381a82be80">


### 10) What is the most frequently used browser in Brazil?
```
SELECT TOP 1 browser 
FROM clicks
WHERE country = 'Brazil'
GROUP BY browser
ORDER BY COUNT(browser) DESC
```

<img width="62" alt="image" src="https://github.com/EranEid24/Ads-exercise-Ram-Kedem-/assets/149265837/4e5b597b-253b-47b8-b6a8-a0441ac43656">

