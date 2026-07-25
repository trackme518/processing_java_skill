# Processing CLI Reference

Binary: `/Applications/Processing.app/Contents/MacOS/Processing`

## Top-Level

```
Usage: processing [<options>] [<sketches>]... <command> [<args>]...

  Start the Processing IDE

Options:
  -v, --version  Print version information
  -h, --help     Show this message and exit

Arguments:
  <sketches>  Sketches to open

Commands:
  lsp            Start the Processing Language Server
  cli
  contributions  Manage Processing contributions
  sketchbook     Manage the sketchbook
  sketch         Manage a Processing sketch
```

## Launch IDE

```bash
/Applications/Processing.app/Contents/MacOS/Processing
/Applications/Processing.app/Contents/MacOS/Processing /path/to/sketch_folder
```

---

## `cli` — Build, Run, Export

```
--help               Show help text
--sketch=<name>      Sketch folder (required)
--output=<name>      Output folder (optional, cannot be same as sketch folder)
--force              Erase output folder before build. Use with extreme caution!
--build              Preprocess and compile into .class files
--run                Preprocess, compile, and run
--present            Preprocess, compile, and run in presentation mode
--export             Export an application
--variant            Specify platform/architecture (Export only)
--no-java            Do not embed Java

--build, --run, --present, or --export must be the final parameter.
```

### Run

```bash
Processing cli --sketch=/path/to/sketch_folder --run
```

### Build (compile)

```bash
Processing cli --sketch=/path/to/sketch_folder --build
```

### Presentation mode

```bash
Processing cli --sketch=/path/to/sketch_folder --present
```

### Export

```bash
Processing cli --sketch=/path/to/sketch_folder --export
```

### Export with custom output directory

```bash
Processing cli --sketch=/path/to/sketch_folder --output=/path/to/output --export
```

### Force overwrite

```bash
Processing cli --sketch=/path/to/sketch_folder --output=/path/to/output --force --export
```

WARNING: `--force` erases the output folder.

### Platform variants

| `--variant` | Platform |
|---|---|
| `macos-x86_64` | macOS Intel 64-bit |
| `macos-aarch64` | macOS Apple Silicon |
| `windows-amd64` | Windows Intel/AMD 64-bit |
| `linux-amd64` | Linux Intel 64-bit |
| `linux-arm` | Raspberry Pi ARM 32-bit |
| `linux-aarch64` | Raspberry Pi ARM 64-bit |

```bash
# Apple Silicon native
Processing cli --sketch=/path/to/sketch_folder --variant=macos-aarch64 --export

# Intel Mac
Processing cli --sketch=/path/to/sketch_folder --variant=macos-x86_64 --export
```

### Export without embedded Java

```bash
Processing cli --sketch=/path/to/sketch_folder --no-java --export
```

### Pass arguments to sketch

```bash
Processing cli --sketch=/path/to/sketch_folder --run hello world 42
```

```java
// Inside sketch:
println(args[0]); // "hello"
println(args[1]); // "world"
println(args[2]); // "42"
```

---

## `lsp` — Language Server

```bash
Processing lsp
```

---

## `sketch` — Sketch Management

```
Commands:
  format  Format a Processing sketch
```

### `sketch format`

```
Usage: processing sketch format [<options>] <file>

Options:
  -i, --inplace  Format file in place, otherwise prints to stdout
  -h, --help     Show this message and exit

Arguments:
  <file>  Path to the sketch file to format
```

```bash
# Print to stdout (dry-run)
Processing sketch format MySketch.pde

# Format in-place (modifies file)
Processing sketch format -i MySketch.pde
```

---

## `sketchbook` — Sketchbook Management

```
Commands:
  list  List all sketches
```

```bash
Processing sketchbook list
```

---

## `contributions` — Contributions Management

```
Commands:
  examples  Manage Processing examples
```

### `contributions examples`

```
Commands:
  list  List all examples
```

```bash
Processing contributions examples list
```

---

## Common Errors

### `processing-java: command not found`

Processing 4 removed `processing-java`. Replace:

```bash
processing-java --sketch=SketchName --run
```

with:

```bash
Processing cli --sketch=SketchName --run
```

### `Output folder already exists`

Use `--force` to replace.

### Wrong argument order

```bash
# WRONG
Processing cli --run --sketch=MySketch

# CORRECT
Processing cli --sketch=MySketch --run
```

---

## Shell alias

```bash
alias processing="/Applications/Processing.app/Contents/MacOS/Processing"
```
