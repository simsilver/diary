Title: GLEW for MinGW in Windows
Date: 2016-11-16 14:01
Tags: glew, mingw
Category: 备忘

在 glew 官方站下到的库是 .lib 格式的，
在QT上使用 mingw 里的 gcc 编译时老不成功，
就找了下如何编译成 mingw 的 .a 格式
[参考链接][1]
~~~
mkdir lib
gcc -DGLEW_NO_GLU -O2 -Wall -W -Iinclude  -DGLEW_BUILD -o src/glew.o -c src/glew.c
gcc -shared -Wl,-soname,libglew32.dll -Wl,--out-implib,lib/libglew32.dll.a    -o lib/glew32.dll src/glew.o -L/mingw/lib -lglu32 -lopengl32 -lgdi32 -luser32 -lkernel32
 
ar cr lib/libglew32.a src/glew.o
 
gcc -DGLEW_NO_GLU -DGLEW_MX -O2 -Wall -W -Iinclude  -DGLEW_BUILD -o src/glew.mx.o -c src/glew.c
gcc -shared -Wl,-soname,libglew32mx.dll -Wl,--out-implib,lib/libglew32mx.dll.a -o lib/glew32mx.dll src/glew.mx.o -L/mingw/lib -lglu32 -lopengl32 -lgdi32 -luser32 -lkernel32
 
ar cr lib/libglew32mx.a src/glew.mx.o
~~~

最后发现可能是没有指定 glu 和 opengl 库的原因，
不过没有还原尝试


[1]: https://bruceoutdoors.wordpress.com/2014/07/16/glew-for-mingw-in-windows/
