```
**gzip**
gzip filename.txt
gunzip filename.txt.gz

**bzip2**
bzip2 filename.txt
bunzip2 filename.txt.bz2

**xz**
xz filename.txt
unxz filename.txt.xz

**tar**
tar -cvf archive.tar folder/

**Create a compressed tarball (`.tar.gz`)**
tar -czvf archive.tar.gz folder/

**Extract `.tar.gz` file:**
tar -xzvf archive.tar.gz

**Extract `.tar.bz2` or `.tar.xz`:**
tar -xjvf archive.tar.bz2   # for bzip2  
tar -xJvf archive.tar.xz    # for xz

**zip**
zip archive.zip file1.txt file2.txt
zip -r archive.zip folder/
unzip archive.zip


**7z**
7z a archive.7z file_or_folder
7z x archive.7z
sudo apt install p7zip-full

```