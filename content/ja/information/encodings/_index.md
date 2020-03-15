---
title: "Character encodings"
weight: 1
---

<font color="Gray">This page describes what character encodings are and how they should be used.</font>

■ Overview
------

A character encoding is a way to translate characters into bytes that can be stored on the hard drive. In the beginnings, one byte (256 states) was used to encode one character. As different writing systems around the world have different requirements for characters to be encoded, there are a lot of different encodings in use throughout the world. Problematically, if the encoding of a text file is not known in advance, misinterpreting the encoding can result in mojibake (meaningless sequence of characters).

Example of a Japanese text encoded in Shift_JIS (requires appropriate fonts available):

{{% code %}}  
ひらがなカタカナ漢字  
{{% /code %}}

The byte sequence generated from the previous example in Shift_JIS:

{{% code %}}  
82 D0 82 E7 82 AA 82 C8 83 4A 83 5E 83 4A 83 69 8A BF 8E 9A  
{{% /code %}}

What happens if the byte sequence is misinterpreted as being ISO 8859-1 (Latin-1) instead of Shift_JIS: 

{{% code %}}  
‚Ð‚ç‚ª‚ÈƒJƒ^ƒJƒiŠ¿Žš  
{{% /code %}}

Usually, one particular encoding only allows to use the writing system for which it was designed for. Unicode, a standard which covers basically all characters as used in writing systems throughout the world, provides a solution here.

While Unicode is wide-spread today, there are still some issues. First of all, Unicode is not directly a character encoding. Instead, so-called Unicode Transformation Formats (UTF) provide the means to encode particular characters. Transformation formats frequently used are UTF-8 and UTF-16.

Route and train files are usually, for compatibility reasons, allowed to be encoded in any arbitrary encoding. As the encoding is not known in advance, the user will need to select the correct encoding for the route and the train in the settings tab in the main menu. Of course this is a nuisance and should be avoided somehow.

## ■ The Byte Order Mark

UTF-16 descends from a character encoding (UTC-2) which always used two bytes to encode one character. As such, the order of those two bytes matters. In order to know in which order the bytes appear, a so-called byte order mark is frequently prepended to the text, usually automatically and transparently by the text editor. The byte order mark provides decoders a means of detecting in which byte order the file was saved in. Additionally, the byte order mark provides a fairly safe way of detecting that the text file is saved in Unicode in the first place.

While not technically necessary, the byte order mark is also frequently used for UTF-8. As UTF-8 does not have a byte order issue, the sole purpose of using a byte order mark with UTF-8 is to provide a means to flagging the file as being encoded in UTF-8.

As said, while Unicode allows to encode virtually all characters used in all writing systems around the world, it does not necessarily make working with text files easier if the encoding to be used was not agreed on.

Using a byte order mark provides openBVE the ability to automatically detect the encoding for each individual file. If you have a good text editor, you will not only be able to select the encoding manually, but also if you want to save with a byte order mark or not. Notepad (Windows) always saves with a byte order mark if UTF-8 is selected as the character encoding upon saving the file, to make an acceptable example.

Technically, the byte order mark is the first bytes of a particular text file. The byte order marks which can be automatically detected by openBVE are:

{{% table %}}

| Encoding               | Hexadecimal representation |
| ---------------------- | -------------------------- |
| UTF-8                  | EF BB BF                   |
| UTF-16 (big endian)    | FE FF                      |
| UTF-16 (little endian) | FF FE                      |
| UTF-32 (big endian)    | 00 00 FE FF                |
| UTF-32 (little endian) | FF FE 00 00                |

{{% /table %}}

You are encouraged to always save text files in one of those encodings with a byte order mark in order for openBVE to automatically detect the encoding used.