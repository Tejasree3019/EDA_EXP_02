**Netflix Shows & Movies**

**Aim**

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

**Procedure / Algorithm**

  1)Load dataset (netflix_titles.csv).
  2)Count movies vs TV shows.
  3)Group by country → top contributors.
  4)Create pivot table (release year vs type).
  5)Visualize with bar & line charts.

**Program**
```
import pandas as pd
# 1. Load dataset directly from GitHub
url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df = pd.read_csv(url)
# Basic inspection
print("Shape:", df.shape)
print("Columns:", df.columns, "\n")
print("Type counts:\n", df['type'].value_counts(), "\n")
# 2. Clean 'date_added' and extract year/month
df['date_added'] = df['date_added'].str.strip()   # remove extra spaces
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')  # invalid -> NaT
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month_name()
# 3. Movies vs TV Shows
count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")
# 4. Country vs Type Pivot Table
pivot_country_type = df.pivot_table(index='country', columns='type',values='title', aggfunc='count', fill_value=0 )
Add "Total" column and find max country
pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax()  # country with most titles
max_count = pivot_country_type['Total'].max()       
# number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
# 5. Top 5 Directors
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
# 6. Yearly Trend of Additions (Movies vs TV Shows)
trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
print("Yearly Trend by Type:\n", trend.head(), "\n")
# 7. Expand Genres
df_genre = (
df[['show_id','listed_in']].dropna().assign(listed_in=df['listed_in'].str.split(', ')).explode('listed_in'))
# Merge expanded genres back
df_expanded = df.merge(df_genre, on='show_id', how='left')
# Check merged columns
print("Columns after merge:\n", df_expanded.columns)
# Show sample of expanded genres
print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())
#Top 5 Genres (extra useful for insights)
top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")
```
**Ouptut**

<img width="913" height="314" alt="image" src="https://github.com/user-attachments/assets/ac30a130-8c77-461d-91c7-106c292088cc" />

<img width="908" height="181" alt="image" src="https://github.com/user-attachments/assets/a433a85d-1eb1-4cb2-9cd0-6a5d16600665" />

<img width="913" height="111" alt="image" src="https://github.com/user-attachments/assets/14f21dd3-4aa3-432b-b2b5-a940bb79e724" />

<img width="920" height="256" alt="image" src="https://github.com/user-attachments/assets/dbda3845-3e4f-42c9-a830-b504d0ed7a7a" />

<img width="902" height="182" alt="image" src="https://github.com/user-attachments/assets/600babf4-86e0-4eaf-9e49-bdab923e4bbc" />

<img width="915" height="150" alt="image" src="https://github.com/user-attachments/assets/ffb2f0b5-d214-4dc8-bdac-6018dd39f28b" />

<img width="942" height="259" alt="image" src="https://github.com/user-attachments/assets/6c85bd76-7dcc-425f-9fa0-8b158b780729" />

<img width="955" height="167" alt="image" src="https://github.com/user-attachments/assets/a00e4551-6e94-4b18-8b1c-7e3c742fc29e" />

<img width="936" height="360" alt="image" src="https://github.com/user-attachments/assets/4ee96f75-4d03-41e3-8d33-1af10c8e1ecd" />

**Result**
Helps Netflix in content planning & investments.
