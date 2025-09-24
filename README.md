# Xquisite-Production-Database-Design-and-Implementation.
**Xquisite Production - A relational Database for film management**

![image](Logo.png)
---

## Project Description  
This project, developed by **Xquisite Analytics**, presents the design and implementation of a relational database system for **Xquisite Production**, a film production management platform. The database is designed to efficiently manage movie-related data, including information about **directors, actors, movies, revenues, and cast members**.  

The system provides a structured approach to handling film records, ensures data integrity through normalization, and supports flexible querying for both operational and analytical use. With this design, Xquisite Production can maintain a clear overview of its productions, financial performance, and talent involvement.  

---

## Table of Contents  
1. [Introduction](Introduction)
2. [Database Design Process](Database_Design_process)  
3. [Entity-Relationship (ER) Model](Entity_Relationship_(ER)_Model)
4. [Database Schema and Tables](Database_Schema_and_Tables)  
5. [Relationships Between Tables](Relationships_Between_Tables)  
6. [Sample Queries](Sample_Queries) 
7. [Conclusion](Conclusion)
8. [Limitations](Limitations)

---

## 1. Introduction  
The **Xquisite Production Database** was developed to handle key operations in a film production environment. Film companies need structured storage for information about **directors, actors, movies, and financial revenues**. This project aims to provide a well-normalized relational database that avoids redundancy, ensures data integrity, and allows for flexible querying.  

---

## 2. Database Design Process  
The design followed a **structured process**:  
- Identifying entities: **Directors, Actors, Movies, Revenues, Movie Cast**  
- Defining attributes for each entity  
- Establishing relationships between entities  
- Enforcing constraints (e.g., primary keys, foreign keys, composite keys)  
- Normalizing to ensure efficiency and eliminate redundancy  

---

## 3. Entity-Relationship (ER) Model  
- **Directors** direct multiple movies → *One-to-Many*  
- **Movies** feature multiple actors, and **actors** can act in multiple movies → *Many-to-Many*  
- **Movies** have a revenue record → *One-to-One*  
- **Actors** and **Directors** are independent entities but both relate to movies  

---

## 4. Database Schema and Tables  

### **Directors Table**  
- Stores information about film directors.  

```sql
CREATE TABLE Directors (
	director_id INT PRIMARY KEY IDENTITY,
	first_name VARCHAR(30), 
	last_name VARCHAR(30) NOT NULL,
	date_of_birth DATE,
	nationality VARCHAR(20)
);
```
---
### Actors Table
- Stores Actors personal details
```
CREATE TABLE Actors (
Actor_id INT PRIMARY KEY IDENTITY,
first_name VARCHAR(30),
last_name VARCHAR(30),
gender CHAR(1), -- M or F
date_of_birth DATE
);
```
---
### Movies Table

- Contains information about movies Produced
```
CREATE TABLE Movies (
	movie_id INT PRIMARY KEY IDENTITY,
	movie_name VARCHAR(50),
	movie_length INT, -- in minutes
	movie_lang VARCHAR(20),
	release_date DATE,
	age_certificate VARCHAR(5), -- e.g., 18+, PG
	director_id INT REFERENCES Directors(director_id)
);
```
---
### Movies Revenue Table

- Records the domestic and International takings of each movies
```
CREATE TABLE Movie_revenues (
	revenue_id INT PRIMARY KEY IDENTITY,
	movie_id INT REFERENCES Movies(movie_id),
	domestic_takings DECIMAL(10,2),
	international_takings DECIMAL(10,2)
);
```
---

### Movies Actors Table

- A joint table to handle the relationship (many-to-many).
```
CREATE TABLE Movies_actors (
	movie_id INT REFERENCES Movies(movie_id),
	actor_id INT REFERENCES Actors(actor_id),
	PRIMARY KEY(movie_id, actor_id)
);
```
---

### 5. Relationships Between Tables  
- **Directors → Movies**: One director can direct many movies.
```  
- **Movies → Actors**: Many-to-Many relationship managed through the `Movies_actors` junction table.  
- **Movies → Movie_revenues**: One-to-One relationship since each movie has a unique revenue record.
```  
---

### 6. Sample Queries  

### Retrieve all movies and their directors  
```sql
SELECT m.movie_name, d.first_name, d.last_name
FROM Movies m
JOIN Directors d ON m.director_id = d.director_id;
```
---
### List all Actors in a particular movie
```
SELECT a.first_name, a.last_name
FROM Actors a
JOIN Movies_actors ma ON a.actor_id = ma.actor_id
JOIN Movies m ON ma.movie_id = m.movie_id
WHERE m.movie_name = 'Inception';
```
---
### Get Revenue details of each movie
```
SELECT m.movie_name, r.domestic_takings, r.international_takings
FROM Movies m
JOIN Movie_revenues r ON m.movie_id = r.movie_id;
```
---
## 7. Conclusion  
The **Xquisite Production Database** provides a solid relational framework for managing film production data. It efficiently handles director–movie–actor relationships, supports financial reporting on revenues, and offers flexible queries for retrieving insights into the film industry.  

---

## 8. Limitations  
- The current design does not handle **award records, genres, or production companies**.  
- **Gender values** are restricted to "M" and "F" only, excluding non-binary categories.  
- The schema assumes each movie has only one director and one revenue record, which may not apply to co-directed or multi-studio productions.
---
## About me
- I transform complex data into clear, actionable insights.
Using analytics, machine learning, and visualization, I help you understand trends, improve efficiency, and plan for the future.

Connect with me @https://www.linkedin.com/in/adetokunbo-olasupo-70aa042a1

