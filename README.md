# Experiment 4: Data Wrangling and Data Visualization  
## Negranza, Jake Andrei D.  
## 2ECE-A  
## September 18, 2026  
---
### Intended Learning Outcomes
At the end of this laboratory activity, the student should be able to:
1. filter tabular data using several categorical and numerical conditions;
2. construct focused DataFrames by selecting relevant features;
3. summarize the relationship between categorical features and a numerical variable; and
4. communicate a data comparison using clear and correctly labeled plots.
---
### Instructions
Use the same ECE Board Exam 2 dataset supplied for Experiment 4. Work in a Jupyter Notebook
using Pandas and a Python plotting library used in class. Use the dataset’s existing column labels,
including Name, Gender, Track, Hometown, Math, GEAS, Electronics, and Average.  
• Derive all tables and plot values from the dataset. Do not manually type rows, category means, or
plotted values.  
• When applying more than one condition, make every condition explicit in the filtering expression.  
• Keep the original DataFrame unchanged.  
• Every graph must have a title, axis labels, readable category labels, and a consistent scale appro-
priate to the data.  

---
### Setup and Data Loading
This code imports required libraries, loads `board2.csv`, and computes the student average scores.
```
import pandas as pd
import matplotlib.pyplot as plt
# Load dataset
board = pd.read_csv('board2.csv')

# Calculate the Average column if not already present in board2.csv
board['Average'] = board[
        ['Math', 'GEAS', 'Electronics', 'Communication']
    ].mean(axis=1)
```
---
### A. Visayas Communication Dataframe
Filters for students whose Hometown is Visayas and Track is Communication, displays the dataset and total row count.
```
VisComm = board[
    (board['Hometown'] == 'Visayas') & (board['Track'] == 'Communication')
][['Name', 'Gender', 'Math', 'Electronics', 'Average']]

# Display DataFrame and row count
print("--- VisComm DataFrame ---")
print(VisComm)
print(f"\nTotal rows in VisComm: {len(VisComm)}")
```
### Sample Output:
<img width="401" height="197" alt="image" src="https://github.com/user-attachments/assets/4b91fb40-6cb2-4dbd-875b-5c2da77263be" />

---
### B. Visayas Female Dataframe
Filters for female students from Visayas, then displays a second view showing only those with an Average score of at least 60.
```
# Create VisFemale DataFrame
VisFemale = board[
    (board['Hometown'] == 'Visayas') & (board['Gender'] == 'Female')
][['Name', 'Track', 'GEAS', 'Electronics', 'Average']]

print("--- VisFemale DataFrame ---")
print(VisFemale)

# Filter VisFemale for Average >= 60 without overwriting the original VisFemale
VisFemale_passed = VisFemale[VisFemale['Average'] >= 60]

print("\n--- VisFemale DataFrame (Average >= 60) ---")
print(VisFemale_passed)
```
### Sample Output:
<img width="492" height="323" alt="image" src="https://github.com/user-attachments/assets/52168e3c-d325-4b13-8d54-be3189480d50" />

---
### C. Category-Average Visualization
Computes mean average scores by Track, Gender, and Hometown, plots a three-chart comparison figure, and prints key sample observations.
```
# --- a & b: Compute means and display summary tables ---
track_summary = board.groupby('Track')['Average'].mean().reset_index()
gender_summary = board.groupby('Gender')['Average'].mean().reset_index()
hometown_summary = board.groupby('Hometown')['Average'].mean().reset_index()

print("--- Mean Average by Track ---")
print(track_summary)
print("\n--- Mean Average by Gender ---")
print(gender_summary)
print("\n--- Mean Average by Hometown ---")
print(hometown_summary)

# --- c: Create one figure with three bar charts ---
fig, axes = plt.subplots(1, 3, figsize=(15, 5), sharey=True)
colors = ['#800000', '#7442C8', '#355E3B']

# Track Chart
axes[0].bar(
    track_summary['Track'],
    track_summary['Average'],
    color=colors[0],
    edgecolor='black',
)
axes[0].set_title('Mean Average by Track')
axes[0].set_xlabel('Track')
axes[0].set_ylabel('Mean Average Score')
axes[0].set_ylim(0, 100)

# Gender Chart
axes[1].bar(
    gender_summary['Gender'],
    gender_summary['Average'],
    color=colors[1],
    edgecolor='black',
)
axes[1].set_title('Mean Average by Gender')
axes[1].set_xlabel('Gender')

# Hometown Chart
axes[2].bar(
    hometown_summary['Hometown'],
    hometown_summary['Average'],
    color=colors[2],
    edgecolor='black',
)
axes[2].set_title('Mean Average by Hometown')
axes[2].set_xlabel('Hometown')

plt.suptitle('Comparison of Mean Board Exam Average Scores Across Categories')
plt.tight_layout()
plt.show()
```
### Sample Output:
<img width="302" height="352" alt="image" src="https://github.com/user-attachments/assets/57694db5-1140-4bf7-aadf-fa3943f3dc27" />
<img width="1370" height="457" alt="image" src="https://github.com/user-attachments/assets/b8ad7e72-8ad4-43e7-931d-8e822e34be8e" />  

### Interpretation Statements for the output above  
**Track**: Among the specialization tracks, the Communication track recorded the highest sample mean board-exam score at approximately 67.98.  
**Gender**: Between the gender categories, Male students recorded the highest sample mean board-exam score at approximately 67.18.  
**Hometown**: Among the regional hometown groups, students from Luzon recorded the highest sample mean board-exam score at approximately 68.08.  

---
# THE END
