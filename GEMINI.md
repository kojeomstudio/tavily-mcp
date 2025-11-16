# Gemini Workspace Analysis

## Project Overview

This project is a TypeScript-based MCP (Model Context Protocol) server for the Tavily API. It exposes several tools to interact with Tavily's services, which include advanced web search, content extraction, website crawling, and site mapping. The server is designed to be integrated with various AI development environments and clients that support MCP, such as Claude Desktop and Cursor.

The server can be run locally using Node.js or as a Docker container. It requires a Tavily API key for authentication.

## Key Technologies

*   **Programming Language**: TypeScript
*   **Platform**: Node.js
*   **Key Libraries**:
    *   `@modelcontextprotocol/sdk`: For creating the MCP server.
    *   `axios`: For making HTTP requests to the Tavily API.
    *   `dotenv`: For managing environment variables.
    *   `yargs`: For parsing command-line arguments.
*   **Containerization**: Docker, Docker Compose

## Building and Running

### Prerequisites

*   Node.js (v20 or higher)
*   npm (Node Package Manager)
*   A Tavily API key, which must be set as the `TAVILY_API_KEY` environment variable.

### Running with NPX (Recommended)

The simplest way to run the server is using `npx`, which will download and run the latest version from npm.

```bash
# The TAVILY_API_KEY must be set in your environment
npx -y tavily-mcp@latest
```

### Local Development

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/tavily-ai/tavily-mcp.git
    cd tavily-mcp
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Create a `.env` file** in the root of the project and add your API key:
    ```
    TAVILY_API_KEY=your-api-key-here
    ```

4.  **Build the project:**
    This command compiles the TypeScript code into JavaScript in the `build` directory.
    ```bash
    npm run build
    ```

5.  **Run the server:**
    ```bash
    node build/index.js
    ```

### Running with Docker

The project includes a `Dockerfile` and `compose.yaml` for containerization.

1.  **Ensure you have a `.env` file** with your `TAVILY_API_KEY` in the project root.

2.  **Build and run the container using Docker Compose:**
    ```bash
    docker-compose up --build
    ```
    The service will be available on port 8080.

## Available Tools

The server exposes the following tools:

*   `tavily-search`: A powerful web search tool.
*   `tavily-extract`: Extracts content from specified URLs.
*   `tavily-crawl`: Performs a structured web crawl starting from a base URL.
*   `tavily-map`: Creates a structured map of a website's URLs.

You can list the available tools and their descriptions by running:
```bash
node build/index.js --list-tools
```

## Development Conventions

*   The main application logic is located in `src/index.ts`.
*   The project uses the `@modelcontextprotocol/sdk` to define and handle tool requests.
*   TypeScript is used for static typing, and code is compiled to the `build` directory.
*   Dependencies are managed with `npm`.
*   The `prepare` script in `package.json` ensures the project is built before being published to npm.
