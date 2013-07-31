---
layout : post
tltle : Sublime Text 2 – 跳至行首与行末的快捷键
tags : SublimeText  
---

Sublime Text越来越流行，但却没有提供其他编辑器好用的Ctrl + left / right方向键跳转至行首或行末的快捷键！

那让我们自己来设置一下，菜单中依次打开 Sublime Text 2 > Preferences > Key Bindings – User:

然后将下面的代码贴入打开的文件”Default (OSX).sublime-keymap” 中：

mac中使用：

```javascript
{ "keys": ["super+right"], "command": "move_to", "args": { "to": "hardeol" } },
{ "keys": ["super+left"], "command": "move_to", "args": { "to": "hardbol" } }
```
Windows中可以使用 Home 和 End 键盘

```javascript
{"keys":["home"], "command":"move_to", "args":{"to":"bol"} },
{"keys":["end"], "command":"move_to", "args":{"to":"eol"} }
```
