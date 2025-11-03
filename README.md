# Exam 2 R

We will be analyzing the `age_count_2020-07-13_2020-10-11.xlsx` data set from lecture 8. This data consists of the date that a COVID-19 test from somebody residing in Nueces County comes back positive (LABDATE) the age of the person (AGE_YEARS). Each row is a person.  There are 4 worksheets in the excel workbook, one per month from July to October.

Your goal is to complete the tasks and create an R script that will work when this repo is cloned to any computer. I encourage you to make tidyverse pipelines, where the responses from several questions are assembled in one or a few pipelines.

As in exam 1, I also want you to copy and paste each question into your script as a comment so I can tell which question you are trying to answer with which lines of code. 

When you are complete, submit the exam by pushing the changes to github (you may do this several times).

> [!IMPORTANT]
> I will take off points for AI slop - [see definition](https://en.wikipedia.org/wiki/AI_slop).  You can use AI, but if the code you submit is overly convoluted using many more lines of code than is necessary, with many more features than are asked for I will deduct points even if the code works.  It is your job to stop AI slop.

> [!CAUTION]
> Read each question carefully.  I have slightly altered the wording of each questionto exclude certain types of [AI slop]((https://en.wikipedia.org/wiki/AI_slop) as valid answers. Further, mind your paths.  If I have to alter you code to make it work on my computer, then I will deduct points.

---


#### 0. Create an R script named `ageCovidSummary.R` in your exam repo and set the working directory with the following command:

```r 
setwd(dirname(rstudioapi::getActiveDocumentContext()$path))
```


#### 1. Load the following libraries (and install if necessary):

  * tidyverse
  
  * readxl
  
  * janitor
  
  * lubridate

#### 2. Referring to lecture 8 (some code will need to be altered), read the data from `age_count_2020-07-13_2020-10-11.xlsx` into a tibble named `covid_cases_age` and then do the following in a single pipeline using tidyverse commands:

  * format the column names
  
  * make a new column called `date` and format it as YYYY-MM-DD using `ymd()`
  
  * make a new column called `age_class` that evaluates the the values in `age_years` and assigns them to the proper 20 yr age bin:
  
    * 0-19, 20-39, 40-59, 60-79, 80+
  
    * use `case_when()` for this
  
  * group the tibble by `date` and `age_class`
  
  * use `summarise()` to transform the tibble so that each row is a unique combination of `date` and `age_class`, and the number of positive cases in each age class on each date is stored in a column called `new_cases`
  
    * If done properly, this is how `covid_cases_age` will look:
  
    ```r
    > covid_cases_age
    # A tibble: 433 x 3
    # Groups:   date [91]
       date       age_class new_cases
       <date>     <chr>         <int>
     1 2020-07-13 0-19             44
     2 2020-07-13 20-39            73
     3 2020-07-13 40-59            84
     4 2020-07-13 60-79            49
     5 2020-07-13 80+               3
     6 2020-07-14 0-19             61
     7 2020-07-14 20-39           213
     8 2020-07-14 40-59           219
     9 2020-07-14 60-79           102
    10 2020-07-14 80+              10
    # ... with 423 more rows
    ```

___


#### 3. Create the following plot from `covid_cases_age` using `ggplot2`

![](nueces_new-cases_age-class.png)


___

#### 4. Recreate the following plot from `covid_cases_age` using `ggplot2`.  Hints: 
  
  * refer to lecture 8 where we used functions to add day of week and month to a tibble
  
  * refer to text book or search web for solution to making error bars, you will need to calculate the mean and sd for each row before initiating the plot
  
  * search the web for solution to allowing y axes to freely vary depending upon age class
  
  * the [R Graphics Cookbook](http://www.cookbook-r.com/Graphs/) could be useful here

![](nueces_mean-new-cases_day-ageclass.png)

___


#### 5. There are different numbers of people in the age brackets and thus we might expect more positive cases in some age brackets than others.  Using tidyverse commands, read in the `Texas_Age_Demographic_Data.csv` file and process it down to a tibble named `nueces_demographics` with just the total number of people in each 20 year age bracket in Nueces county. The tibble should have 5 rows and 2 columns.  Name the columns `age_class` and `num_people` as follows:

```r 
> nueces_demographics
# A tibble: 5 x 2
  age_class num_people
  <chr>          <dbl>
1 0-19           64181
2 20-39         114717
3 40-59          95495
4 60-79          70266
5 80+            16584
```

___


#### 6. Recreate the following plot with the data in the `nueces_demographics` tibble and `ggplot2`.  The font size of the axis titles is 20 and the font size of the axis values is 18.  Hint: the [R Graphics Cookbook](http://www.cookbook-r.com/Graphs/) could be useful here.

![](nueces_num-people_age-class.png)

___


#### 7. Now we can use the demographic data to calculate the number of new cases relative to the number of people in each age class.  Using tidyverse commands, join `covid_cases_age` and `nueces_demographics` together and save the new tibble as `covid_cases_age_census`. Add a column named `new_cases_per10k` with values calculated as follows: `10000*new_cases/num_people`.  The result will be a tibble like `covid_cases_age` but with 2 additional columns :

```r 
> covid_cases_age_census
# A tibble: 433 x 5
# Groups:   date [91]
   date       age_class new_cases num_people new_cases_per10k
   <date>     <chr>         <int>      <dbl>            <dbl>
 1 2020-07-13 0-19             44      64181             6.86
 2 2020-07-13 20-39            73     114717             6.36
 3 2020-07-13 40-59            84      95495             8.80
 4 2020-07-13 60-79            49      70266             6.97
 5 2020-07-13 80+               3      16584             1.81
 6 2020-07-14 0-19             61      64181             9.50
 7 2020-07-14 20-39           213     114717            18.6 
 8 2020-07-14 40-59           219      95495            22.9 
 9 2020-07-14 60-79           102      70266            14.5 
10 2020-07-14 80+              10      16584             6.03
# ... with 423 more rows
```

___


#### 8. I have noticed a pattern where the elderly are the last age group to experience a spike in COVID cases after a local outbreak.  Modify your code from question 3 above using tidyverse commands and `ggplot2` to make a plot with `new_cases_per10k` on the y axis.  Is the figure consistent with my observations from other time periods? Which figure, this one or the one from question 3 better portrays the level of COVID infection within and among age classes?  Why?

![](nueces_new-cases-per10k_age-class.png)

___


#### 9. Extra Credit (worth 25% of total exam).  Convert the present file into an R Markdown file named `ageCovidSummary.Rmd`
