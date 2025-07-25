# Data Visualization

## Assignment 3: Final Project

### Requirements:
- We will finish this class by giving you the chance to use what you have learned in a practical context, by creating data visualizations from raw data. 
- Choose a dataset of interest from the [City of Toronto’s Open Data Portal](https://www.toronto.ca/city-government/data-research-maps/open-data/) or [Ontario’s Open Data Catalogue](https://data.ontario.ca/). 
- Using Python and one other data visualization software (Excel or free alternative, Tableau Public, any other tool you prefer), create two distinct visualizations from your dataset of choice.  
- For each visualization, describe and justify: 
    > What software did you use to create your data visualization?

    I used Python, primarily within Visual Studio Code, to create my data visualizations. The core of the visualization was built using the Plotly library, specifically plotly.express for generating the graphs and plotly.graph_objects for creating a more customized bar chart. I also used libraries like pandas for data manipulation and requests to fetch data from an external GeoJSON source from the City of Toronto.

    > Who is your intended audience? 
    My intended audience includes Toronto city planners, public transit officials, and urban development policymakers. The visualizations are also accessible to the general public, particularly local residents and cycling advocacy groups, who are interested in understanding and advocating for cycling infrastructure. The goal is to provide a clear, data-driven perspective on cycling trends that can inform decisions and spark public discussion.
    
    > What information or message are you trying to convey with your visualization? 
    I'm trying to convey the significant and varying cycling volume across Toronto and highlight the seasonal trends. The main messages are:
    
    (1) Cycling is a major mode of transport in Toronto, with certain locations showing very high annual volumes.
    
    (2) Cycling volume is highly seasonal, with a clear peak during the warmer months of summer and a significant drop in winter.
    
    The data can inform resource allocation, showing which locations have the highest demand and where infrastructure improvements (like dedicated bike lanes or permanent counters) might be most impactful.
    
    > What aspects of design did you consider when making your visualization? How did you apply them? With what elements of your plots? 
    I considered several key design principles to ensure clarity and effectiveness:
    
    Clarity and Simplicity: I used a simple bar chart to show seasonal volume, which is intuitive and easy to read. For the map, I chose a scatter plot (px.scatter_mapbox) to show the intersections and their locations.
    
    Colour and marker size: I used two visual encodings—color and size—to represent the 'Annual Volume' on the map. This makes the highest-volume locations instantly recognizable, even at a glance. I chose a continuous color scale (Plasma) to show a clear progression from low to high volume.
   
    Interactivity: Plotly's interactive features are crucial. Hovering over a bar or a point on the map provides specific data points (hover_name, hover_data), allowing the audience to explore details without cluttering the visualization. The ability to zoom and pan on the map makes it easy to explore specific neighborhoods.
    
    > How did you ensure that your data visualizations are reproducible? If the tool you used to make your data visualization is not reproducible, how will this impact your data visualization? 

     I ensured reproducibility by using a scripted approach (Python and Plotly). My entire process of data analysis (pandas) and generating the plots (plotly), is contained within a single, executable script.
     
     The code is version-controlled (e.g., in a Git repository), so every change is tracked.
     
     The output (.html files) can be generated from scratch by simply running the script.
    
    The script uses a public, stable data source (the GeoJSON URL from Open Data Toronto).
    
    If the tool used is not reproducible, it would be a significant drawback. It would be impossible to trace how the final plot was created, making it difficult to update with new data, share with collaborators, or verify the results. Any changes to the underlying data would require manually rebuilding the entire visualization from scratch, which is inefficient and prone to human error.
    
    > How did you ensure that your data visualization is accessible?  

    Accessibility: I selected a color palette that offers sufficient contrast to be readable. The plots are interactive, allowing users to hover over elements to see exact values, which helps those who might not be able to differentiate colors easily. The titles and axis labels are clear and descriptive, providing immediate context.
    
    > Who are the individuals and communities who might be impacted by your visualization?  
    This visualization can impact:
    
    Cyclists: By identifying high-traffic areas and showing seasonal patterns, they can better plan their routes and understand peak usage times.
    
    City Planners & Engineers: The data can inform decisions on where to invest in new bike lanes, maintenance, or counter installations. For example, the high volume in certain areas might justify an investment in a separated bike path.
    
    Public Health Officials: The data can be used to assess the growth of active transportation and its potential benefits for public health.
    
    Local Businesses: Businesses located near high-volume cycling routes might see an opportunity to cater to this growing community.
    
    A potential negative impact could be the misuse of the data. For instance, without proper context, a large drop in winter volume might be misconstrued as a failure of infrastructure rather than a natural seasonal trend, potentially leading to incorrect policy decisions
    
    > How did you choose which features of your chosen dataset to include or exclude from your visualization? 

    I focused on the features most relevant to my intended message:
Included:
    
    Location Name and Location ID: These are crucial for identifying specific counter locations on the map and for merging the data.
    
    Annual Volume and Seasonal Volume: These were the core metrics chosen to answer the central questions about cycling volume and its distribution.
    
    Latitude and Longitude: These were extracted from the GeoJSON to accurately plot each counter location on the map.

Excluded:
    Hourly/Daily Data: While available, this fine-grained data would have been too complex for a high-level summary visualization. I chose to aggregate to 'Annual' and 'Seasonal' to show the macro trends.
    
    Bike Direction and Type: These are valuable features but were not essential for my initial high-level overview. Including them would have added complexity without directly addressing the primary questions about total volume and its location. I opted for a clean, focused visualization to avoid information overload.
    
    > What ‘underwater labour’ contributed to your final data visualization product?
    
    Data Sourcing and Cleaning: Finding a reliable, up-to-date public dataset and understanding its structure. This includes discovering the Toronto Open Data Portal and the specific GeoJSON file.
    
    Data Integration: The non-trivial task of merging two different datasets (the aggregated volume data and the geographic GeoJSON data). This involved identifying a common key (Location ID / location_dir_id) and writing code to perform a reliable merge, including handling potential mismatches.
    
    Code Debugging: The process of troubleshooting why the initial choropleth_map wasn't working and identifying that a scatter_mapbox was the correct tool for point data. This iterative process of trial and error is a major part of the work.
    
    Plotly-Specific Knowledge: Learning the syntax and features of Plotly, such as how to create a scatter_mapbox, how to handle color scales, and how to customize hover data and layout options.
    
    Data Aggregation: The work done to create seasonal_data and aggregated_data from the raw, time-series data. This involves writing Python code to group data by season, sum volumes, and create a new summary DataFrame.


## Appendix
# importing necessary libraries
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

# accessing the dataset for analysis 
df = pd.read_csv('/Users/melanieram/Desktop/visualization/02_activities/assignments/data/cycling_permanent_counts_daily.csv')

# displaying first five entries and last five entries
print(df.head())
print(df.tail())

# Convert 'dt' column to datetime format
df['dt'] = pd.to_datetime(df['dt'])

# Filter rows where 'dt' is in 2024
filtered_df = df[df['dt'].dt.year.isin([2024])]

# Display the filtered dataframe
print(filtered_df)

# Group the filtered data by 'location_name' and calculate the sum of 'daily_volume' for 2024
aggregated_data = filtered_df.groupby(['location_name', filtered_df['dt'].dt.year])['daily_volume'].sum().reset_index()

# Rename columns for clarity
aggregated_data.columns = ['Location Name', 'Year', 'Annual Volume']
print(aggregated_data[['Location Name', 'Year', 'Annual Volume']].head(15))
# Display the aggregated data
print(aggregated_data)

#Generating a table to show seasonl cycling volume
# Define seasons based on the month
def get_season(month):
    if month in [12, 1, 2]:
        return 'Winter'
    elif month in [3, 4, 5]:
        return 'Spring'
    elif month in [6, 7, 8]:
        return 'Summer'
    else:
        return 'Fall'
# Apply the function to create a 'season' column
filtered_df['season'] = filtered_df['dt'].dt.month.apply(get_season)        
# Group by 'season' and calculate the sum of 'daily_volume'
seasonal_data = filtered_df.groupby(['season', filtered_df['dt'].dt.year])['daily_volume'].sum().reset_index()  
# Rename columns for clarity
seasonal_data.columns = ['Season', 'Year', 'Seasonal Volume']       
# Display the seasonal data
print(seasonal_data) 

#Generate a dynamic visualization to show the seasonal cycling volume using Plotly and having icons appear for each season
import plotly.io as pio
pio.renderers.default = 'notebook'  # Set the renderer to display in the notebook   

import plotly.express as px
import plotly.graph_objects as go
import pandas as pd # Import pandas, as seasonal_data is a DataFrame


# --- 1. Order the data by descending seasonal volume FIRST ---
# This ensures the bars are plotted in the desired order
seasonal_data = seasonal_data.sort_values(by='Seasonal Volume', ascending=False)

# --- 2. Create a consistent color mapping ---
# Define the order of seasons as they appear in the sorted data
sorted_seasons = seasonal_data['Season'].unique().tolist()
# Create a dictionary to map each season to a specific color from the Pastel palette
# This ensures unique and consistent colors even if data changes order
season_color_map = {
    season: px.colors.qualitative.Pastel[i % len(px.colors.qualitative.Pastel)]
    for i, season in enumerate(sorted_seasons)
}


# --- Create a bar chart with icons for each season ---
fig = go.Figure()


# Add bars for each season with corresponding colors
for season in sorted_seasons:
    season_data = seasonal_data[seasonal_data['Season'] == season]
    fig.add_trace(go.Bar(
        x=season_data['Seasonal Volume'],
        y=season_data['Season'],
        name=season,
        marker_color=season_color_map[season],
        orientation='h',
        hoverinfo='x+name'
    ))  

# Update layout for better readability
fig.update_layout(
    title='Seasonal Cycling Volume in Toronto (2024)',
    xaxis_title='Seasonal Volume',
    yaxis_title='Season',
    barmode='stack',
    height=600,
    width=1000,
    legend_title_text='Season'
)
# Show the figure
fig.show()

#pasting geojson data for the map
import json
import requests
# URL to the GeoJSON data
geojson_url = 'https://ckan0.cf.opendata.inter.prod-toronto.ca/dataset/ff7e7369-cbba-4545-9e26-e5a5ef6a123c/resource/03ec1db0-7258-4d6b-8e64-29b80e543c55/download/cycling_permanent_counts_locations_geojson.geojson'
# Fetch the GeoJSON data
response = requests.get(geojson_url)
if response.status_code == 200:
    geojson_data = response.json()
else:
    print("Failed to fetch GeoJSON data. Status code:", response.status_code)
    geojson_data = None 
# Display the first few features of the GeoJSON data
if geojson_data and 'features' in geojson_data:
    print("Number of features in GeoJSON data:", len(geojson_data['features']))
    print("First feature:", geojson_data['features'][0])
#aligning the GeoJSON data with the aggregated data
# Create a mapping from location name to geometry
location_to_geometry = {
    feature['properties']['location_name']: feature['geometry']
    for feature in geojson_data['features']
}   
# Add a new column to the aggregated data with the corresponding geometry
aggregated_data['geometry'] = aggregated_data['Location Name'].map(location_to_geometry)    
# Display the first few rows of the updated aggregated data
print(aggregated_data[['Location Name', 'Annual Volume', 'geometry']].head())
# Generate a dynamic visualization to show the top locations by annual volume on a map
import plotly.express as px
# Create the px.scatter_mapbox plot
fig = px.scatter_mapbox(
    aggregated_data,
    lat=aggregated_data['geometry'].apply(lambda x: x['coordinates'][1]),
    lon=aggregated_data['geometry'].apply(lambda x: x['coordinates'][0]),               
    size='Annual Volume',
    color='Annual Volume',
    hover_name='Location Name',
    title='Annual Cycling Volume by Location in Toronto (2024)',
    color_continuous_scale=px.colors.sequential.Plasma,
    zoom=10,
    height=600,
    width=1000
)
# Update the layout to use a Mapbox style
fig.update_layout(  
    mapbox_style='carto-positron',  # Use a Mapbox style
    mapbox_zoom=11,  # Initial zoom level
    mapbox_center={'lat': 43.66, 'lon': -79.4}  # Center the map on Toronto
)
# Show the figure
fig.show()


- This assignment is intentionally open-ended - you are free to create static or dynamic data visualizations, maps, or whatever form of data visualization you think best communicates your information to your audience of choice! 
- Total word count should not exceed **(as a maximum) 1000 words** 
 
### Why am I doing this assignment?:  
- This ongoing assignment ensures active participation in the course, and assesses the learning outcomes: 
* Create and customize data visualizations from start to finish in Python
* Apply general design principles to create accessible and equitable data visualizations
* Use data visualization to tell a story  
- This would be a great project to include in your GitHub Portfolio – put in the effort to make it something worthy of showing prospective employers!

### Rubric:

| Component         | Scoring  | Requirement                                                                 |
|-------------------|----------|-----------------------------------------------------------------------------|
| Data Visualizations | Complete/Incomplete | - Data visualizations are distinct from each other<br>- Data visualizations are clearly identified<br>- Different sources/rationales (text with two images of data, if visualizations are labeled)<br>- High-quality visuals (high resolution and clear data)<br>- Data visualizations follow best practices of accessibility |
| Written Explanations | Complete/Incomplete | - All questions from assignment description are answered for each visualization<br>- Explanations are supported by course content or scholarly sources, where needed |
| Code              | Complete/Incomplete | - All code is included as an appendix with your final submissions<br>- Code is clearly commented and reproducible |

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 13/07/2025`
* The branch name for your repo should be: `assignment-3`
* What to submit for this assignment:
    * A folder/directory containing:
        * This file (assignment_3.md)
        * Two data visualizations 
        * Two markdown files for each both visualizations with their written descriptions.
        * Link to your dataset of choice.
        * Complete and commented code as an appendix (for your visualization made with Python, and for the other, if relevant) 
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/visualization/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-3`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
