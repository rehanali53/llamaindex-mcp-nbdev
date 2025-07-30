# LlamaIndex MCP Server for SQLite Database Operations

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![MCP](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io/)

A Model Context Protocol (MCP) server implementation for SQLite database operations, built with nbdev and FastMCP. This server enables AI assistants to interact with SQLite databases through standardized MCP tools.

## 🚀 Features

- **MCP Integration**: Fully compatible with the Model Context Protocol for AI assistant integration
- **SQLite Operations**: Provides tools for inserting and querying data in SQLite databases
- **Natural Language Queries**: Ask questions in plain English using LlamaIndex (optional)
- **Dual Server Modes**: Supports both SSE (Server-Sent Events) and STDIO communication
- **Type-Safe**: Comprehensive type hints and documentation
- **nbdev Framework**: Built using literate programming with Jupyter notebooks
- **Error Handling**: Robust error management for database operations
- **CLI Interface**: Easy-to-use command-line interface with customizable options
- **Web Demo**: Interactive browser-based demo for testing

## 📋 Requirements

- Python 3.9 or higher
- fastmcp library
- SQLite3 (included with Python)

## 🛠️ Installation

### Basic Installation

```bash
# Clone the repository
git clone https://github.com/rehanali53/llamaindex-mcp-nbdev.git
cd llamaindex-mcp-nbdev

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in development mode
pip install -e .
```

### With LlamaIndex Support (Optional)

```bash
# Install with natural language query support
pip install -e ".[llama]"

# Or manually install LlamaIndex
pip install llama-index llama-index-llms-openai
```

### Direct Installation

```bash
# Basic installation
pip install git+https://github.com/rehanali53/llamaindex-mcp-nbdev.git

# With LlamaIndex support
pip install "llamaindex-mcp-nbdev[llama] @ git+https://github.com/rehanali53/llamaindex-mcp-nbdev.git"
```

## 🎯 Quick Start

### Command Line Usage

```bash
# Run with default settings (SSE server, demo.db)
llamaindex-mcp-server

# Specify custom database
llamaindex-mcp-server --db_path mydata.db

# Use STDIO server mode
llamaindex-mcp-server --server_type stdio

# View help
llamaindex-mcp-server --help
```

### Python API Usage

```python
from llamaindex_mcp_nbdev.core import SQLiteMCPServer

# Create server instance
server = SQLiteMCPServer('my-sqlite-server', 'database.db')

# Run the server (blocks until stopped)
server.run('sse')  # or 'stdio'
```

### 🌐 Web Demo

Run the interactive web demo to test the server functionality:

```bash
# After installation, run:
llamaindex-web-demo

# Or using the module
python -m llamaindex_mcp_nbdev.web_demo
```

Then open http://localhost:8000 in your browser to:
- Add new records to the database
- Query data with custom SQL queries
- View results in a formatted table
- Use quick query buttons for common operations

### 🤖 Natural Language Queries with LlamaIndex

Run the server with natural language support:

```bash
# Set your OpenAI API key
export OPENAI_API_KEY="your-api-key"

# Run the LlamaIndex-enabled server
llamaindex-nl-server

# Or pass the API key directly
llamaindex-nl-server --openai_api_key "your-api-key"
```

Now AI assistants can ask questions in plain English:
- "What's the average age of our team?"
- "How many developers do we have?"
- "Who is the oldest engineer?"
- "Show me age distribution by profession"

The server will:
1. Convert natural language to SQL
2. Execute the query
3. Return both the answer and generated SQL

### 💬 Interactive Chat Interface

For a simple chat experience without needing an MCP client:

```bash
# Make sure your OpenAI key is set
export OPENAI_API_KEY="your-api-key"

# Run the chat interface
python chat_interface.py
```

This provides:
- Natural language database queries
- Interactive person addition
- View current data
- See generated SQL queries

Or use the console command:
```bash
llamaindex-chat
```

### 📓 Jupyter Notebook Interface

For development and experimentation, use the notebook interface:

```python
from llamaindex_mcp_nbdev.chat_interface import DatabaseChatbot

# Create chatbot
chatbot = DatabaseChatbot("demo.db")

# Add a person
chatbot.add_person("Jane Doe", 28, "Designer")

# Ask questions
response = chatbot.query_database("What's the average age?")
print(response)

# Show data
print(chatbot.show_sample_data())
```

## 📊 Database Schema

The server manages a SQLite database with the following schema:

### Table: `people`

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key (auto-increment) |
| name | TEXT | Person's name (required) |
| age | INTEGER | Person's age (required) |
| profession | TEXT | Person's profession (required) |

## 🔧 MCP Tools

### 1. `add_data`
Executes INSERT queries to add records to the database.

**Parameters:**
- `query` (str): SQL INSERT query

**Example:**
```sql
INSERT INTO people (name, age, profession) 
VALUES ('Alice Smith', 25, 'Developer')
```

### 2. `read_data`
Executes SELECT queries to retrieve data.

**Parameters:**
- `query` (str, optional): SQL SELECT query (defaults to `SELECT * FROM people`)

**Examples:**
```sql
-- Get all records
SELECT * FROM people

-- Filter by age
SELECT name, profession FROM people WHERE age > 30

-- Order results
SELECT * FROM people ORDER BY age DESC
```

### 3. `nl_query` (LlamaIndex Enhanced)
Ask questions about the database in natural language.

**Parameters:**
- `question` (str): Natural language question

**Examples:**
```
"What is the average age of developers?"
"How many people are older than 30?"
"Who is the youngest person in the database?"
"Show me all engineers sorted by age"
```

**Note:** Requires LlamaIndex installation and OpenAI API key.

## 🧪 Testing

Run the test suite:

```bash
# Run tests from notebooks
nbdev_test

# Or run pytest
pytest
```

## 📁 Project Structure

```
llamaindex-mcp-nbdev/
├── nbs/                          # Development notebooks
│   ├── 00_core.ipynb            # Core functionality
│   ├── 01_server.ipynb          # Server implementation
│   ├── 02_testing.ipynb         # Test suite
│   └── index.ipynb              # Documentation
├── llamaindex_mcp_nbdev/        # Generated Python package
│   ├── __init__.py
│   ├── core.py                  # SQLiteMCPServer class
│   └── server.py                # CLI entry point
├── settings.ini                 # nbdev configuration
├── setup.py                     # Package setup
└── README.md                    # This file
```

## 🔨 Development

This project uses [nbdev](https://nbdev.fast.ai/) for development. To contribute:

1. Fork the repository
2. Create a feature branch
3. Make changes in the notebooks (`nbs/` directory)
4. Run `nbdev_prepare` to export code and run tests
5. Submit a pull request

### nbdev Commands

```bash
# Export notebooks to Python modules
nbdev_export

# Run tests
nbdev_test

# Build documentation
nbdev_docs

# Prepare for git (export, test, clean)
nbdev_prepare
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Rehan Ali**
- Email: rehanalid63@gmail.com
- GitHub: [@rehanali53](https://github.com/rehanali53)

## 🙏 Acknowledgments

- Built with [nbdev](https://nbdev.fast.ai/) - A literate programming environment
- Powered by [FastMCP](https://github.com/jlowin/fastmcp) - Fast Model Context Protocol implementation
- Implements the [Model Context Protocol](https://modelcontextprotocol.io/) specification

## 📚 Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [nbdev Documentation](https://nbdev.fast.ai/)
- [SQLite Documentation](https://www.sqlite.org/docs.html)

---

**Note**: This is an initial release (v0.0.1). Features and API may change in future versions.