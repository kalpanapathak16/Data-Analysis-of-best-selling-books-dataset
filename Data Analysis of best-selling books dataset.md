# Contents
* [Introduction](#introduction)    
* [Getting Started](#getting-started) 
    * [Prerequisites](#prerequisites)
    * [Importing Python libraries](#importing-python-libraries)
    * [Importing Plotly libraries](#importing-plotly-libraries)
    * [Reading dataset](#reading-dataset)
* [Understanding the data](#understanding-the-data)
    * [Information of dataset](#information-of-dataset)
* [Cleaning the data](#cleaning-the-data)
    * [Changing lengthy column names](#Changing-lengthy-column-names)
    * [Cleaning messy book titles](#Cleaning-messy-book-titles)
    * [Verifying updated book titles](#Verifying-updated-book-titles)
    * [Finding missing values](#Finding-missing-values)
    * [Deleting empty rows](#Deleting-empty-rows)
    * [Verifying updated rows](#Verifying-updated-rows)
* [Retrieving statistics summary](#Retrieving-statistics-summary)
* [Analyzing univariate data](#Analyzing-univariate-data)
	* [Identifying top-selling books](#Identifying-top-selling-books)
	* [Displaying bar chart of top-selling books](#Displaying-bar-chart-of-top-selling-books)
	* [Visualizing distribution of sales](#Visualizing-distribution-of-sales)
* [Analyzing bivariate data](#Analyzing-bivariate-data)
	* [Identifying relationship between sales and year](#Identifying-relationship-between-sales-and-year)
	* [Identifying top authors by sales](#Identifying-top-authors-by-sales)
	* [Identifying total sales by genre](#Identifying-total-sales-by-genre)
	* [Identifying total sales by book](#Identifying-total-sales-by-book)
	* [Determining language with the highest sales](#Determining-language-with-the-highest-sales)
	* [Determining sales distribution by language](#Determining-sales-distribution-by-language)
	* [Identifying sales over time](#Identifying-sales-over-time)
* [Conclusion](#Conclusion)

# Introduction  <a class="anchor" id="introduction"></a>
[Back to Top](#Contents)

This project analyzes data from the <i>best_selling_books</i> CSV file. By analyzing a dataset of best-selling books, you can gain valuable insights into the factors that contribute to a book's success. Use the Exploratory Data Analysis (EDA) technique to identify which factors are most influential in the success of best-selling books. Additionally, you can use data visualization techniques to present your findings effectively.

The <i>best_selling_books</i> CSV file contains the following data columns:
* Book
* Authors
* Original language
* First published
* Approximate sales in millions
* Genre

The following diagram depicts the process of the data analysis required for the <i>best_selling_books</i> dataset.
![best-book-selling-data-analysis-workflow.png](attachment:best-book-selling-data-analysis-workflow.png)

For a thorough data analysis, use these data points and examine the outcome.

# Getting Started <a class="anchor" id="getting-started"></a>

## Prerequisites<a class="anchor" id="prerequisites"></a>
[Back to Top](#Contents)

Before you perform data analysis, 

* Ensure that the *best-selling-books.csv* file is available in the Python working directory. You can find the location of the Python working directory by running the `pwd` command.
* Ensure that you have already installed the following necessary Python libraries:
  - **Pandas and Numpy**: Use for data processing of a CSV file, data manipulation, and numerical calculations
  - **Seaborn, Matplotlib, and Plotly**: Use for data visualization. You can view data on attractive graphs, such as histograms, box plots, pie charts, and so on.

## Importing Python libraries <a class="anchor" id="step1:importing-python-libraries"></a>
[Back to Top](#Contents)


```python
#importing Required library 
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import numpy as np # linear algebra
import seaborn as sns # to get summary statistics
import matplotlib.pyplot as plt # to get summary statistics
%matplotlib inline
```

## Importing Plotly libraries<a class="anchor" id="importing-plotly-libraries"></a>
[Back to Top](#Contents)


```python
#importing plotly library
from plotly.offline import iplot
import plotly as py
import plotly.tools as tls
import cufflinks as cf
py.offline.init_notebook_mode(connected=True) #Turning on notebook mode 
cf.go_offline()
```


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.25.2.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.25.2.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>



## Reading dataset<a class="anchor" id="reading-dataset"></a>
[Back to Top](#Contents)

Using the `pd.read_csv()` function, convert best-selling-books CSV data to a pandas DataFrame.


```python
df=pd.read_csv(r"best-selling-books.csv") #dataset
```

# Understanding the data<a class="anchor" id="understanding-the-data"></a>
[Back to Top](#Contents)

Before making any inferences, examine all variables in the data. The primary aim of data understanding is to extract key insights about the data, including its size (number of rows and columns), data values, data types, and any missing values in the dataset.

* **shape**: Displays the number of observations(rows) and features(columns) in the dataset
* **info()**: Assists in comprehending data: reveals data types, record counts per column, presence of null values, data types, and dataset memory consumption
* **head()**: Displays the top 5 observations/rows of the dataset
* **tail()**: Displays the bottom 5 observations/rows of the dataset



```python
df.shape #displays 174 rows and 6 columns in the dataset
```




    (174, 6)



## Information of dataset<a class="anchor" id="information-of-dataset"></a>
[Back to Top](#Contents)

`df.info()` shows the following variables:
* **Categorical variables**: Book, Authors, Original language, and Genre with the object data type. The Genre variable/column has missing values.
* **Numeric variables**: First published with the int64 data type and Approximate sales in millions with the float64 data type. 



```python
df.info() #get information on dataset
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 174 entries, 0 to 173
    Data columns (total 6 columns):
     #   Column                         Non-Null Count  Dtype  
    ---  ------                         --------------  -----  
     0   Book                           174 non-null    object 
     1   Authors                        174 non-null    object 
     2   Original language              174 non-null    object 
     3   First published                174 non-null    int64  
     4   Approximate sales in millions  174 non-null    float64
     5   Genre                          118 non-null    object 
    dtypes: float64(1), int64(1), object(4)
    memory usage: 8.3+ KB
    


```python
df.head() #show top rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Book</th>
      <th>Authors</th>
      <th>Original language</th>
      <th>First published</th>
      <th>Approximate sales in millions</th>
      <th>Genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A Tale of Two Cities</td>
      <td>Charles Dickens</td>
      <td>English</td>
      <td>1859</td>
      <td>200.0</td>
      <td>Historical fiction</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Little Prince (Le Petit Prince)</td>
      <td>Antoine de Saint-Exupéry</td>
      <td>French</td>
      <td>1943</td>
      <td>200.0</td>
      <td>Novella</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Harry Potter and the Philosopher's Stone</td>
      <td>J. K. Rowling</td>
      <td>English</td>
      <td>1997</td>
      <td>120.0</td>
      <td>Fantasy</td>
    </tr>
    <tr>
      <th>3</th>
      <td>And Then There Were None</td>
      <td>Agatha Christie</td>
      <td>English</td>
      <td>1939</td>
      <td>100.0</td>
      <td>Mystery</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Dream of the Red Chamber (紅樓夢)</td>
      <td>Cao Xueqin</td>
      <td>Chinese</td>
      <td>1791</td>
      <td>100.0</td>
      <td>Family saga</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail() #show bottom rows
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Book</th>
      <th>Authors</th>
      <th>Original language</th>
      <th>First published</th>
      <th>Approximate sales in millions</th>
      <th>Genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>169</th>
      <td>The Goal</td>
      <td>Eliyahu M. Goldratt</td>
      <td>English</td>
      <td>1984</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Fahrenheit 451</td>
      <td>Ray Bradbury</td>
      <td>English</td>
      <td>1953</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Angela's Ashes</td>
      <td>Frank McCourt</td>
      <td>English</td>
      <td>1996</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>172</th>
      <td>The Story of My Experiments with Truth (સત્યના...</td>
      <td>Mohandas Karamchand Gandhi</td>
      <td>Gujarati</td>
      <td>1929</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>173</th>
      <td>Bridget Jones's Diary</td>
      <td>Helen Fielding</td>
      <td>English</td>
      <td>1996</td>
      <td>10.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



# Cleaning the data<a class="anchor" id="cleaning-the-data"></a>
[Back to Top](#Contents)

The following variables/column names are too lengthy and not easy to understand. You can rename them for easy data processing.
| Original column name| New column name|
| ----------- | ----------- |
| First published| Year |
| Original language| Language |

## Changing lengthy column names<a class="anchor" id="Changing-lengthy-column-names"></a>
[Back to Top](#Contents)

Run the `df.rename(columns={'original column name':'new column name'})` command.


```python
#Changing the column names
df = df.rename(columns={'Original language':'Language','First published':'Year',
                                  'Approximate sales in millions':'Sales'})
df.columns
```




    Index(['Book', 'Authors', 'Language', 'Year', 'Sales', 'Genre'], dtype='object')



## Cleaning messy book titles<a class="anchor" id="Cleaning-messy-book-titles"></a>
[Back to Top](#Contents)

Cleaning column headers is an essential data preparation step. Some of the book titles contain unwanted characters. You must clean these messy column headers to improve data clarity and usability. Remove these characters from the headers to significantly enhance data visualization.


```python
print(df.Book.unique())
print(df.Book.nunique())
```

    ['A Tale of Two Cities' 'The Little Prince (Le Petit Prince)'
     "Harry Potter and the Philosopher's Stone" 'And Then There Were None'
     'Dream of the Red Chamber (紅樓夢)' 'The Hobbit'
     'The Lion, the Witch and the Wardrobe' 'She: A History of Adventure'
     'Vardi Wala Gunda (वर्दी वाला गुंडा)' 'The Da Vinci Code'
     'Harry Potter and the Chamber of Secrets'
     'Harry Potter and the Prisoner of Azkaban'
     'Harry Potter and the Goblet of Fire'
     'Harry Potter and the Order of the Phoenix'
     'Harry Potter and the Half-Blood Prince'
     'Harry Potter and the Deathly Hallows' 'The Alchemist (O Alquimista)'
     'The Catcher in the Rye' 'The Bridges of Madison County'
     'Ben-Hur: A Tale of the Christ' 'You Can Heal Your Life'
     'One Hundred Years of Solitude (Cien años de soledad)' 'Lolita' 'Heidi'
     'The Common Sense Book of Baby and Child Care' 'Anne of Green Gables'
     'Black Beauty' 'The Name of the Rose (Il Nome della Rosa)'
     'The Eagle Has Landed' 'Watership Down' 'The Hite Report'
     "Charlotte's Web" 'The Ginger Man' 'The Tale of Peter Rabbit'
     'Jonathan Livingston Seagull' 'The Very Hungry Caterpillar'
     'A Message to Garcia' 'To Kill a Mockingbird' 'Flowers in the Attic'
     'Cosmos' "Sophie's World (Sofies verden)" 'Angels & Demons'
     'Kane and Abel' 'How the Steel Was Tempered (Как закалялась сталь)'
     'War and Peace (Война и мир)'
     'The Adventures of Pinocchio (Le avventure di Pinocchio)'
     'The Diary of Anne Frank (Het Achterhuis)' 'Your Erroneous Zones'
     'The Thorn Birds' 'The Purpose Driven Life' 'The Kite Runner'
     'Valley of the Dolls' 'Alcoholics Anonymous Big Book'
     'How to Win Friends and Influence People' 'The Great Gatsby'
     'Gone with the Wind' 'Rebecca' 'Nineteen Eighty-Four'
     'The Revolt of Mamie Stover'
     'The Girl with the Dragon Tattoo (Män som hatar kvinnor)'
     'The Lost Symbol' 'The Hunger Games' 'James and the Giant Peach'
     'The Young Guard (Молодая гвардия)' 'Who Moved My Cheese?'
     'A Brief History of Time' 'Paul et Virginie' 'Lust for Life'
     'The Wind in the Willows' 'The 7 Habits of Highly Effective People'
     'Virgin Soil Upturned (Поднятая целина)' 'The Celestine Prophecy'
     'The Fault in Our Stars' 'The Girl on the Train' 'The Shack'
     'Uncle Styopa (Дядя Стёпа)' 'The Godfather' 'Love Story' 'Catching Fire'
     'Mockingjay' 'Kitchen (キッチン)' 'Andromeda Nebula (Туманность Андромеды)'
     'Autobiography of a Yogi (योगी कथामृत)' 'Gone Girl'
     'All Quiet on the Western Front (Im Westen nichts Neues)'
     'The Bermuda Triangle' 'Things Fall Apart ' 'Animal Farm '
     'Wolf Totem (狼图腾)' 'The Happy Hooker: My Own Story' 'Jaws'
     'Love You Forever' "The Women's Room"
     "What to Expect When You're Expecting" 'Adventures of Huckleberry Finn'
     'The Secret Diary of Adrian Mole, Aged 13¾' 'Pride and Prejudice'
     'Kon-Tiki: Across the Pacific in a Raft (Kon-Tiki ekspedisjonen)'
     'The Good Soldier Švejk (Osudy dobrého vojáka Švejka za světové války)'
     'Where the Wild Things Are' 'The Power of Positive Thinking' 'The Secret'
     'Fear of Flying' 'Dune' 'Charlie and the Chocolate Factory'
     'The Naked Ape' 'Where the Crawdads Sing'
     'Totto-chan, the Little Girl at the Window (窓ぎわのトットちゃん)' 'Matilda'
     'The Book Thief' 'The Horse Whisperer' 'Goodnight Moon'
     'The Neverending Story (Die unendliche Geschichte)'
     'All the Light We Cannot See' 'Fifty Shades of Grey' 'The Outsiders'
     'Guess How Much I Love You' 'Shōgun' 'The Poky Little Puppy'
     'The Pillars of the Earth' 'Perfume (Das Parfum)' 'The Grapes of Wrath'
     'The Shadow of the Wind (La sombra del viento)' 'Interpreter of Maladies'
     'Becoming' "The Hitchhiker's Guide to the Galaxy" 'Tuesdays with Morrie'
     "God's Little Acre" "Follow Your Heart (Va' dove ti porta il cuore)"
     'A Wrinkle in Time' 'Long Walk to Freedom' 'The Old Man and the Sea'
     'Life After Life' 'Peyton Place ' 'The Giver' 'Me Before You'
     'Norwegian Wood (ノルウェイの森)' 'The Plague (La Peste)'
     'No Longer Human (人間失格)'
     "Man's Search for Meaning (Ein Psychologe erlebt das Konzentrationslager)"
     'The Divine Comedy (La Divina Commedia)' 'The Prophet'
     'The Boy in the Striped Pyjamas' 'The Exorcist' 'The Gruffalo'
     'Fifty Shades Darker' 'Tobacco Road' "Ronia, the Robber's Daughter"
     'The Cat in the Hat' 'Diana: Her True Story' 'The Help' 'Catch-22'
     "The Stranger (L'Étranger)" 'Eye of the Needle' 'The Lovely Bones'
     'Wild Swans' 'Santa Evita' 'Night (Un di Velt Hot Geshvign)'
     'Confucius from the Heart (于丹《论语》心得)' 'The Total Woman'
     'Knowledge-value Revolution (知価革命)'
     "Problems in China's Socialist Economy (中国社会主义经济问题研究)"
     'What Color Is Your Parachute?' 'The Dukan Diet' 'The Joy of Sex'
     'The Gospel According to Peanuts' 'The Subtle Art of Not Giving a Fuck'
     'Life of Pi' 'The Front Runner' 'The Goal' 'Fahrenheit 451'
     "Angela's Ashes"
     'The Story of My Experiments with Truth (સત્યના પ્રયોગો અથવા આત્મકથા)'
     "Bridget Jones's Diary"]
    174
    

To remove multiple characters from a string in Python, you can use regular expressions from the `re` module. 
1. Import the regular expression library by using the `import re` function
2. Define the `remove_text_within_parentheses(Book)` custom function that uses regular expressions to remove text enclosed within parentheses from a given string Book.
3. Specify the `re.sub(r'\([^)]*\)'`regular expression that matches any text enclosed within parentheses and replaces it with an empty string `('')`, effectively removing the content within the brackets.
4. Apply the custom function to the 'Book' column using the `apply()` method. 


```python
import re
# Function to remove text within parentheses from a string
def remove_text_within_parentheses(Book):
    return re.sub(r'\([^)]*\)', '', Book)
df['Book'] = df['Book'].apply(remove_text_within_parentheses)

```

## Verifying updated book titles<a class="anchor" id="Verifying-updated-book-titles"></a>
[Back to Top](#Contents)


```python
print(df.Book.unique())
print(df.Book.nunique())
```

    ['A Tale of Two Cities' 'The Little Prince '
     "Harry Potter and the Philosopher's Stone" 'And Then There Were None'
     'Dream of the Red Chamber ' 'The Hobbit'
     'The Lion, the Witch and the Wardrobe' 'She: A History of Adventure'
     'Vardi Wala Gunda ' 'The Da Vinci Code'
     'Harry Potter and the Chamber of Secrets'
     'Harry Potter and the Prisoner of Azkaban'
     'Harry Potter and the Goblet of Fire'
     'Harry Potter and the Order of the Phoenix'
     'Harry Potter and the Half-Blood Prince'
     'Harry Potter and the Deathly Hallows' 'The Alchemist '
     'The Catcher in the Rye' 'The Bridges of Madison County'
     'Ben-Hur: A Tale of the Christ' 'You Can Heal Your Life'
     'One Hundred Years of Solitude ' 'Lolita' 'Heidi'
     'The Common Sense Book of Baby and Child Care' 'Anne of Green Gables'
     'Black Beauty' 'The Name of the Rose ' 'The Eagle Has Landed'
     'Watership Down' 'The Hite Report' "Charlotte's Web" 'The Ginger Man'
     'The Tale of Peter Rabbit' 'Jonathan Livingston Seagull'
     'The Very Hungry Caterpillar' 'A Message to Garcia'
     'To Kill a Mockingbird' 'Flowers in the Attic' 'Cosmos' "Sophie's World "
     'Angels & Demons' 'Kane and Abel' 'How the Steel Was Tempered '
     'War and Peace ' 'The Adventures of Pinocchio '
     'The Diary of Anne Frank ' 'Your Erroneous Zones' 'The Thorn Birds'
     'The Purpose Driven Life' 'The Kite Runner' 'Valley of the Dolls'
     'Alcoholics Anonymous Big Book' 'How to Win Friends and Influence People'
     'The Great Gatsby' 'Gone with the Wind' 'Rebecca' 'Nineteen Eighty-Four'
     'The Revolt of Mamie Stover' 'The Girl with the Dragon Tattoo '
     'The Lost Symbol' 'The Hunger Games' 'James and the Giant Peach'
     'The Young Guard ' 'Who Moved My Cheese?' 'A Brief History of Time'
     'Paul et Virginie' 'Lust for Life' 'The Wind in the Willows'
     'The 7 Habits of Highly Effective People' 'Virgin Soil Upturned '
     'The Celestine Prophecy' 'The Fault in Our Stars' 'The Girl on the Train'
     'The Shack' 'Uncle Styopa ' 'The Godfather' 'Love Story' 'Catching Fire'
     'Mockingjay' 'Kitchen ' 'Andromeda Nebula ' 'Autobiography of a Yogi '
     'Gone Girl' 'All Quiet on the Western Front ' 'The Bermuda Triangle'
     'Things Fall Apart ' 'Animal Farm ' 'Wolf Totem '
     'The Happy Hooker: My Own Story' 'Jaws' 'Love You Forever'
     "The Women's Room" "What to Expect When You're Expecting"
     'Adventures of Huckleberry Finn'
     'The Secret Diary of Adrian Mole, Aged 13¾' 'Pride and Prejudice'
     'Kon-Tiki: Across the Pacific in a Raft ' 'The Good Soldier Švejk '
     'Where the Wild Things Are' 'The Power of Positive Thinking' 'The Secret'
     'Fear of Flying' 'Dune' 'Charlie and the Chocolate Factory'
     'The Naked Ape' 'Where the Crawdads Sing'
     'Totto-chan, the Little Girl at the Window ' 'Matilda' 'The Book Thief'
     'The Horse Whisperer' 'Goodnight Moon' 'The Neverending Story '
     'All the Light We Cannot See' 'Fifty Shades of Grey' 'The Outsiders'
     'Guess How Much I Love You' 'Shōgun' 'The Poky Little Puppy'
     'The Pillars of the Earth' 'Perfume ' 'The Grapes of Wrath'
     'The Shadow of the Wind ' 'Interpreter of Maladies' 'Becoming'
     "The Hitchhiker's Guide to the Galaxy" 'Tuesdays with Morrie'
     "God's Little Acre" 'Follow Your Heart ' 'A Wrinkle in Time'
     'Long Walk to Freedom' 'The Old Man and the Sea' 'Life After Life'
     'Peyton Place ' 'The Giver' 'Me Before You' 'Norwegian Wood '
     'The Plague ' 'No Longer Human ' "Man's Search for Meaning "
     'The Divine Comedy ' 'The Prophet' 'The Boy in the Striped Pyjamas'
     'The Exorcist' 'The Gruffalo' 'Fifty Shades Darker' 'Tobacco Road'
     "Ronia, the Robber's Daughter" 'The Cat in the Hat'
     'Diana: Her True Story' 'The Help' 'Catch-22' 'The Stranger '
     'Eye of the Needle' 'The Lovely Bones' 'Wild Swans' 'Santa Evita'
     'Night ' 'Confucius from the Heart ' 'The Total Woman'
     'Knowledge-value Revolution ' "Problems in China's Socialist Economy "
     'What Color Is Your Parachute?' 'The Dukan Diet' 'The Joy of Sex'
     'The Gospel According to Peanuts' 'The Subtle Art of Not Giving a Fuck'
     'Life of Pi' 'The Front Runner' 'The Goal' 'Fahrenheit 451'
     "Angela's Ashes" 'The Story of My Experiments with Truth '
     "Bridget Jones's Diary"]
    174
    

## Finding missing values
[Back to Top](#Contents)

Use the `isnull()` function to identify null values in the data. Run the `df.isnull().sum()` to retrieve the number of missing records in each column.


```python
df.isnull().sum()
```




    Book         0
    Authors      0
    Language     0
    Year         0
    Sales        0
    Genre       56
    dtype: int64



## Deleting empty rows<a class="anchor" id="Deleting-empty-rows"></a>
[Back to Top](#Contents)

The Genre column contains 56 empty rows. These blank rows do not add any value to the data analysis. Therefore, you must delete these rows to get accurate data analysis. Run the `df.dropna(how='any',inplace=True)` command to delete the empty rows.


```python
#deleting empty/null rows from the Genre column. It has 56 empty rows.
df.dropna(how='any',inplace=True)
```

## Verifying updated rows<a class="anchor" id="Verifying-updated-rows"></a>
[Back to Top](#Contents)


```python
#checking the NaN values
df.isnull().sum()
```




    Book        0
    Authors     0
    Language    0
    Year        0
    Sales       0
    Genre       0
    dtype: int64



# Retrieving statistics summary<a class="anchor" id="Retrieving-statistics-summary"></a>
[Back to Top](#Contents)

Statistics summary provides a high-level overview of the data to identify outliers, data entry errors, and distribution of data such as determining whether the data follows a normal distribution or exhibits deviation to the left or right.

Use the `describe()` function to retrieve all statistics summary of data that belongs to the numerical data type such as int (Year column) and float64 (Sales column).



```python
df.describe() #to analyze dataset #describe function is applicable only to numerical columns
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Sales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>118.000000</td>
      <td>118.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1961.483051</td>
      <td>38.765254</td>
    </tr>
    <tr>
      <th>std</th>
      <td>44.992636</td>
      <td>30.295672</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1788.000000</td>
      <td>10.400000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1945.000000</td>
      <td>20.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1971.000000</td>
      <td>30.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1992.750000</td>
      <td>50.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2018.000000</td>
      <td>200.000000</td>
    </tr>
  </tbody>
</table>
</div>



# Analyzing univariate data<a class="anchor" id="Analyzing-univariate-data"></a>
[Back to Top](#Contents)

Univariate analysis suggests that you can examine or visualize a single variable individually. You can perform univariate analysis for both categorical and numerical variables. 
* Categorical variables: Visualize using a Count plot, Bar chart, Pie plot, and so on.
* Numerical variables: Visualize using Histogram, Box plot, Density plot, and so on. 

# Identify top-selling books<a class="anchor" id="Identifying-top-selling-books"></a>
[Back to Top](#Contents)

To identify the top selling books, calculate the summary statistics for the `Sales` categorical variable in the DataFrame.


```python
top_selling_books = df.sort_values(by="Sales", ascending=False).head(10)
print ("Top selling books:\n", top_selling_books)
```

    Top selling books:
                                            Book                   Authors  \
    0                      A Tale of Two Cities           Charles Dickens   
    1                        The Little Prince   Antoine de Saint-Exupéry   
    2  Harry Potter and the Philosopher's Stone             J. K. Rowling   
    3                  And Then There Were None           Agatha Christie   
    4                 Dream of the Red Chamber                 Cao Xueqin   
    5                                The Hobbit          J. R. R. Tolkien   
    6      The Lion, the Witch and the Wardrobe               C. S. Lewis   
    7               She: A History of Adventure          H. Rider Haggard   
    8                         Vardi Wala Gunda         Ved Prakash Sharma   
    9                         The Da Vinci Code                 Dan Brown   
    
      Language  Year  Sales                        Genre  
    0  English  1859  200.0           Historical fiction  
    1   French  1943  200.0                      Novella  
    2  English  1997  120.0                      Fantasy  
    3  English  1939  100.0                      Mystery  
    4  Chinese  1791  100.0                  Family saga  
    5  English  1937  100.0                      Fantasy  
    6  English  1950   85.0  Fantasy, Children's fiction  
    7  English  1887   83.0                    Adventure  
    8    Hindi  1992   80.0                    Detective  
    9  English  2003   80.0             Mystery thriller  
    

## Displaying bar chart of top-selling books <a class="anchor" id="Displaying-bar-chart-of-top-selling-books"></a>
[Back to Top](#Contents)


```python
top_selling_books = df.groupby('Book')['Sales'].sum().nlargest(10)
plt.figure(figsize=(10, 6))
top_selling_books.plot(kind='bar')
plt.xlabel('Book')
plt.ylabel('Total Sales (millions)')
plt.title('Top Books by Sales')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_40_0.png)
    


## Visualizing distribution of sales<a class="anchor" id="Visualizing-distribution-of-sales"></a>
[Back to Top](#Contents)

You can visualize data distribution and identify outliers. From the box plot, you can visualize that approximate sale has occurred between 22 to 50 million.


```python
df.boxplot(column='Sales')
plt.title('Box Plot of approximate sales in millions')
plt.ylabel('Value')
plt.show()
```


    
![png](output_42_0.png)
    


# Analyzing bivariate data <a class="anchor" id="Analyzing-bivariate-data"></a>
[Back to Top](#Contents)

To perform bivariate data analysis, you can view the Scatter plot for numerical variables such as Year and Sales.

## Identifying relationship between sales and year <a class="anchor" id="Identifying-relationship-between-sales-and-year"></a>
[Back to Top](#Contents)

In the following scatter plot, you can infer the following points:
* The sale is high between the years 1950 to 2000.
* The maximum sale value is between 25 to 75 million.


```python
plt.scatter(df['Year'], df['Sales'])
plt.title('Scatter Plot')
plt.xlabel('Published year')
plt.ylabel('Approximate sales in millions')
plt.show()
```


    
![png](output_45_0.png)
    


## Identifying top authors by sales <a class="anchor" id="Identifying-top-authors-by-sales"></a>
[Back to Top](#Contents)

Use a  bar plot to show the relationship between the categorical variable (Author) and numerical variable (Sales). Identify the top authors by Sales.


```python
top_authors = df.groupby('Authors')['Sales'].sum().nlargest(10)
plt.figure(figsize=(10, 6))
top_authors.plot(kind='bar')
plt.xlabel('Authors')
plt.ylabel('Total Sales (millions)')
plt.title('Top Authors by Sales')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_47_0.png)
    


## Identifying total sales by genre <a class="anchor" id="Identifying-total-sales-by-genre"></a>
[Back to Top](#Contents)

Determine which genre performs better in terms of total sales.


```python
genre_sales = df.groupby('Genre')['Sales'].sum().reset_index()
# Sort the genres by total sales in descending order
genre_sales = genre_sales.sort_values(by='Sales', ascending=False)

# Set the figure size for the bar chart
plt.figure(figsize=(15, 6))

# Create a bar chart to visualize genre vs. total sales
plt.bar(genre_sales['Genre'], genre_sales['Sales'])
plt.xlabel('Genre')
plt.ylabel('Total Sales')
plt.title('Total Sales by Genre')

# Rotate x-axis labels for better readability (optional)
plt.xticks(rotation=90)

# Show the plot
plt.show()
```


    
![png](output_49_0.png)
    


## Identifying total sales by book <a class="anchor" id="Identifying-total-sales-by-book"></a>
[Back to Top](#Contents)

Identify the books that have the highest sales in proportion to the total sales.


```python
genre_sales = df.groupby('Book')['Sales'].sum().reset_index()
# Sort the genres by total sales in descending order
genre_sales = genre_sales.sort_values(by='Sales', ascending=False)

# Set the figure size for the bar chart
plt.figure(figsize=(20, 8))

# Create a bar chart to visualize genre vs. total sales
plt.bar(genre_sales['Book'], genre_sales['Sales'])
plt.xlabel('Genre')
plt.ylabel('Total Sales')
plt.title('Total Sales by Book')

# Rotate x-axis labels for better readability (optional)
plt.xticks(rotation=90)

# Show the plot
plt.show()
```


    
![png](output_51_0.png)
    


## Determining language with the highest sales <a class="anchor" id="Determining-language-with-the-highest-sales"></a>
[Back to Top](#Contents)

Find out which language has the highest sales with the total sales. The bar plot shows that the books in the English language have the highest sales.


```python
sales_by_language = df.groupby('Language')['Sales'].sum().reset_index()
plt.figure(figsize=(8, 6))
sns.barplot(data=sales_by_language, x='Language', y='Sales')
plt.xlabel('Language')
plt.ylabel('Total Sales (millions)')
plt.title('Sales by Language')
plt.xticks(rotation=45)
plt.show()
```

    C:\Users\Kalpana\anaconda3\envs\py3115\Lib\site-packages\seaborn\_oldcore.py:1498: FutureWarning:
    
    is_categorical_dtype is deprecated and will be removed in a future version. Use isinstance(dtype, CategoricalDtype) instead
    
    C:\Users\Kalpana\anaconda3\envs\py3115\Lib\site-packages\seaborn\_oldcore.py:1498: FutureWarning:
    
    is_categorical_dtype is deprecated and will be removed in a future version. Use isinstance(dtype, CategoricalDtype) instead
    
    C:\Users\Kalpana\anaconda3\envs\py3115\Lib\site-packages\seaborn\_oldcore.py:1498: FutureWarning:
    
    is_categorical_dtype is deprecated and will be removed in a future version. Use isinstance(dtype, CategoricalDtype) instead
    
    


    
![png](output_53_1.png)
    


## Determining sales distribution by language  <a class="anchor" id="Determining-sales-distribution-by-language"></a>
[Back to Top](#Contents)

The following pie chart shows the distribution of sales in percentage by language.


```python
sales_by_language = df.groupby('Language')['Sales'].sum().reset_index()
plt.figure(figsize=(13, 13))
plt.pie(sales_by_language['Sales'], labels=sales_by_language['Language'], autopct='%1.1f%%')
plt.title('Sales Distribution by Language')
plt.show()
```


    
![png](output_55_0.png)
    


## Identifying sales over time <a class="anchor" id="Identifying-sales-over-time"></a>
[Back to Top](#Contents)

To identify sales over time, use the 'Sales' continuous variable. The following code creates a simple line chart showing sales over time per year.



```python
df['Year'] = pd.to_datetime(df['Year'])
df = df.sort_values('Year')
plt.figure(figsize=(10, 6))
plt.plot(df['Year'], df['Sales'])
plt.xlabel('Published year')
plt.ylabel('Approximate sales in millions')
plt.title('Sales Over Time')
plt.xticks(rotation=45)
plt.show()
```


    
![png](output_57_0.png)
    


# Conclusion<a class="anchor" id="Conclusion"></a>
[Back to Top](#Contents)

From the exploratory data analysis conducted on the _Best-selling-books_ dataset, the following conclusions emerge:

* The books categorized under the English language consistently achieve the highest sales figures.

* The approximate sales range falls between 22 and 50 million units.

* Sales remained consistently high during the year from 1950 to 2000.

* The peak sales value ranges from 25 to 75 million units.

* The Fantasy genre outperforms other genres in terms of sales.

* Books named **The Little Prince** and **A Tale of Two Cities** consistently attain the highest sales as a proportion of total sales.

* The Sales value fluctuates during the year 1970.

You can use these points to make informed decisions, optimize sales strategies, and achieve financial goals.
