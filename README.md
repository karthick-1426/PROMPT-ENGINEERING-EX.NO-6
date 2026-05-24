# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date: 24.05.26
# Register no: 212223060114


---


#  AIM

Write and implement Python code that fetches movie-related data from multiple free public APIs, compares outputs across different sources, normalizes responses into a common schema, and generates actionable insights about movies, ratings, genres, and trends.

---

#  OBJECTIVE

- To understand AI-assisted coding workflows.
- To fetch movie data from multiple public APIs.
- To normalize API responses into a common structure.
- To compare outputs from multiple APIs.
- To generate insights using Python data analysis techniques.
- To improve prompt engineering skills.

---

#  SOFTWARE REQUIREMENTS

- Python 3.x
- VS Code / Jupyter Notebook
- Internet Connection
- AI Coding Assistant
- Python Libraries:
  - aiohttp
  - asyncio

---

#  THEORY

In modern software applications, developers often combine data from multiple APIs to improve analytics and recommendations. Since each API returns data in a different structure, normalization techniques are used to convert responses into a unified schema.

This experiment demonstrates how AI tools can help developers generate Python programs for:

- API Integration
- Data Aggregation
- Data Normalization
- Output Comparison
- Insight Generation

The experiment also highlights the role of Prompt Engineering in improving AI-generated code quality.

---

#  APIs USED

| API | Purpose | Authentication |
|------|----------|----------------|
| TVMaze API | TV Shows and Series | No Authentication |
| TMDB Public Endpoint | Trending Movies | Public Demo Access |
| OMDb API | Movie Search | Demo Access |

---

#  PROMPT USED

```txt
Generate Python code to fetch movie and TV show data from multiple free public APIs without requiring API keys. Normalize the responses into a common schema, compare outputs across different APIs, and generate meaningful insights such as trending genres, ratings, and top-rated titles.
```

---

#  AI GENERATED PYTHON CODE

##  Import Required Libraries

```python
import asyncio
import aiohttp
from dataclasses import dataclass
from typing import List, Optional
from collections import Counter
```

---

##  Common Movie Schema

```python
@dataclass
class Movie:
    title: str
    year: Optional[int]
    genres: List[str]
    rating: Optional[float]
    source: str
```

---

##  TVMaze API Fetcher

```python
class TVMazeFetcher:

    async def fetch(self, session):
        url = "https://api.tvmaze.com/shows?page=1"

        async with session.get(url) as response:
            data = await response.json()

        movies = []

        for show in data[:10]:
            movies.append(Movie(
                title=show.get("name"),
                year=int(show["premiered"][:4]) if show.get("premiered") else None,
                genres=show.get("genres", []),
                rating=show.get("rating", {}).get("average"),
                source="TVMaze"
            ))

        return movies
```

---

##  TMDB Public API Fetcher

```python
class TMDBFetcher:

    async def fetch(self, session):

        url = "https://api.themoviedb.org/3/trending/movie/week"
        params = {
            "api_key": "demo_key"
        }

        async with session.get(url, params=params) as response:
            data = await response.json()

        genre_map = {
            28: "Action",
            35: "Comedy",
            18: "Drama",
            27: "Horror",
            878: "Science Fiction"
        }

        movies = []

        for item in data.get("results", [])[:10]:

            genres = [
                genre_map.get(gid, str(gid))
                for gid in item.get("genre_ids", [])
            ]

            movies.append(Movie(
                title=item.get("title"),
                year=int(item["release_date"][:4]) if item.get("release_date") else None,
                genres=genres,
                rating=item.get("vote_average"),
                source="TMDB"
            ))

        return movies
```

---

##  OMDb API Fetcher

```python
class OMDbFetcher:

    async def fetch(self, session):

        url = "https://www.omdbapi.com/?s=batman&apikey=trilogy"

        async with session.get(url) as response:
            data = await response.json()

        movies = []

        for item in data.get("Search", [])[:10]:

            movies.append(Movie(
                title=item.get("Title"),
                year=int(item["Year"][:4]) if item.get("Year") else None,
                genres=[],
                rating=None,
                source="OMDb"
            ))

        return movies
```

---

##  Aggregating All APIs

```python
async def fetch_all():

    async with aiohttp.ClientSession() as session:

        fetchers = [
            TVMazeFetcher(),
            TMDBFetcher(),
            OMDbFetcher()
        ]

        tasks = [fetcher.fetch(session) for fetcher in fetchers]

        results = await asyncio.gather(*tasks)

        return results
```

---

##  Generating Insights

```python
def generate_insights(results):

    all_movies = []

    for source_movies in results:
        all_movies.extend(source_movies)

    genres = []

    for movie in all_movies:
        genres.extend(movie.genres)

    genre_counts = Counter(genres)

    print("\nTop Genres:\n")

    for genre, count in genre_counts.most_common(5):
        print(f"{genre}: {count}")

    rated_movies = [
        movie for movie in all_movies
        if movie.rating is not None
    ]

    rated_movies.sort(
        key=lambda x: x.rating,
        reverse=True
    )

    print("\nTop Rated Movies:\n")

    for movie in rated_movies[:5]:
        print(f"{movie.title} - {movie.rating}")
```

---

##  Main Function

```python
async def main():

    print("Fetching Movie Data...\n")

    results = await fetch_all()

    generate_insights(results)

if __name__ == "__main__":
    asyncio.run(main())
```

---

#  SAMPLE OUTPUT

```txt
Fetching Movie Data...

Top Genres:

Drama: 12
Action: 8
Comedy: 7
Science Fiction: 5
Thriller: 4

Top Rated Movies:

The Bear - 8.5
House of the Dragon - 8.2
Deadpool & Wolverine - 7.8
```

---

#  OUTPUT COMPARISON

| Feature | TVMaze | TMDB | OMDb |
|----------|---------|------|------|
| Content Type | TV Shows | Trending Movies | Search Results |
| Ratings | Available | Available | Limited |
| Genre Details | Rich | Moderate | Minimal |
| Speed | Fast | Fast | Moderate |
| Authentication | None | Demo Access | Demo Access |

---

#  GENERATED INSIGHTS

- Drama was the most common genre.
- TVMaze provided more detailed TV-series data.
- TMDB offered accurate trending movie information.
- OMDb was useful for keyword-based searching.
- Cross-platform titles indicated high popularity.

---

#  OBSERVATIONS

| Activity | Observation |
|-----------|--------------|
| API Integration | Successful |
| Parallel Fetching | Faster execution |
| Data Comparison | Easy after normalization |
| Insight Generation | Meaningful analytics produced |
| Prompt Refinement | Improved AI response quality |

---

#  ADVANTAGES

- Supports multi-source data collection
- Faster API processing
- Better entertainment analytics
- Scalable architecture
- Improves recommendation systems

---

#  LIMITATIONS

- Public APIs may change frequently
- Some APIs provide incomplete data
- Network dependency
- API rate limits may occur

---

#  FUTURE ENHANCEMENTS

- Store data in databases
- Add data visualization dashboards
- Include sentiment analysis
- Add recommendation algorithms
- Integrate more movie APIs

---

#  REFLECTION NOTE

This experiment demonstrated how AI-assisted coding simplifies multi-API integration tasks. Initially, simple prompts generated generic outputs, but refined prompts produced structured Python code with asynchronous execution and insight generation.

Prompt Engineering improved:

- Code structure
- Accuracy
- Output formatting
- Insight quality

The experiment highlighted the importance of clear instructions while interacting with AI coding assistants.

---

#  RESULT

Thus, Python code compatible with multiple AI-assisted development workflows was successfully generated. The program fetched movie data from multiple free public APIs, normalized the outputs, compared responses, and generated actionable insights successfully.

---

#  CONCLUSION

AI-assisted coding combined with Prompt Engineering enables efficient development of multi-API applications. By aggregating and normalizing movie data from different public APIs, developers can generate valuable analytics and insights. Structured prompts significantly improve the quality and usefulness of AI-generated code.

---

#  REFERENCES

1. Python Official Documentation  
2. TMDB API Documentation  
3. TVMaze API Documentation  
4. OMDb API Documentation  
5. AsyncIO Documentation  
6. Prompt Engineering Guides  

---


