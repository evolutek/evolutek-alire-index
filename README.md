# Evolutek Alire Index

## Maintenance Policy

### Before You Start
An Alire index is a catalog containing references to other repositories. It simplifies dependency management for your projects. Only repositories intended for reuse across multiple projects should be added to the index.

### Adding a New Entry to the Index
Each index entry resides in a directory named after the crate (`your_crate_name`), containing a TOML file named `your_crate_name-<version>.toml`. **Important:** The version in the filename must match the version specified inside the TOML file.

#### Index Structure
- The root `index` directory contains an `index.toml` file specifying the current index version
- Increment this version when publishing a new stable release of the index
- Entries are organized in subdirectories by their **first two letters** (e.g., `st` for `stm32_hal`)

#### Steps to Add an Entry:
1. Create a folder for the first two letters of your crate name if it doesn't exist (e.g., `st` for `stm32_hal`)
2. Make a new directory with the exact crate name in the letter folder (e.g., `index/st/stm32_hal`)
3. Place your crate's TOML file in this directory and rename it to:  
   `your_crate_name-<version>.toml` (e.g., `stm32_hal-1.2.0.toml`)
4. In the TOML file, include an `[origin]` section with **at minimum**:

```toml
[origin]
url = "git+https://your/repository/url.git"
commit = "a1b2c3d4e5f6..."  # Full commit hash
```
   Optional parameters:
   - `subdir`: If the crate is in a subdirectory
   - Binary release URLs (for prebuilt artifacts)

### Using the Index in Projects
Add this index to your Alire workspace with:

```shell
alr index --add git+https://github.com/evolutek/evolutek-alire-index.git#master --name evi
```

Then include dependencies from this index in your crate manifest (`alire.toml`):
```toml
[dependencies]
your_crate_name = "*"  # Version constraint
```
