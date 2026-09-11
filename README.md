# dotenv

[![CI](https://github.com/alya-lang/dotenv/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/dotenv/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/dotenv?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fdotenv%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fdotenv%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Environment variable (.env) parser, interpolation, and configuration loader for the [Alya Programming Language](https://github.com/alya-lang/alya).

---

## 🌟 Features

- ⚡ **Lightweight & Fast**: Fast lexical parsing and configuration loading
- 📄 **File & In-Memory Support**: Load `.env` from disk or parse arbitrary strings
- 🛡️ **Quoted & Escaped Strings**: Supports single (`'`) and double (`"`) quotes, escaped newlines and tabs
- 💬 **Inline Comments**: Automatically strips trailing `#` comments outside quotes
- 🔄 **Variable Interpolation**: Expands `${VAR}` references using parsed values and OS environment
- 🎯 **Type-Safe Getters**: Safe conversions to `string`, `int`, `bool`, and `float` with custom fallbacks
- 🧰 **Dump Support**: Export parsed environment maps back to `.env` formatted text

---

## 📁 Project Architecture

```text
dotenv/
├── alya.toml               # Package manifest
├── src/
│   ├── lib.alya            # Public API facade
│   ├── parser.alya         # Lexical line parser, quote/escape, interpolation
│   ├── serializer.alya     # .env text serializer (dump)
│   └── getters.alya        # Type-safe value extractors
├── examples/
│   └── demo.alya           # Runnable usage example
├── tests/
│   └── test_basic.alya     # Automated test suite
└── benches/
    └── bench_basic.alya    # Micro-benchmarks
```

---

## 📦 Installation

Add `dotenv` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
dotenv = { git = "https://github.com/alya-lang/dotenv", branch = "main" }
```

Or install it directly using the Alya package CLI:

```bash
alyac add dotenv --git https://github.com/alya-lang/dotenv --branch main
alyac install
```

---

## 🚀 Quick Start

Create a `.env` file in your project root:

```env
# Application Settings
APP_NAME=AlyaService
PORT=8080
DEBUG=true
MAX_RETRIES=5

# Database URL with interpolation
DB_HOST=127.0.0.1
DB_PORT=5432
DATABASE_URL="postgres://${DB_HOST}:${DB_PORT}/prod_db"
```

Load and read configuration in your Alya application:

```alya
import "dotenv" as dotenv

function main()
    # 1. Load .env file
    let config = dotenv::load()

    # 2. Access typed values with fallback defaults
    let app_name = dotenv::get(config, "APP_NAME", "DefaultApp")
    let port = dotenv::get_int(config, "PORT", 3000)
    let is_debug = dotenv::get_bool(config, "DEBUG", 0)
    let db_url = dotenv::get(config, "DATABASE_URL")

    say "Starting " + app_name + " on port " + str(port)
    say "Database: " + db_url
    if is_debug == 1
        say "[DEBUG MODE ACTIVE]"
    end
end

main()
```

---

## 📖 API Reference

### Loading & Parsing

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `load()` | None | `Map` | Loads `.env` from cwd (or `.env.local`), parses and returns a configuration map. |
| `load_file(path)` | `path: string` | `Map` | Reads the file at `path` and returns a parsed configuration map. |
| `parse(content)` | `content: string` | `Map` | Parses raw multi-line `.env` string content into a key-value map. |
| `dump(env_map)` | `env_map: Map` | `string` | Formats an environment map back into valid `.env` string syntax. |

### Accessors & Typed Getters

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `get(map, key, default)` | `map, key: string, default = ""` | `string` | Returns string value, or `default` if missing or empty. |
| `get_int(map, key, default)` | `map, key: string, default = 0` | `int` | Parses integer value, or returns `default`. |
| `get_bool(map, key, default)` | `map, key: string, default = 0` | `int (0/1)` | Evaluates `"true"`, `"1"`, `"yes"`, `"on"` to `1`, and `"false"`, `"0"`, `"no"`, `"off"` to `0`. |
| `get_float(map, key, default)` | `map, key: string, default = 0.0` | `float` | Parses floating-point value, or returns `default`. |
| `has(map, key)` | `map, key: string` | `int (0/1)` | Returns `1` if key is present in map, `0` otherwise. |

---

## 🧪 Running Tests & Benchmarks

Run the automated test suite:

```bash
alyac run tests/test_basic.alya
```

Run the performance micro-benchmarks:

```bash
alyac run benches/bench_basic.alya
```

Run the runnable usage demo:

```bash
alyac run examples/demo.alya
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alyac install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alyac test
   alyac fmt . --check
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
