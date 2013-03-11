sqlite-cmake
============
Testing @ Mac OS 10.8.2
<pre>
$ git clone --recursive git://github.com/changyy/sqlite-cmake.git
$ cd sqlite-cmake
$ mkdir build
$ cmake .. -DPCRE_SUPPORT_UTF:bool=ON -DBUILD_SHARED_LIBS:bool=ON
$ make
$ ls lib/
libsqlite3.a            libsqlite3.dylib        libsqlite3_regexp.dylib
$ ls bin/
sqlite3
$ ./bin/sqlite3
SQLite version 3.7.15.2 2013-01-09 11:53:05
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite> 
sqlite> .load ./lib/libsqlite3_regexp.dylib
sqlite> CREATE TABLE A ( NAME VARCHAR(256) );
sqlite> INSERT INTO A (NAME) VALUES ('Hello'), ('World');
sqlite> SELECT * FROM A;
Hello
World
sqlite> SELECT * FROM A WHERE NAME REGEXP 'W.*';   
World
sqlite> .quit
</pre>
