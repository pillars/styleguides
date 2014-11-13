# Sublime Text 3

## Plugins

- [EditorConfig](https://github.com/sindresorhus/editorconfig-sublime): Read the `.editorconfig` file and apply project configuration to your editor.
- [Alignment](http://wbond.net/sublime_packages/alignment): Let you align variable definition list and objects.
- [SublimeLinter](http://www.sublimelinter.com/en/latest/)
- [SublimeLinter-jshint](https://sublime.wbond.net/packages/SublimeLinter-jshint)

## SublimeLinter

To lint your code, install `jshint`:

```
npm install -g jshint
```

## Configuration

Set your editor to display a line at 80 characters:

```json
// add to your config file
"rulers": [80]
```