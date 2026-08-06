This lab teaches you how to use **ExifTool** to inspect **metadata** stored in files. Metadata often includes information such as the author, software used to create the file, creation dates, camera model, GPS coordinates (if present), and document properties. Reviewing metadata is a common **OSINT** technique because it can reveal information that was unintentionally left in publicly shared files.

---

# Lab Objective

Learn how to:

* Install ExifTool
* View supported file types
* Download sample files
* Extract metadata
* Analyze metadata
* Export metadata to CSV
* Research interesting metadata values

---

# Step 1: Install ExifTool

Open **Kali Linux**.

Open a Terminal.

Update package lists:

```bash
sudo apt update
```

Install ExifTool:

```bash
sudo apt install libimage-exiftool-perl
```

Example:

```text
Reading package lists...

Installing...

libimage-exiftool-perl

Done.
```

---

# Step 2: Verify Installation

Type:

```bash
exiftool -ver
```

Example:

```text
13.xx
```

This confirms ExifTool is installed.

---

# Step 3: View All Supported Tags

ExifTool refers to metadata fields as **tags**.

To see the tags it understands:

```bash
exiftool -list
```

Example output (very long):

```text
Aperture
Author
CameraModel
GPSLatitude
GPSLongitude
Software
DateTimeOriginal
...
```

There are thousands of supported tags.

---

# Step 4: View Supported File Types

Run:

```bash
exiftool -listf
```

Example output:

```text
Supported file types:

PDF
DOC
DOCX
TXT
JPEG
PNG
GIF
TIFF
MP3
MP4
AVI
ZIP
RAR
...
```

---

# Step 5: Complete the Lab Table

Some common file formats supported by ExifTool are:

| Type      | Examples                            |
| --------- | ----------------------------------- |
| Documents | PDF, TXT, DOC, DOCM, DOCX, HTML     |
| Audio     | MP3, WAV, FLAC, AIFF, WMA           |
| Video     | AVI, MOV, MP4, MPEG, WEBM, WMV      |
| Graphics  | JPG, JPEG, PNG, GIF, TIFF, BMP, SVG |
| Archives  | ZIP, RAR, GZ, GZIP                  |

These match the expected answers in your lab.

---

# Step 6: Obtain Sample Files

The lab suggests downloading publicly available files.

Examples include:

* PDF documents
* Word documents
* JPEG images
* PNG images

Save them in a folder.

Example:

```text
/home/kali/Documents/metadata_lab/
```

Suppose the folder contains:

```text
photo1.jpg

photo2.jpg

report.pdf

manual.docx
```

---

# Step 7: View Metadata of One File

Example:

```bash
exiftool photo1.jpg
```

Example output:

```text
File Name                       : photo1.jpg

File Size                       : 2.1 MB

Image Width                     : 4032

Image Height                    : 3024

Camera Model Name               : Canon EOS R6

Create Date                     : 2024:08:15 10:34:21

Software                        : Adobe Photoshop

GPS Latitude                    : (if present)

GPS Longitude                   : (if present)
```

Each line is a metadata tag.

---

# Step 8: Understand Common Tags

Example:

```text
Author : John Smith
```

Means

The document creator was recorded as **John Smith**.

---

Example:

```text
Company : ABC Corporation
```

Means

The software recorded the organization name.

---

Example:

```text
Software : Microsoft Word
```

Shows which application created or modified the document.

---

Example:

```text
Camera Model : iPhone 15 Pro
```

Shows the device used to capture the image.

---

Example:

```text
GPS Latitude
GPS Longitude
```

If present, these indicate where the image was taken.

---

# Step 9: Analyze a PDF

Example:

```bash
exiftool report.pdf
```

Example output:

```text
Title

Author

Creator

Producer

Create Date

Modify Date

PDF Version
```

You may discover:

```text
Author : Alice

Creator : Microsoft Word

Company : ABC Ltd
```

---

# Step 10: Analyze a Word Document

Example:

```bash
exiftool report.docx
```

Example output:

```text
Author : John Doe

Last Modified By : Jane Doe

Company : XYZ Ltd

Template : Normal.dotm

Application : Microsoft Office
```

This information may help identify how the document was created.

---

# Step 11: Analyze an Entire Folder

Instead of one file:

```bash
exiftool /home/kali/Documents/metadata_lab
```

ExifTool processes every supported file inside the folder.

Example:

```text
======== photo1.jpg ========

======== photo2.jpg ========

======== report.pdf ========

======== manual.docx ========
```

---

# Step 12: Export Metadata to CSV

To save metadata in CSV format:

```bash
exiftool -csv /home/kali/Documents/metadata_lab > metadata.csv
```

Example:

```text
metadata.csv created
```

Open it:

```bash
cat metadata.csv
```

Or with LibreOffice Calc:

```bash
libreoffice metadata.csv
```

The CSV contains one row per file with metadata fields as columns.

---

# Step 13: Research Interesting Values

Example metadata:

```text
Creator : gd-jpeg v1.0
```

This indicates the image was generated using the **PHP GD** graphics library.

You can research that software version to learn about:

* Its purpose
* Whether it is outdated
* Whether any known vulnerabilities have been reported for that version

This can help you understand the technologies used by an organization, but remember that metadata alone does **not** prove a system is currently vulnerable.

---

# Example Metadata Analysis

Suppose you inspect:

```text
photo.jpg
```

Metadata:

```text
Author : John Smith

Software : Adobe Photoshop

Camera : Nikon D850

Create Date : 2023

GPS : None
```

Possible observations:

* The creator name is exposed.
* The editing software is identified.
* The camera model is visible.
* No GPS information is present.

---

Suppose a PDF contains:

```text
Author : HR Department

Company : ABC Corporation

Creator : Microsoft Word 2016
```

Possible observations:

* Organization name is visible.
* Document creator information is exposed.
* Office software information is available.

---

# Answer to the Lab Question

**Did you find any information that could be useful to ethical hackers?**

Example answer:

> Yes. The metadata included information such as the document author, organization name, software used to create the file, creation and modification dates, and the device or camera model. Some files may also contain GPS coordinates or other identifying information. This information can help assess what technologies are used and identify data that organizations may want to remove before publishing files.

---

# Summary Workflow

```text
Install ExifTool
        │
        ▼
Check Version
        │
        ▼
List Supported Tags
        │
        ▼
List Supported File Types
        │
        ▼
Download Sample Files
        │
        ▼
Run ExifTool on One File
        │
        ▼
Review Metadata
        │
        ▼
Run ExifTool on Folder
        │
        ▼
Export Results to CSV
        │
        ▼
Research Interesting Metadata Values
        │
        ▼
Document Findings
```

### Best Practices

* Only analyze files you own or are authorized to examine.
* Before publishing documents or images, consider removing unnecessary metadata that could reveal internal information.
* Treat metadata as one source of evidence; verify any conclusions with additional information rather than relying on a single tag.
