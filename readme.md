# EasyTOML

A simple and lightweight TOML serialization/deserialization library in modern C++.

---

## Supported Types

- **String:** `std::string` or string literals (`const char*`)
- **Integer:** `int64_t`
- **Float:** `double`
- **Boolean:** `bool`
- **Array:** `Toml::TomlArray` (which is a `std::vector<TomlValue>`)
- **Table:** `Toml::Table` (internally stores key-value pairs preserving insertion order)

---

## Usage Example

```cpp
#include "toml.hpp"

// Prepare the root table
auto table = Toml::Table::make("");

// Create a nested table "project"
auto project = Toml::Table::make("project");

// Fill project info with various types
project["name"] = "MyProject";              // String
project["version"] = "1.0.0";               // String

// Create an array of authors
Toml::TomlArray authors_array;
authors_array.push_back("Tsukimoto");
authors_array.push_back("Another Author");
project["authors"] = Toml::TomlValue(authors_array); // Array of strings

project["license"] = "MIT";                  // String
project["output"] = "exe";                    // String
project["sourceFolder"] = "src";              // String
project["buildFolder"] = "build";             // String

// Insert nested table "project" into root table
table["project"] = Toml::TomlValue(project.get());

// Create another nested table "tasks"
auto tasks = Toml::Table::make("tasks");
tasks["dev"] = "neoluma run --debug";         // String command

// Insert "tasks" table into root
table["tasks"] = Toml::TomlValue(tasks.get());

// Serialize to a file
std::ofstream config("project_config.toml");
if (config.is_open()) {
    Toml::serializeTable(config, table);
}
config.close();
```
## Key Points
- Use `Toml::Table::make()` to create new tables.
- Use `operator[]` to insert or access keys in tables.
- Use `Toml::TomlValue` to wrap arrays (`TomlArray`) or nested tables (`TomlTable`).
- Arrays are `std::vector<TomlValue>`, so you can mix types if needed.
- Serialization preserves the structure and order of tables.
- You can parse single values with  `Toml::parseValue(std::string)` (full file parsing not included).

### Serialization Output Example
```toml
[project]
name = "MyProject"
version = "1.0.0"
authors = ["Tsukimoto", "Another Author"]
license = "MIT"
output = "exe"
sourceFolder = "src"
buildFolder = "build"

[tasks]
dev = "neoluma run --debug"
```
## License
[MIT License](./LICENSE)

---
### Made by Tsukimoto 💞
