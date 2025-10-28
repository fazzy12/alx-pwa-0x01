# CineSeek - Movie Discovery Application

CineSeek is a modern movie discovery application built with Next.js, TypeScript, and Tailwind CSS. The application allows users to browse movies from the MoviesDatabase API, view movie details, and search for films by year or genre.

## API Overview

The MoviesDatabase API is a comprehensive REST API that provides access to an extensive database of movie information. It offers robust endpoints for retrieving movie titles, detailed information about specific films, and supports advanced filtering capabilities. Key features include:

- Access to thousands of movie titles with detailed metadata
- Filtering options by year, genre, and other criteria
- Pagination support for efficient data retrieval
- Rich movie details including cast, crew, ratings, and plot summaries
- High-performance endpoints optimized for modern web applications
- RESTful architecture with predictable URL patterns and HTTP methods

## Version

API Version: **v1**

The current stable version of the MoviesDatabase API is v1, which provides all core functionality for movie data retrieval and search operations.

## Available Endpoints

### `/titles`
The primary endpoint for fetching movie data. Returns a paginated list of movies with support for various filtering options including year and genre.

### `/titles/{id}`
Retrieves detailed information about a specific movie by its unique identifier. Returns comprehensive data including cast, crew, production details, and ratings.

### `/titles/search`
Enables searching for movies by title, keywords, or other searchable fields. Supports partial matching and returns relevant results.

### `/genres`
Returns a list of all available movie genres that can be used for filtering titles.

### `/titles/random`
Fetches a random selection of movies, useful for discovery features and recommendations.

## Request and Response Format

### Typical Request Structure

```
GET https://api.moviesdatabase.com/v1/titles?year=2023&genre=action&page=1
Headers:
  X-RapidAPI-Key: YOUR_API_KEY
  X-RapidAPI-Host: moviesdatabase.p.rapidapi.com
```

**Query Parameters:**
- `year` (optional): Filter movies by release year
- `genre` (optional): Filter by genre (e.g., action, comedy, drama)
- `page` (optional): Page number for pagination (default: 1)
- `limit` (optional): Number of results per page (default: 10, max: 50)

### Typical Response Object

```json
{
  "page": 1,
  "next": "/titles?page=2&year=2023",
  "entries": 50,
  "results": [
    {
      "id": "tt1234567",
      "primaryImage": {
        "url": "https://example.com/image.jpg",
        "width": 1000,
        "height": 1500
      },
      "titleType": {
        "text": "Movie",
        "id": "movie"
      },
      "titleText": {
        "text": "Example Movie Title"
      },
      "releaseYear": {
        "year": 2023
      },
      "releaseDate": {
        "day": 15,
        "month": 6,
        "year": 2023
      }
    }
  ]
}
```

**Response Fields:**
- `page`: Current page number
- `next`: URL for the next page of results
- `entries`: Total number of entries in the current response
- `results`: Array of movie objects containing title information

## Authentication

The MoviesDatabase API requires authentication via API key using RapidAPI headers.

**Required Headers:**
```
X-RapidAPI-Key: YOUR_API_KEY_HERE
X-RapidAPI-Host: moviesdatabase.p.rapidapi.com
```

**Setup Instructions:**

1. Sign up for a RapidAPI account at https://rapidapi.com
2. Subscribe to the MoviesDatabase API
3. Copy your unique API key from the RapidAPI dashboard
4. Store the API key securely in your `.env.local` file:
   ```
   NEXT_PUBLIC_API_KEY=your_api_key_here
   ```
5. Never commit your API key to version control - ensure `.env.local` is in your `.gitignore`

**Security Best Practices:**
- Use Next.js API routes to make server-side requests and keep your API key hidden from the client
- Rotate your API key regularly
- Monitor your API usage through the RapidAPI dashboard

## Error Handling

The API returns standard HTTP status codes along with descriptive error messages.

### Common Error Responses

**401 Unauthorized**
```json
{
  "message": "Invalid API key. Go to https://docs.rapidapi.com/docs/keys for more info."
}
```
*Solution:* Verify your API key is correct and properly formatted in request headers.

**403 Forbidden**
```json
{
  "message": "You are not subscribed to this API."
}
```
*Solution:* Subscribe to the MoviesDatabase API on RapidAPI.

**429 Too Many Requests**
```json
{
  "message": "Rate limit exceeded. Try again later."
}
```
*Solution:* Implement rate limiting in your application and respect API usage limits.

**404 Not Found**
```json
{
  "message": "The requested resource was not found."
}
```
*Solution:* Verify the endpoint URL and resource ID are correct.

**500 Internal Server Error**
```json
{
  "message": "An unexpected error occurred on the server."
}
```
*Solution:* Retry the request after a short delay. If the issue persists, contact API support.

### Implementing Error Handling in Code

```typescript
try {
  const response = await fetch('/api/fetch-movies');
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  const data = await response.json();
  // Process data
} catch (error) {
  console.error('Failed to fetch movies:', error);
  // Display user-friendly error message
}
```

## Usage Limits and Best Practices

### Rate Limits

The API enforces the following rate limits based on your subscription tier:
- **Free Tier:** 100 requests per day
- **Basic Tier:** 1,000 requests per day
- **Pro Tier:** 10,000 requests per day

### Best Practices

**1. Implement Pagination**
Always use pagination when fetching large datasets. Request only the data you need for the current view.

```typescript
const fetchMovies = async (page: number = 1, limit: number = 20) => {
  // Fetch paginated results
};
```

**2. Cache Responses**
Cache API responses on the client-side to reduce redundant requests and improve performance.

```typescript
// Use React Query, SWR, or implement custom caching
const cachedData = localStorage.getItem('movies-cache');
```

**3. Use Server-Side API Routes**
Make API calls from Next.js API routes instead of directly from the client to protect your API key.

```typescript
// pages/api/fetch-movies.ts
export default async function handler(req, res) {
  // Server-side request with protected API key
}
```

**4. Handle Errors Gracefully**
Implement comprehensive error handling with user-friendly messages and loading states.

**5. Optimize Image Loading**
Use Next.js Image component with proper sizing and lazy loading for movie posters.

**6. Debounce Search Requests**
When implementing search functionality, debounce user input to avoid excessive API calls.

**7. Monitor API Usage**
Regularly check your API usage through the RapidAPI dashboard to avoid hitting rate limits unexpectedly.

**8. Implement Request Retry Logic**
Add exponential backoff retry logic for failed requests due to network issues or temporary server errors.

**9. Use TypeScript Interfaces**
Define clear TypeScript interfaces for all API responses to ensure type safety and better development experience.

**10. Respect the API**
Follow the API's terms of service and use reasonable request patterns to maintain good standing and API availability.

---

## Getting Started

1. Clone the repository
2. Install dependencies: `npm install`
3. Create `.env.local` and add your API key
4. Run the development server: `npm run dev`
5. Open http://localhost:3000 in your browser

## Tech Stack

- **Framework:** Next.js 14 (Pages Router)
- **Language:** TypeScript
- **Styling:** Tailwind CSS
- **Icons:** Font Awesome
- **API:** MoviesDatabase API (via RapidAPI)

## Project Structure

```
alx-movie-app/
├── components/       # Reusable React components
├── interfaces/       # TypeScript interfaces
├── pages/           # Next.js pages and API routes
├── public/          # Static assets
└── styles/          # Global styles
```

## License

This project is part of the ALX Software Engineering Program.

