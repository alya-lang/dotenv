# dotenv

[![CI](https://github.com/alya-lang/dotenv/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/dotenv/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Alya](https://img.shields.io/badge/Alya-%3E%3D0.0.5-orange.svg)](https://github.com/Taiizor/Alya)
[![Package Version](https://img.shields.io/badge/version-0.1.0-brightgreen.svg)](alya.toml)

Zero-dependency `.env` environment variable parser and configuration loader for the [Alya Programming Language](https://github.com/Taiizor/Alya).

---

## 🌟 Features

- ⚡ **Lightweight & Pure Alya**: Zero external dependencies, fast lexical parsing
- 📄 **File & In-Memory Support**: Load `.env` from disk or parse arbitrary strings
- 🛡️ **Quoted & Escaped Strings**: Supports single (`'`) and double (`"`) quotes, escaped newlines and tabs
- 💬 **Inline Comments**: Automatically strips trailing `#` comments outside quotes
- 🔄 **Variable Interpolation**: Expands `${VAR}` references using parsed values and OS environment
- 🎯 **Type-Safe Getters**: Safe conversions to `string`, `int`, `bool`, and `float` with custom fallbacks
- 🧰 **Dump Support**: Export parsed environment maps back to `.env` formatted text

---

## 📦 Installation

Add `dotenv` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
dotenv = { git = "https://github.com/alya-lang/dotenv", tag = "v0.1.0" }
```

Or install it directly using the Alya package CLI:

```bash
alyac add dotenv --git https://github.com/alya-lang/dotenv --tag v0.1.0
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

## 🧪 Running Tests

Run the full automated test suite using `alyac`:

```bash
cd E:/MyProject/Alya/Lib/dotenv
alyac run tests/test_basic.alya
```

Test results:
```text
═══════════════════════════════════════════════════
          Alya Dotenv Package Test Suite            
═══════════════════════════════════════════════════

  ✓ parse basic string
  ✓ parse integer
  ✓ parse boolean true
  ✓ parse float
  ✓ fallback default on empty
  ✓ export keyword parsing
  ✓ has key check
  ✓ has missing key check
  ✓ double quotes stripped
  ✓ single quotes preserved without comment strip
  ✓ inline comment stripped
  ✓ hash preserved inside double quotes
  ✓ variable interpolation
  ✓ load_file reads disk file
  ✓ load_file parses int

───────────────────────────────────────────────────
All 15 tests passed successfully! ✓
═══════════════════════════════════════════════════
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m "feat: add support for multiline values"`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
