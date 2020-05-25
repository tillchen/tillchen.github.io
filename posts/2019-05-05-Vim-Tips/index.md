# Vim Tips


* [Deletion](#deletion)
* [Navigation](#navigation)
* [Modification](#modification)
* [Miscellaneous](#miscellaneous)
* [Plugins](#plugins)
  * [Multiple Cursors](#multiple-cursors)
  * [Emmet](#emmet)
  * [Tagbar](#tagbar)
  * [NERDTree](#nerdtree)

## Deletion

1. 'dw’ deletes the word including the space.

2. ‘d$’ deletes from the cursor to the end of the line.

3. ‘de’ deletes the word excluding the space.

4. ‘d2w’, ‘d2e’ delete 2 words.

5. ‘dd’ deletes a whole line, ‘2dd’ deletes 2 lines.

6. ‘p’ puts the deleted content.

## Navigation

1. ‘0’ goes to the start of the line. '$' goes to the end of the line.

2. ‘2w’ goes 2 words ahead.

3. ‘2e’ goes to the end of the second word ahead.

4. ‘G’ goes to the bottom, \<C-g\> shows the location status, ‘gg’ goes to the top, ‘5G’ goes to the 5th line.

## Modification

1. ‘u’ undoes, ‘U’ fixes the whole line, \<C-r\> undoes the undo.

2. ‘r’ and ‘a’ replace the char with a.

3. ‘ce’ and ‘abc’ change the word to the end with abc.

4. ‘c$’ changes to the end of the line.

5. :r foo appends the content of foo to the file.

6. ‘R’ goes to replace mode.

7. ‘y’ yanks and ‘p’ pastes, ‘yw’ yanks word.

8. ‘gt’ goes to the next tab, ‘gT’ goes to the previous tab.

9. `:1,10s/foo/bar/g` replaces foo with bar between line 1 and line 10.

## Miscellaneous

1. To enable spell check :setlocal spell spelllang=en\_us .

2. :terminal to open terminal, \<C-\\\>\<C-n\> to enter vim mode to enable go up and go down.

3. g, \<C-g\> to see the word count.

## Plugins

### Multiple Cursors

In **normal mode**, \<C-n\>\<C-n\>\<C-n\>c, new_name (select 3 variables and rename.)

### Emmet

`html:5_` ("_" is the cursor position), then \<C-y\> **,** , we get:

```html
<!DOCTYPE HTML>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    _
</body>
</html>
```

### Tagbar

F8 to toggle.

### NERDTree

:NERDTreeFocus to turn on.
