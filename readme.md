**1. CORS Explained**

CORS (Cross-Origin Resource Sharing) errors occur when a web application running in a browser attempts to make requests to a server on a different origin than where the frontend was served. An "origin" consists of the protocol, domain, and port number. In a typical MERN stack development environment, the frontend (React) runs on `http://localhost:3000` while the backend (express) runs on `http://localhost:5000`. Even though both are on localhost, the different port numbers make them separate **origins**, triggering the browser's same-origin policy security mechanism. The browser blocks these cross-origin requests by default to protect users from accessing sensitive data without permission. When the backend doesn't explicitly allow requests from the frontend's origin, the browser refuses to share the response data with the client application, resulting in a CORS error.

- Strategy 1:
  - Install and configure the `cors` package in your Express server. You can then explicitly whitelist which origins can access your API.
- Strategy 2:
  - Configure a proxy in the `package.json` file of your React application by adding `"proxy": "http://localhost:5000"`.

**2. Environment Managament**

Hardcoding API URLs is bad practice because of the lack of flexibility, the code changes required, and the security risk. When you need to switch between development (e.g. `http://localhost:5000`) and production (e.g.`https://api.myapp.com`) environments, you'd have to manually change the URLs in your codebase.

Client Side (React): Use .env files with vairables with variables prefixed `REACT_APP_` and Access them via process.(`env.REACT_APP_API_URL`).

Server Side (Node/Express): The dotenv package loads variables from .env files which are accessesed via `process.env.PORT.` This keeps sensitive data like API keys and database credentials out of your code.

Both approaches use .gitignore to exclude .env files from version control, preventing secrets from being exposed in repositories while letting each environment maintain its own configuration.

**3. Data Fetching Trade-offs**

| Feature               | Axios                                                 | Fetch                                                                         |
| :-------------------- | :---------------------------------------------------- | :---------------------------------------------------------------------------- |
| **1. Json Handling**  | Automatically parses JSON responses `(response.data)` | Requires manual `.json()` call (returns another promise)                      |
| **2. Error Handling** | Throws an error for any `4xx` or `5xx` status code    | Only rejects on network failures; HTTP errors need manual `response.ok check` |

While both `fetch` and `axios` can handle HTTP requests, axios provides automatic JSON transformation as a significant advantage for complex applications. The native fetch API requires developers to manually call `.json()` on the response object and handle the promise chain, adding boilerplate code to EVERY request. Axios automatically parses JSON responses and makes the data directly available in the response object's data property.
For my final project (a full-stack MERN application that provides real-time competitive Pokémon team analytics)utilizing Axios is the more efficient choice because its automatic JSON transformation streamlines the process of merging complex, nested data from multiple sources like PokeAPI and Smogon, allowing me to focus on the actual logic of the team-building engine rather than repetitive data-parsing.
