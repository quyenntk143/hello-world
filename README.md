##### Our most valuable users are the ones who have completed a minimum of 5 VIEWs and 10 CLICKs. What age group do they fall in: 18-49 or 50-80?
```sql
WITH valuable_age AS ( SELECT pivot_by_user.user_id,
                              pivot_by_user.age,
                              SUM(CASE WHEN pivot_by_user.event_type = "CLICK" THEN pivot_by_user.step ELSE NULL END) AS event_click,
                              SUM(CASE WHEN pivot_by_user.event_type = "VIEW"  THEN pivot_by_user.step ELSE NULL END) AS event_view,
                              SUM(CASE WHEN pivot_by_user.event_type = "EXIT"  THEN pivot_by_user.step ELSE NULL END) AS event_exit
                         FROM 
                             (SELECT user_interaction.user_id,
                                     user_interaction.event_type,
                                     users.age,
                                     COUNT(1) AS step
                                FROM user_interaction 
                                JOIN users 
                                  ON user_interaction.user_id=users.user_id
                               GROUP BY 1,2,3) AS pivot_by_user 
                        GROUP BY 1 )
SELECT age, 
       event_click, 
       event_view
  FROM valuable_age
 WHERE event_click >=10 
   AND event_view >=5
```

##### We need to identify the first and the last interaction for every day. Show the whole record for the first and last interactions for each day.
```sql
WITH first_interaction AS ( SELECT DATE(event_time) AS event_date, 
                                   MIN(event_time), 
                                   event_type AS first_interaction
                              FROM user_interaction 
                             GROUP BY 1 ),
      last_interaction AS ( SELECT DATE(event_time) AS event_date, 
                                   MAX(event_time), 
                                   event_type AS last_interaction
                              FROM user_interaction 
                             GROUP BY 1 )
SELECT first_interaction.event_date, 
       first_interaction, 
       last_interaction
  FROM first_interaction
  JOIN last_interaction
    ON first_interaction.event_date = last_interaction.event_date
```
##### What country do most of our website visitors come from? 🌐
```sql
WITH website_visitor AS (SELECT users.user_id,
                                users.age,
                                users.country,
                                user_interaction.event_id,
                                user_interaction.event_type,
                                user_interaction.event_time
                           FROM user_interaction
                           JOIN users
                             ON user_interaction.user_id = users.user_id) 
SELECT country, 
       COUNT(user_id)
  FROM website_visitor
 GROUP BY 1
 ORDER BY 2 DESC
 LIMIT 1
```
