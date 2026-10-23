# Diving deep into Editing modes in Linux

## Command Mode
Command mode is the default mode in which you can navigate through the text, delete text, copy text, paste text, and perform other operations. 

- `yy` --> Yank (copy) the current line
- `p` --> Paste
- `u` --> Undo the last change
- `'n'yy` --> Yank (copy) 'n' number of lines
- `yw` --> Yank (copy) the single word
- `'n'yw` --> Yank (copy) 'n' number of words
- `dd` --> Delete the current line
- `'n'dd` --> Delete 'n' number of lines
- `dw` --> Delete the single word
- `'n'dw --> Delete 'n' number of words
- `G` --> Go to last line
- `gg` --> To go to first line
- `'n'gg` --> To go 'n'th line
- `Ctrl + r` --> Redo

## Insert Mode

- `i` --> Insert before cursor
- `a` --> Insert after cursor
- `I / A` --> Insert at start / Insert end of line
- `o / O` --> New line below /New line above
- `s / S` --> Delete a character / Delete entire line
- `r / R` --> replace the character a cursor / replacing entire line as you type

## Execute Mode

- `:w` --> save
- `:q` --> quit
- `:wq` --> save and quit
- `:q!` --> quit without saving
- `:wq!` --> save and quit forcefully(in some files the alteration is not allowed so to forcefully do it, this shortcut is used)
- `/word` --> search word and also highlights the word searched
- `:noh` --> removes the highlight

## Visual Mode

- `v` --> select character-wise
- `V` --> select line-wise
- `Ctrl + v` --> select by block