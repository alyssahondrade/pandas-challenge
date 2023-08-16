# pandas-challenge
Module 4 Challenge - UWA/edX Data Analytics Bootcamp

Github repository at: [https://github.com/alyssahondrade/pandas-challenge.git](https://github.com/alyssahondrade/pandas-challenge.git)

## Table of Contents
1. [Introduction](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#introduction)
    1. [Goal](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#goal)
    2. [Repository Structure](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#repository-structure)
    3. [Dataset](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#dataset)
2. [Approach](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#approach)
    1. [Initial Dataframe](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#initial-dataframe)
    2. [Local Government Area (LGA) Summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#local-government-area-lga-summary)
    3. [School Summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#school-summary)
    4. [Top and Bottom Performing Schools (by % Overall Passing)](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#top-and-bottom-performing-schools-by--overall-passing)
    5. [Maths and Reading Scores by Year](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#maths-and-reading-scores-by-year)
    6. [Scores by School Spending and School Size](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#scores-by-school-spending-and-school-size)
    7. [Scores by School Type](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#scores-by-school-type)
3. [Analysis & Results](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#analysis--results)
4. [References](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#references)

## Introduction
### Goal
The goal of this analysis is to aggregate the provded data to identify trends in school performance.

The task is broken down into sections, each requiring a dataframe as an output (provided in [Analysis & Results](https://github.com/alyssahondrade/pandas-challenge/blob/main/README.md#analysis--results)).
- Local Government Area (LGA) Summary - `area_summary`
- School Summary - `per_school_summary`
- Highest Performing Schools (by % Overall Passing) - `top_schools`
- Lowest Performing Schools (by % Overall Passing) - `bottom_schools`
- Maths Scores by Year - `maths_scores_by_year`
- Reading Scores by Year - `reading_scores_by_year`
- Scores by School Spending - `spending_summary`
- Scores by School Size - `size_summary`
- Scores by School Type - `type_summary`

### Repository Structure
- `PyCitySchools` directory contains the Jupyter notebook of the same name.
- `Resources` directory contains the datasets: [`schools_complete.csv`](https://github.com/alyssahondrade/pandas-challenge/blob/main/Resources/schools_complete.csv) and [`students_complete.csv`](https://github.com/alyssahondrade/pandas-challenge/blob/main/Resources/students_complete.csv).

### Dataset
The datasets were generated by **Mockaroo, LLC (2022) Realistic Data Generator** and **edX Boot Camps LLC**.

## Approach
### Initial Dataframe
1. Renamed column headings for consistency.
2. Rearranged columns so that student details and school details are separated, for readability.
3. Merge results confirmed using `.head()`

### Local Government Area (LGA) Summary
1. For each requested calculation, set the result to a variable.
2. Pass the variables to a dictionary, then to create a dataframe.
3. Format the **"Total Students"** and **"Total Budget"** columns using the `.map()` function.

### School Summary
1. Create a variable that holds the list of **unique school names**, this list will be enumerated in the for-loop later.
2. Initialise lists to hold column values, this will be converted to a dictionary, then a dataframe.
3. In the for-loop, append each school result to the correct list.
    1. Create a variable that holds `school_name` as a condition. Doing so will minimise repetition, as this will be used in the `.loc` method throughout the for-loop.
    2. For **"School Type"** and **"School Budget"**, access the value of the array at `index = 0`, as the result is an array of length 1.
    3. For percentage results:
        1. Get the passing conditions.
        2. Find the number of students which satisfy the conditions, returning a count of "School ID".
        3. Calculate the percentage by dividing the total students per school.      
4. Create a dictionary of lists and convert it to a dataframe. Use the list of **unique school names** as the index and sort alphabetically.
5. Format the **"Total School Budget"** and **"Per Student Budget"** using the `.style.format()` function instead of the `.map()` function.
    1. `.map()` will change the datatype to an `object`, which is problematic when the dataframe is used for calculations later.
    2. `.style.format()` will only affect the displayed result and not alter the datatype. It is not persistent and will need to be run at every cell that requires formatting of the original dataframe.

### Top and Bottom Performing Schools (by % Overall Passing)
1. Use the dataframe from **School Summary** and sort by **"% Overall Passing"**.
2. For top schools, set `ascending=False`, and for bottom schools, although the default is `True`, set `ascending=True` for clarity.
3. `.head()` will always return the first 5 rows if a number is not specified.

### Maths and Reading Scores by Year
1. Declare a list for each year group.
2. Using a nested for-loop, loop over the list of unique schools, as well as for each year group.
    1. Use if-elif statements to populate the correct list.
    2. Use `.loc` to find all the maths/reading scores for each year in each school.
    3. Calculate the average using `.mean()`.
3. Use the lists to create a Series, grouping by the list of unique school names.
    - As it is a `groupby()` object, a function is required to access the data. It does not matter whether `.sum()` or `.mean()` is used as each cell in the dataframe contains only 1 value. However, `.mean()` is used as it is most logical.
4. Combine the series into a dataframe using the `pd.concat()` function about `axis=1`.

### Scores by School Spending and School Size
1. Use the provided code to create the bins and labels.
2. Get a dataframe with all the required columns using `.loc` on the `per_school_summary` dataframe from **School Summary**.
3. Use `pd.cut()` to bin the **"Per Student Budget"** column (or **"School Size"** column), applying the correct bins and labels.
4. Set the binned column as the index and update the index name.
5. Condense the rows to the correct ranges by using the `groupby().mean()`.

### Scores by School Type
1. Create a dataframe from `per_school_summary` with all the required columns, including **"School Type"**, as this is needed to group-by later.
2. Use `groupby().mean()` over the **"School Type"**.

## Analysis & Results
### LGA Summary
The `area_summary` results provide a total average across all schools and students. This can be used as the baseline "average" when comparing detailed results later.

![area_summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/area_summary.png)

### School Summary
The `per_school_summary` results provides an overview of each school's results and budget per student, including percentage passing rates, and identifies the school type. This data can be used as a basis for drilling down to identify trends in greater detail.

![per_school_summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/per_school_summary.png)

### Highest and Lowest Performing Schools
1. Of the Top 5 performing schools, 3 are independent schools and 2 are government schools. All scores fall above the population average, as per `area_summary` results.

| ![top_schools](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/top_schools.png) |
|:--:|
| Highest Performing Schools |

2. Of the Bottom 5 performing schools, 1 is an independent school and 4 are government schools. All scores fall below the population average, as per `area_summary` results.

| ![bottom_schools](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/bottom_schools.png) |
|:--:|
| Lowest Performing Schools |

3. Given the results of `top_schools` and `bottom_schools`, it is not evident whether school size or budget per student has a relationship with student scores. However, the results do imply that Independent schools tend to perform better than Government schools.

### Maths and Reading Scores by Year
|![maths_scores_by_year](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/maths_scores_by_year.png)|![reading_scores_by_year](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/reading_scores_by_year.png)|
|:--:|:--:|
| Maths Scores by Year | Reading Scores by Year |

The trends for both maths and readings scores can be further explored by:
1. Calculating the standard deviation for each school to confirm the spread of scores, and
2. Plotting the progression from Year 9 to 12 to identify whether scores improve or deteriorate over time.

For simplicity, the results for Year 9 can be compared to Year 12 results as a snapshot of school cohort performance.
1. In terms of maths scores, 8 schools performed worse in Year 12, of which 5 were Independent schools. Of the remaining 7 schools which performed better in Year 12, 4 were Government schools and 3 were Independent.

2. In terms of reading scores, there were very similar results, with the exception of 1 school which had an identical Year 9 and Year 12 score.

3. For maths scores by year, schools performed relatively consistently across each year group. However, there appears to be a slightly higher spread for each school in reading scores (i.e. reading scores had greater fluctuation throughout Year 9 through to Year 12 compared to maths scores).

Despite not being a longitudinal study of the same cohort over 4 years, the conclusions drawn above are useful as schools can place better attention on a specific year group if it is identified scores deteriorate in that year. It could be that students find the content of that year more challenging than in other years, and the school can provide support accordingly.

### Scores by School Spending
Scores across the board peak when the budget per student spending is in the $585-$630 range. This implies that this is the optimal range, that spending more money per student does not affect scores beyond this given range.

|![spending_summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/spending_summary.png)|
|:--:|
| Scores by School Spending |

### Scores by School Size
It is evident that schools with <1,000 students perform better. Class sizes would need to be further investigated, but if it can be assumed that smaller school sizes correlates to smaller class sizes, then this would imply teachers can spend more time per student. This is in line with the results of the Tennessee Student-Teacher Achievement Ratio (STAR) project.

|![size_summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/size_summary.png)|
|:--:|
| Scores by School Size |

### Scores by School Type
Independent schools perform better than Government schools. This confirm the observation made in the **Highest and Lowest Performing Schools** results.

|![type_summary](https://github.com/alyssahondrade/pandas-challenge/blob/main/Images/type_summary.png)|
|:--:|
| Scores by School Type |

## References

