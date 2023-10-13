Title: Step-by-Step Data Analysis with SQLite: Exploring Movie Dataset

Objective: By the end of this lesson, learners will be able to analyze a movie dataset using SQLite, perform various SQL queries, and extract valuable insights from the data.

Necessary tools for the project:
IMDB Movies cleaned version 
sqliteonline.com
Repository with completed queries 

Extra optional assignments:
IMDB movies raw version-With the raw version they can clean this and add it to their portfolio.
Take a look at and get familiar with your upcoming capstone project options
How can a wellness company play it smart?
How does a bike-share navigate speedy success?
Follow your own case study path

Step 1: Introduction

Welcome the learners and explain the objective of the lesson: data analysis with SQLite using a movie dataset.
Briefly discuss the importance of data analysis in gaining insights and making informed decisions.
Provide an overview of the lesson plan and the topics covered.

Step 2: Setting up the Environment

Instruct learners to log into GitHub (if they have an account) and access the online SQLite platform (sqliteonline.com).
Instruct them to download the movie dataset and save it as a CSV file for easy access during the analysis.

Step 3: Setting up GitHub 

Instruct learners on how to create a new repository in Github. This is their portfolio project so they need to have access to this.
On the profile of GitHub click the icon in the upper right-hand corner and click “your repositories” 
Click the green “New” button
Type the repository name “portfolioprojects” 
Make sure to set it to public
Click the green “create repository” button at the bottom
This takes you to the portfolioproject repository click “add file”
(some learners may have a quick setup option instead if that is the case instruct them to click the “get started by creating a new file” option and it will bring them to the same place)
Name the file” IMDb project”
In the body, this is where they will paste all the code they write out during this session

Move over to sqliteonline.com
Step 4: Importing and Exploring the Dataset

Demonstrate how to open the SQLite online platform and import the movie dataset.
Click “import” -> “open” -> find file they downloaded -> “open” 
In the import box where it says “column name” make sure “first line” is selected and click “ok”
Under the table on the left side, they should see the file name and when you click on the arrow next to it the drop down will show all of the column names. 
If a learners still says c1 c2 and so on, instruct them to refresh the page and import again with “first line” selected. 

** make sure every time you and the learners go to write a query out, add a title. Since this will be copied and pasted into the repository when you are done and will act as a portfolio project having the title will help the reader understand the queries

Show learners how to view the dataset: 
(you will be typing everything I have highlighted in yellow into the environment)
–view our dataset–
SELECT * 
FROM IMDBTop250MoviesCleanedIMDBTop250Movies
Then highlight the query (not including the title) and click “run” in the top bar (you will do this for every new query you type out moving forward)
In the output, you'll be able to view column names and some of the output.

Explain the process of renaming the table to "Movies" for easier referencing: 
–change table name–
ALTER TABLE  IMDBTop250MoviesCleanedIMDBTop250Movies RENAME TO Movies

Then view to make sure the change worked by typing and running this query:
SELECT * 
FROM Movies


Step 5: Analyzing Budget and Revenue 

Guide learners on how to find the top 5 movies with the highest budgets: 
–Top 5 movies budgeted movies–
SELECT name, budget
FROM Movies
ORDER BY budget DESC
LIMIT 5

Discuss the need to adjust budgets for non-USD currencies for specific movies like "Princess Mononoke" and "3 Idiots."
Instruct learners on the SQL queries to update budgets:
For "Princess Mononoke": 
–Princess Mononoke update–
UPDATE Movies
SET budget = REPLACE(budget, 2400000000, 23500000)


For "3 Idiots": 
–3 Idiots Update–
UPDATE Movies 
SET budget = REPLACE(budget, 550000000, 6700000)

Now look at the updated version of the top 5 budgeted movies:
–updated top 5 budgeted movies–
SELECT name, budget
FROM Movies
Order by budget DESC
LIMIT 5

**Learners need to save this output and the following ones after they have run them and they are still on the screen
Click “Export” in the top bar
Click CSV
Change “column name” to “first line 
Click OK 
Go into their downloads and change the name of the CSV to reflect its output for example this one would be “top 5 budgeted movies”


Step 6: Exploring Top Movies

Show learners how to find the top 5 highest-rated movies: 
–Top 5 rated movies–
SELECT name, rating
FROM Movies
ORDER BY rating DESC
LIMIT 5

**make sure to export and save as CSV and rename the file

Guide them on finding the top 5 box office hits: 
--top 5 box office hits--
SELECT name, box_office
 FROM Movies
 GROUP BY name 
 ORDER BY box_office DESC
 LIMIT 5

**make sure to export and save as CSV and rename the file

Step 7: Analyzing Profits

Explain how to calculate the top 10 most profitable movies using the SQL query: 
--top 10 highest profit--
 SELECT name, budget, box_office, (box_office - budget) AS profit 
 FROM Movies
 ORDER BY profit DESC
 LIMIT 10

**make sure to export and save as CSV and rename the file

Step 8: Identifying Popular Genres 

Demonstrate the SQL query to find the most popular genre in the dataset. 
Explain why they can't just write a simple command like before because of the commas separating the values and being combined in one column it is counting them as one value instead of their comma-separated value. (example: comedy,drama,family is counted as 1 instead of comedy-1 drama-1 family-1)
To make this easier on you just copy and paste this from the repository into the environment and run it that way. This will save time and give you an opportunity to explain why the query is written the way it is:
 --Most popular genres--
SELECT genre, COUNT(*) AS genre_count
FROM (
  SELECT TRIM(value) AS genre
  FROM Movies
  CROSS JOIN json_each('["' || REPLACE(genre, ',', '","') || '"]')
)
GROUP BY genre
ORDER BY genre_count DESC
LIMIT 5


Break down and explain each row of this query 
We are creating a subquery to create a temp table 
TRIM function is used to remove any leading or trailing extra spaces from the genre
Cross join line is to perform cross join operation with the json_each function, which will extract elements from a JASON array
Cross join combines every row from both tables essentially creating a new table where each row from the first table is paired with every row from the second.
In SQLite the json_each function is used to extract the elements of an array as individual rows in the result set
REPLACE(genre,’,’’”,’”) replaces commas in the genre column with double quotation marks and a comma, effectively splitting the “genre” values into individual elements of a JSON array 
The concat operator of the 2 “Pipes” is used to combine the opening and closing brackets of the JSON array, along with the modified genre values this is specific to SQLite and other platforms it would be concat()
The json_each function is then applied to the resulting JSON array, treating each element as a separate row. The single quotes surrounding the JSON array indicate that this is a string

**make sure to export and save as CSV and rename the file

If enough time Step 9: Directors with Most Movies 

Guide learners on discovering the top 10 directors with the most movies in the dataset:
 --top 10 directors--
 SELECT directors, COUNT(*) AS Number_of_movies
 FROM Movies
 GROUP BY directors
 ORDER BY COUNT(*) DESC
 Limit 10

**make sure to export and save as CSV and rename the file

Step 10: Conclusion and Q&A 

Summarize the key points covered in the lesson.
Copy all of the queries created and pasted into Git Hub. Click “commit changes” in the top right and they can copy this URL and paste it into their portfolios after they finish with the other queries after class 
Open the floor for questions and address any concerns or queries learners may have.

Step 11: Homework 

Encourage learners to practice their data analysis skills using SQLite and explore other datasets of interest.
Suggest saving their SQL queries on GitHub with descriptive notes for personal reference and future collaboration.
Make sure learners have access to the repository I created and also give them the list of CSVs they will need for the next small group.
It is as follows:
Top 5 budgeted movies
Top 5 rated movies
Top 5 box office hits
Top 10 highest profit
Most popular genre 
Top 10 directors 
How many movies in each rating category
Top 10 best years
How many movies in each decade 
Most popular movie genre in each decade



