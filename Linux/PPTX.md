### **1. Using unoconv / LibreOffice (GUI-less merge)**

Merge PDF

`libreoffice --headless --invisible --convert-to pdf *.pptx pdfunite file1.pdf file2.pdf merged.pdf`
`pdfunite VAPT-Scoping-Fundamentals.pdf The-Art-and-Science-of-VAPT-Scoping2.pdf merged.pdf`


Then if you want it back as PPTX:

`libreoffice --convert-to pptx merged.pdf`