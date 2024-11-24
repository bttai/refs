## Compiling Code

```sh

# Installing the mingw-w64 cross-compiler in Kali
sudo apt install mingw-w64

# We can use mingw-w64 to compile the code into a Windows Portable Executable
i686-w64-mingw32-gcc exploit.c -o exploit.exe

# If there are erros we must fix them. Google is perhaps our friend
i686-w64-mingw32-gcc exploit.c -o exploit.exe -lws2_32


```
