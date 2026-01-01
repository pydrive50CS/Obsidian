```bash
sudo pacman -S pandoc

pandoc main.tex \  
 --citeproc \  
 --bibliography=references.bib \  
 --csl=ieee.csl \  
 -o output.docx
```

