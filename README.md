# pandas_challenge



# Dependencies and Setup
import pandas as pd
from pathlib import Path

# File to Load (Remember to Change These)
school_data_to_load = Path("Resources/schools_complete.csv")
student_data_to_load = Path("Resources/students_complete.csv")
# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete
## District Summary
# Calculate the total number of unique schools
total_schools = len(school_data_complete.school_name.unique())
total_schools
# Calculate the total number of students
total_students = school_data_complete["school_name"].value_counts().dropna()
total_students
# Calculate the total budget
total_budget = sum(school_data_complete.budget.unique())
total_budget
# Calculate the average (mean) math score
average_math_score = school_data_complete.math_score.mean()
average_math_score
# Calculate the average (mean) reading score
average_reading_score = school_data_complete.reading_score.mean()
average_reading_score
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)


pass_math_percentage = (school_data_complete['math_score'] >= 70).mean() * 100
pass_math_percentage
# Calculate the percentage of students who passed reading (hint: look at how the math percentage was calculated)  
pass_reading_percentage = (school_data_complete['reading_score'] >= 70).mean() * 100
pass_reading_percentage
school_types = school_data.set_index(["school_name"])["type"]
school_types
# Use the following to calculate the percentage of students that passed math and reading

overall_passing_percentage = (pass_math_percentage + pass_reading_percentage)/2
overall_passing_percentage
per_student_budget = total_budget / total_students
# Create a high-level snapshot of the district's key metrics in a DataFrame
school_summary = pd.DataFrame({"Total Schools":[total_schools],"School Type":[school_types],"Total Students":[total_students], "Total Budget":[total_budget],"Per Student Budget":[per_student_budget], "Average Math Score":[average_math_score], "Average Reading Score":[average_reading_score], "% passing math":[pass_math_percentage],"% passing reading":[pass_reading_percentage],"% overall passing":[overall_passing_percentage]})

# Formatting
school_Summary = pd.DataFrame({"School Name":[total_schools],"School Type":[school_types], "Total Students":[total_students],"Total Budget":[total_budget],"Per Student Budget":[per_student_budget],
                                "Average Math Score":[average_math_score], "Average Reading Score":[average_reading_score], "% Passing Math":[pass_math_percentage],
                                "% Passing Reading":[pass_reading_percentage],"% Overall Passing Rate":[overall_passing_percentage]})

# Display the DataFrame
school_summary
## School Summary
# Use the code provided to select all of the school types
school_types = school_data.set_index(["school_name"])["type"]
school_types
# Calculate the total student count per school
per_school_counts = school_data_complete["school_name"].value_counts().dropna()
per_school_counts
# Calculate the total school budget and per capita spending per school
per_school_budget = school_data.set_index("school_name")["budget"].dropna()
per_school_capita = per_school_budget / per_school_counts


# Calculate the average test scores per school
per_school_math = school_data_complete.groupby("school_name")["math_score"].mean().dropna()
per_school_math
per_school_reading = school_data_complete.groupby("school_name")["reading_score"].mean().dropna()
per_school_reading
# Calculate the number of students per school with math scores of 70 or higher
students_passing_math = school_data_complete.loc[school_data_complete["math_score"]>=70]
school_students_passing_math = students_passing_math.groupby(["school_name"]).size()
school_students_passing_math

# Calculate the number of students per school with reading scores of 70 or higher
students_passing_reading = school_data_complete.loc[school_data_complete["reading_score"]>=70]
school_students_passing_reading = students_passing_reading.groupby(["school_name"]).size()
# Use the provided code to calculate the number of students per school that passed both math and reading with scores of 70 or higher
students_passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
school_students_passing_math_and_reading = students_passing_math_and_reading.groupby(["school_name"]).size()
school_students_passing_math_and_reading
# Use the provided code to calculate the passing rates
overall_passing_percentage_per_school = (school_students_passing_reading + school_students_passing_math)/2
overall_passing_percentage_per_school

# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame({"School Type" : school_types,"Total Students" : per_school_counts,"Total School Budget" : per_school_budget,"Per Student Budget" : per_school_capita,"Average Math Score" : per_school_math,"Average Reading Score" : per_school_reading.dropna(), "% Passing Math" : school_students_passing_math,"% Passing Reading" : school_students_passing_reading,"% Overall Passing" : overall_passing_percentage_per_school})

# Formatting
per_school_summary_formatted = per_school_summary.style.format({
    "Total Students": "{:,}",
    "Total School Budget": "${:,.2f}",
    "Per Student Budget": "${:,.2f}",
    "Average Math Score": "{:.2f}",
    "Average Reading Score": "{:.2f}",
    "% Passing Math": "{:.2f}%",
    "% Passing Reading": "{:.2f}%",
    "% Overall Passing": "{:.1f}%"
})



# Display the DataFrame
per_school_summary
## Highest-Performing Schools (by % Overall Passing)
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows.
top_schools = per_school_summary.sort_values("% Overall Passing", ascending=False).head(5)
## Bottom Performing Schools (By % Overall Passing)
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
bottom_schools = per_school_summary.sort_values("% Overall Passing").head(5)
## Math Scores by Grade
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by `school_name` and take the mean of the `math_score` column for each.
ninth_grade_math_scores = ninth_graders.groupby(["school_name"])['math_score'].mean()
tenth_grader_math_scores = tenth_graders.groupby(["school_name"])['math_score'].mean()
eleventh_grader_math_scores = eleventh_graders.groupby(["school_name"])['math_score'].mean()
twelfth_grader_math_scores = twelfth_graders.groupby(["school_name"])['math_score'].mean()

# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
math_scores_by_grade = pd.DataFrame({"9th": ninth_grade_math_scores,
                                        "10th": tenth_grader_math_scores,
                                        "11th": eleventh_grader_math_scores,
                                        "12th": twelfth_grader_math_scores})

# Minor data wrangling
math_scores_by_grade.index.name = None

# Display the DataFrame
math_scores_by_grade
## Reading Score by Grade 
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by `school_name` and take the mean of the the `reading_score` column for each.
ninth_grade_reading_scores = ninth_graders.groupby(["school_name"])['reading_score'].mean()
tenth_grader_reading_scores = tenth_graders.groupby(["school_name"])['reading_score'].mean()
eleventh_grader_reading_scores = eleventh_graders.groupby(["school_name"])['reading_score'].mean()
twelfth_grader_reading_scores = twelfth_graders.groupby(["school_name"])['reading_score'].mean()

# Combine each of the scores above into single DataFrame called `reading_scores_by_grade`
reading_scores_by_grade = pd.DataFrame({"9th": ninth_grade_reading_scores,
                                        "10th": tenth_grader_reading_scores,
                                        "11th": eleventh_grader_reading_scores,
                                        "12th": twelfth_grader_reading_scores})

# Minor data wrangling
reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None

# Display the DataFrame
reading_scores_by_grade
## Scores by School Spending
# Establish the bins 
bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]
# Create a copy of the school summary since it has the "Per Student Budget" 
school_spending_df = per_school_summary
# Use `pd.cut` to categorize spending based on the bins.
school_spending_df['Spending Ranges (Per Student)'] = pd.cut(school_spending_df['Per Student Budget'], bins=bins, labels=labels)
#  Calculate averages for the desired columns. 
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()
# Assemble into DataFrame
spending_summary = pd.DataFrame({"Average Math Score":spending_math_scores,"Average Reading Score":spending_reading_scores, "% passing math":spending_passing_math, "% passing reading":spending_passing_reading, "% overall passing":overall_passing_spending })

# Display results
spending_summary
## Scores by School Size
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
# Categorize the spending based on the bins
# Use `pd.cut` on the "Total Students" column of the `per_school_summary` DataFrame.


per_school_summary["School Size"] = pd.cut(per_school_summary["Total Students"], size_bins, labels=labels)
per_school_summary
# Calculate averages for the desired columns. 
size_math_scores = per_school_summary.groupby(["School Size"])["Average Math Score"].mean()
size_reading_scores = per_school_summary.groupby(["School Size"])["Average Reading Score"].mean()
size_passing_math = per_school_summary.groupby(["School Size"])["% Passing Math"].mean()
size_passing_reading = per_school_summary.groupby(["School Size"])["% Passing Reading"].mean()
size_overall_passing = per_school_summary.groupby(["School Size"])["% Overall Passing"].mean()
# Create a DataFrame called `size_summary` that breaks down school performance based on school size (small, medium, or large).
# Use the scores above to create a new DataFrame called `size_summary`

size_summary = pd.DataFrame({"Math Scores":size_math_scores,"Reading Scores":size_reading_scores, "% Passing Math":size_passing_math, "% Passing Reading":size_passing_reading, "% Overall Passing":size_overall_passing})

# Display results
size_summary
## Scores by School Type
# Group the per_school_summary DataFrame by "School Type" and average the results.
average_math_score_by_type = per_school_summary.groupby(["School Type"])["Average Math Score"].mean()
average_reading_score_by_type = per_school_summary.groupby(["School Type"])["Average Reading Score"].mean()
average_percent_passing_math_by_type = per_school_summary.groupby(["School Type"])["% Passing Math"].mean()
average_percent_passing_reading_by_type = per_school_summary.groupby(["School Type"])["% Passing Reading"].mean()
average_percent_overall_passing_by_type = per_school_summary.groupby(["School Type"])["% Overall Passing"].mean()
# Assemble the new data by type into a DataFrame called `type_summary`
type_summary = pd.DataFrame({"Average Math Score":average_math_score_by_type ,
"Average Reading Score":average_reading_score_by_type ,
"Average % passing math": average_percent_passing_math_by_type,
"Average % passing Reading":average_percent_passing_reading_by_type ,
"Average % overall passing": average_percent_overall_passing_by_type })

# Display results
type_summary
