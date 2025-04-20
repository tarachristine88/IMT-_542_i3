# IMT_542_i3
IMT 542 - Tara Hulcome

## Overview
For this IMT 542 task (I3), I will walk through the process of integrating and visualizing scores from the 2025 NCAA Women's College Gymnastics Championships, held on 17-19 April 2025 in Dallas-Fort Worth TX. This project is useful as it allows the user to synthesize and interrogate data from 320 individual events across 3 separate competitions (including semi-final and final rounds). The user will be able to combine and transform the three datasets, comparing scores achieved by the 8 final teams and individual athletes.

## Instructions

**Step 1.**
Find updated links to the 2025 NCAA Women's College Gymnastics Championships, via Balance Beam Situation website: https://balancebeamsituation.com/2025/04/16/2025-nationals-schedule-links/

**Step 2.**
Follow links to Virtius scoring platform for each of the three competition rounds:
•	Semifinal I competition scores: https://virti.us/session?s=xhZmjBrMse 
•	Semifinal II competition scores: https://virti.us/session?s=3mxRqdyuqJ 
•	National Championship scores: https://virti.us/session?s=jbwIq0qS0k 

**Step 3.**
As Virtius' Terms of Service do not permit web crawling / scraping activities, manually save full competition data from the ‘stats’ tab. Convert to CSV format and save locally. 

**Step 4**
Copy the 3 CSV files to Google CoLab and begin coding, using the Gemini AI assistant if required (as per code examples shown below). The specific task is to experiment with creating scatter plot graphs that show the range of scores achieved across all four events, and comparing the two strongest teams (UCLA and Oklahoma).
![image](https://github.com/user-attachments/assets/5810492d-7f08-4725-8235-2d560b6ab66d)


## Code example

# prompt: combine three csv files

import pandas as pd

# Replace 'file1.csv', 'file2.csv', and 'file3.csv' with your actual file names
file1 = 'NCAA_25_National Championship.csv'
file2 = 'NCAA_25_Semifinal_I.csv'
file3 = 'NCAA_25_Semifinal_II.csv'

# Create an empty list to store dataframes
dfs = []

# Read each CSV file into a dataframe and append it to the list
for file in [file1, file2, file3]:
    try:
        df = pd.read_csv(file)
        dfs.append(df)
    except FileNotFoundError:
        print(f"Error: File '{file}' not found.")

# Concatenate all dataframes in the list into a single dataframe
if dfs:  # Check if the list is not empty
  combined_df = pd.concat(dfs, ignore_index=True)

  # Save the combined dataframe to a new CSV file
  combined_df.to_csv('combined_file.csv', index=False)

  print("CSV files combined successfully into 'combined_file.csv'")
else:
  print("No valid CSV files found.")


# prompt: identify the top 5 scores for each event

import pandas as pd

# Assuming 'combined_file.csv' exists from the previous code
try:
    df = pd.read_csv('combined_file.csv')

    # Group by 'Event' and get the top 5 scores for each event
    top_5_scores = df.groupby('Event').apply(lambda x: x.nlargest(5, 'Score'))

    print(top_5_scores)

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")




# prompt: identify the top 5 scores for each event

import pandas as pd

try:
    df = pd.read_csv('combined_file.csv')

    # Group by 'Event' and get the top 5 scores for each event
    top_5_scores = df.groupby('Event').apply(lambda x: x.nlargest(5, 'Score'))

    print(top_5_scores)

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: identify the top 5 scores for each event

import pandas as pd

# Assuming 'combined_file.csv' exists from the previous code
try:
    df = pd.read_csv('combined_file.csv')

    # Group by 'Event' and get the top 5 scores for each event
    top_5_scores = df.groupby('Event').apply(lambda x: x.nlargest(5, 'Score'))

    print(top_5_scores)

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: For this data, only return the Event, Name, Team and Score

import pandas as pd

try:
    df = pd.read_csv('combined_file.csv')
    #Select only the specified columns
    selected_columns = df[['Event', 'Name', 'Team', 'Score']]
    print(selected_columns)
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: identify the top 5 scores for each event

try:
    df = pd.read_csv('combined_file.csv')
    # Group by 'Event' and get the top 5 scores for each event
    top_5_scores = df.groupby('Event').apply(lambda x: x.nlargest(5, 'Score'))
    #Select only the specified columns
    selected_columns = top_5_scores[['Event', 'Name', 'Team', 'Score']]
    print(selected_columns)
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: identify the lowest five scores for each event

try:
    df = pd.read_csv('combined_file.csv')
    # Group by 'Event' and get the lowest 5 scores for each event
    lowest_5_scores = df.groupby('Event').apply(lambda x: x.nsmallest(5, 'Score'))
    #Select only the specified columns
    selected_columns = lowest_5_scores[['Event', 'Name', 'Team', 'Score']]
    print(selected_columns)
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Visualize the variance between the lowest score and highest score for all four events

import pandas as pd
import matplotlib.pyplot as plt

try:
    df = pd.read_csv('combined_file.csv')
    
    # Group data by event
    event_groups = df.groupby('Event')

    # Calculate the range (variance) for each event
    event_variance = event_groups['Score'].agg(['min', 'max'])
    event_variance['range'] = event_variance['max'] - event_variance['min']

    # Create the bar plot
    plt.figure(figsize=(10, 6))
    plt.bar(event_variance.index, event_variance['range'])
    plt.xlabel('Event')
    plt.ylabel('Score Variance (Max - Min)')
    plt.title('Variance of Scores for Each Event')
    plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for readability
    plt.tight_layout()  # Adjust layout to prevent labels from overlapping
    plt.show()

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Add labels that show the actual lowest and highest scores

try:
    df = pd.read_csv('combined_file.csv')
    
    # Group data by event
    event_groups = df.groupby('Event')

    # Calculate the range (variance) for each event
    event_variance = event_groups['Score'].agg(['min', 'max'])
    event_variance['range'] = event_variance['max'] - event_variance['min']

    # Create the bar plot
    plt.figure(figsize=(10, 6))
    bars = plt.bar(event_variance.index, event_variance['range'])
    plt.xlabel('Event')
    plt.ylabel('Score Variance (Max - Min)')
    plt.title('Variance of Scores for Each Event')
    plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for readability

    # Add labels for min and max scores above each bar
    for bar, min_val, max_val in zip(bars, event_variance['min'], event_variance['max']):
        yval = bar.get_height()
        plt.text(bar.get_x() + bar.get_width()/2, yval, f'Min: {min_val}\nMax: {max_val}', ha='center', va='bottom')

    plt.tight_layout()  # Adjust layout to prevent labels from overlapping
    plt.show()

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Add labels that show the name of the athlete that scored the lowest and highest scores for each event

try:
    df = pd.read_csv('combined_file.csv')
    
    # Group data by event
    event_groups = df.groupby('Event')

    # Calculate the range (variance) for each event
    event_variance = event_groups['Score'].agg(['min', 'max'])
    event_variance['range'] = event_variance['max'] - event_variance['min']

    # Create the bar plot
    plt.figure(figsize=(10, 6))
    bars = plt.bar(event_variance.index, event_variance['range'])
    plt.xlabel('Event')
    plt.ylabel('Score Variance (Max - Min)')
    plt.title('Variance of Scores for Each Event')
    plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for readability

    # Add labels for min and max scores and athlete names above each bar
    for event, min_val, max_val in zip(event_variance.index, event_variance['min'], event_variance['max']):
      min_athlete = df[(df['Event'] == event) & (df['Score'] == min_val)]['Name'].iloc[0]
      max_athlete = df[(df['Event'] == event) & (df['Score'] == max_val)]['Name'].iloc[0]

      yval = event_variance.loc[event, 'range']
      plt.text(event, yval,
              f'Min: {min_val} ({min_athlete})\nMax: {max_val} ({max_athlete})',
              ha='center', va='bottom')

    plt.tight_layout()  # Adjust layout to prevent labels from overlapping
    plt.show()

except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Create a scatter plot that maps the score against event

try:
    df = pd.read_csv('combined_file.csv')
    plt.figure(figsize=(10, 6))
    plt.scatter(df['Event'], df['Score'])
    plt.xlabel('Event')
    plt.ylabel('Score')
    plt.title('Score vs. Event')
    plt.xticks(rotation=45, ha='right')
    plt.tight_layout()
    plt.show()
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Create a scatter plot that maps the score against event for the UCLA team

try:
    df = pd.read_csv('combined_file.csv')
    # Filter data for UCLA team
    ucla_df = df[df['Team'] == 'UCLA']
    
    plt.figure(figsize=(10, 6))
    plt.scatter(ucla_df['Event'], ucla_df['Score'])
    plt.xlabel('Event')
    plt.ylabel('Score')
    plt.title('UCLA Score vs. Event')
    plt.xticks(rotation=45, ha='right')
    plt.tight_layout()
    plt.show()
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")


# prompt: Create a scatter plot that maps the score against event for the Oklahoma team

try:
    df = pd.read_csv('combined_file.csv')
    # Filter data for Oklahoma team
    oklahoma_df = df[df['Team'] == 'Oklahoma']
    
    plt.figure(figsize=(10, 6))
    plt.scatter(oklahoma_df['Event'], oklahoma_df['Score'])
    plt.xlabel('Event')
    plt.ylabel('Score')
    plt.title('Oklahoma Score vs. Event')
    plt.xticks(rotation=45, ha='right')
    plt.tight_layout()
    plt.show()
except FileNotFoundError:
    print("Error: 'combined_file.csv' not found. Please run the previous code block first.")
