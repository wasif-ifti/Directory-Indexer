# 📂 Directory Indexer

A Bash script that walks through a whole folder tree and drops an `index.json` into every directory, listing what's inside: regular files, subfolders, and anything you've marked as "special." You can also hide things you don't want listed.

I think of it like cleaning up before guests come over. First I walk through every room and note what I want hidden (say, the PS5 before the kids show up). Then I go room by room again, put the special stuff in its own drawer and everything else in normal piles, and figure out how far each room is from the front door.

## How it works

- **`_blacklist`**: put this file in any folder, one name per line, and those items get left out of the index
- **`_special`**: same idea, but those items are listed separately as special files or directories
- **`outer`**: each `index.json` records the relative path back to the starting folder (`.`, `..`, `../..`, and so on)

## Usage

```bash
chmod +x indexer
./indexer path/to/folder
```

## Example

```
demo/
├── _blacklist      # contains: secret.txt
├── _special        # contains: photos
├── todo.txt
├── secret.txt
├── notes/
│   ├── a.md
│   └── old/
└── photos/
```

Running `./indexer demo` creates `demo/index.json`:

```json
{
  "files": ["todo.txt"],
  "directories": ["notes"],
  "special_files": [],
  "special_directories": ["photos"],
  "outer": "."
}
```

and `demo/notes/index.json`:

```json
{
  "files": ["a.md"],
  "directories": ["old"],
  "special_files": [],
  "special_directories": [],
  "outer": ".."
}
```

`secret.txt` never shows up, `photos` is flagged as special, and every folder knows its way back to the start.

## Built with

Bash · `find` · `realpath`. Written for CS 265 (Advanced Programming Tools and Techniques) at Drexel University and run on the Tux Linux cluster.
