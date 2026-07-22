```markdown
# Secret of the Polyglot

| **Challenge**  | Secret of the Polyglot       |
| -------------- | ---------------------------- |
| **Event**      | picoCTF 2024                 |
| **Category**   | Forensics                    |
| **Difficulty** | Easy (100 pts)               |
| **Author**     | syreal                       |

---

## Challenge Description

> The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?
>
> Download the suspicious file [here](https://artifacts.picoctf.net/c_titan/9/flag2of2-final.pdf).

---

## Hints

> 1. This problem can be solved by just opening the file in different ways.

---

## Initial Analysis

We download the file with the extension `.pdf`:

```bash
$ wget https://artifacts.picoctf.net/c_titan/9/flag2of2-final.pdf
```

When we examine it using the `file` command, we get a surprising result:

```bash
$ file flag2of2-final.pdf
flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced
```

The file is actually a PNG image, despite having a `.pdf` extension. This indicates that the file is a **polyglot** – a file that is valid in two different formats. It can be opened as both a PDF and a PNG, revealing different parts of the flag.

---

## Extracting the Flag

### Part 1 – Opening as a PDF

Opening the file with a PDF viewer (or using the `open` command on macOS) shows the first part of the flag:

```bash
$ open flag2of2-final.pdf
```

The PDF viewer displays:

```text
1n_pn9_&_pdf_7f9bccd1}
```

### Part 2 – Opening as a PNG

We rename the file to have a `.png` extension and open it as an image:

```bash
$ mv flag2of2-final.pdf flag2of2-final.png
$ open flag2of2-final.png
```

The image reveals the first part of the flag:

```text
picoCTF{f1u3n7_
```

---

## Combining the Parts

By combining the two parts, we obtain the complete flag:

```text
picoCTF{f1u3n7_1n_pn9_&_pdf_7f9bccd1}
```

---

## Flag

```text
picoCTF{f1u3n7_1n_pn9_&_pdf_7f9bccd1}
```

---

## Exploitation Chain

```text
Download flag2of2-final.pdf
         │
         ▼
file command reveals it's a PNG
         │
         ▼
Open as PDF → get suffix: "1n_pn9_&_pdf_7f9bccd1}"
         │
         ▼
Rename to .png and open as image → get prefix: "picoCTF{f1u3n7_"
         │
         ▼
Concatenate → full flag
```

---

## Root Cause

The file is a **polyglot** – it is simultaneously a valid PNG and a valid PDF. Polyglots exploit the fact that different file formats have different structures, and it's possible to create a file that satisfies both specifications. The PNG parser interprets the file as an image, and the PDF parser interprets it as a document, with each displaying different content.

In this case, the flag was split across the two interpretations, so viewing the file in both ways allowed us to recover the complete flag.

---

## Lessons Learned

- A file's extension does not always reflect its true type; always use the `file` command to check.
- Polyglot files can hide information across multiple formats.
- Sometimes, you need to open a file with different applications to extract all pieces.
- This challenge reinforces the importance of examining files from multiple perspectives.

---

## Repository Structure

```text
.
├── README.md
└── flag2of2-final.pdf / png

```

---

## References

- [Polyglot files – Wikipedia](https://en.wikipedia.org/wiki/Polyglot_(computing))
- [Magic number (file format)](https://en.wikipedia.org/wiki/Magic_number_(programming))
```
