# **Music Text-to-SQL RAG**
A Retrieval-Augmented Generation (RAG) system for querying music data from the Chinook SQLite database. The system uses LangChain, OpenAI's GPT-4o, and fuzzy matching to handle natural language queries about artists, albums, and genres, with robust error handling and fallback mechanisms.

## Features
- **Query Types**:
  - Artist lookup: e.g., "Which artist released the album 'Supernatural'?" → Returns "Santana."
  - Album count: e.g., "How many albums does AC/DC have?" → Returns the number of albums.
  - Album list: e.g., "List the albums by U2." → Returns a list of albums like "Achtung Baby," "Zooropa."
  - Genre tracks: e.g., "Which tracks belong to the Metal genre?" → Returns a list of tracks.
- **Misspelling Handling**: Uses fuzzy matching to correct misspellings (e.g., "Greeen Day" → "Green Day").
- **Fallback Mechanisms**: Partial album title matching (e.g., "Diary" → "Diary of a Madman" by Ozzy Osbourne).
- **Error Handling**: Gracefully handles invalid inputs (e.g., "Unknown Band XYZ" → "No albums found").
- **SQL Validation**: Ensures plausible results (e.g., limits album counts to ≤10).
- **Caching**: Persistent caching for database lists and retriever results to improve efficiency.

## Installation

1. **Clone the Repository**:
```bash
git clone https://github.com/Dinhngoclam2906/MusicRAGSystem.git
cd MusicRAGSystem
```
2. **Install Dependencies**:
- Requires Python 3.8+.
- Install required packages:pip install langchain langchain-openai langchain-community langchain-huggingface fuzzywuzzy python-Levenshtein faiss-cpu tenacity

3. **Set Up Environment**:

- Create a .env file with your OpenAI API key:
```bash
OPENAI_API_KEY=your-api-key
LANGCHAIN_API_KEY=your-langchain-api-key
```
- Generate the Chinook database from the provided SQL file:
```bash
sqlite3 Chinook.db < Chinook_Sqlite.sql
```
4. **Run the Script**:
```bash
python rag_source_code.py
```

## Usage:
The script runs a set of test prompts and generated questions, querying the Chinook database for music-related information. Example queries:

"Which artist released the album 'American Idiot'?" → Returns "Green Day."
"List the albums by U2." → Returns a list of U2 albums.
"How many albums does Greeen Day have?" → Returns "2."
Results are logged to the console with explanations of the SQL queries used.

## Optimizations:
The system was iteratively optimized to address:
- **Name Resolution**: Corrected misspellings using fuzzy matching (threshold 70).
- **SQL Accuracy**: Fixed truncated album titles and implausible counts (e.g., 214 → 1 for some artists).
- **Retriever Efficiency**: Reduced API calls with persistent caching (retriever_cache.pkl).
- **Fallbacks**: Limited partial title matches to 1 result for precision.
- **Diagnostics**: Added checks for missing artists (e.g., The Beatles) and duplicates.

## Known Limitations:
- **Resolved Names**: Some responses display incorrect names (e.g., "How Many Albums Does Acdc Have" instead of "AC/DC"). SQL queries use correct names, so results remain accurate.
- **Album Lists**: Queries like "Which albums are by Queen?" may return incorrect names (e.g., "Unknown") despite correct album lists.
- **Retriever Efficiency**: No cache hits logged, and retriever limit warnings occur due to API constraints.
- **Data Integrity**: Missing artists (e.g., The Beatles) and genre issues (e.g., "Heavy Metal" returning no tracks) stem from the Chinook database.

## Future Improvements:
- **Database Fixes**: Add missing artists (e.g., The Beatles) and correct genre data in Chinook.db.
- **Local Embeddings**: Use a local model (e.g., sentence-transformers/all-MiniLM-L6-v2) to reduce API dependency.
- **Enhanced NLP**: Implement advanced query parsing for complex phrasing.
- **Testing**: Expand test cases to cover edge cases.

## License:
This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments:
- Built with LangChain, OpenAI, and fuzzywuzzy.
- Uses the Chinook database for music data.
