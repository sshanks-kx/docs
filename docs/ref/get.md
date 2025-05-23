---
title: get, set – Reference – kdb+ and q documentation
description: get and set are q keywords that read or set value of a variable or a kdb+ data file.
author: Stephen Taylor
keywords: get, kdb+, q, set
---
# :fontawesome-solid-database: `get`, `set`

_Read or set the value of a variable or a kdb+ data file_




## `get`

_Read or memory-map a variable or kdb+ data file_

```syntax
get x     get[x]
```

Where `x` is

-   the name of a global variable as a symbol atom
-   a [file or folder](../basics/glossary.md#file-symbol) named as a symbol atom or vector

returns its value.

Signals a `type` error if the file is not a kdb+ data file.

Used to map columns of databases in and out of memory when querying splayed databases, and can be used to read q log files, etc.

```q
q)a:42
q)get `a
42

q)\l trade.q
q)`:NewTrade set trade                  / save trade data to file
`:NewTrade
q)t:get`:NewTrade                       / t is a copy of the table
q)`:SNewTrade/ set .Q.en[`:.;trade]     / save splayed table
`:SNewTrade/
q)s:get`:SNewTrade/                     / s has columns mapped on demand
```

!!! note "`value` is a synonym for `get`"

    By convention, [`value`](value.md) is used for other purposes. But the two are completely interchangeable.

    ```q
    q)value "2+3"
    5
    q)get "2+3"
    5
    ```

    <!-- FIXME: describe other uses. -->

:fontawesome-solid-book:
[`eval`](eval.md),
[`value`](value.md)



## `set`

_Assign a value to a global variable
<br>
Persist an object as a file or directory_

```{.syntax style="font-size: .8em"}
nam set y                 set[nam;y]                /set global var nam
file set y                set[file;y]               /serialize y to file
dir set t                 set[dir;t]                /splay t to dir
(file;lbs;alg;lvl) set y  set[(file;lbs;alg;lvl);y] /write y to file, compressed and/or encrypted
(dir;lbs;alg;lvl) set t   set[(dir;lbs;alg;lvl);t]  /splay t to dir, compressed and/or encrypted
(dir;dic) set t           set[(dir;dic);t]          /splay t to dir, compressed and/or encrypted
```

Where

```txt
alg   integer atom     compression/encryption algorithm
dic   dictionary       compression/encryption specifications
dir   filesymbol       directory in the filesystem
file  filesymbol       file in the filesystem
lbs   integer atom     logical block size
lvl   integer atom     compression level
nam   symbol atom      valid q name
t     table
y     (any)            any q object
```

:fontawesome-solid-database:
[Compression parameters `alg`, `lbs`, and `lvl`](../kb/file-compression.md#compression-parameters)
<br>
:fontawesome-solid-database:
[Encryption parameters `alg` and `lbs`](../kb/dare.md#encryption)
<br>
[Compression/Encryption specification dictionary](#compressionencryption)

Examples:
```q
q)`a set 42                         / set global variable
`a
q)a
42

q)`:a set 42                        / serialize object to file
`:a

q)t:([]tim:100?23:59;qty:100?1000)  / splay table
q)`:tbl/ set t
`:tbl/

q)(`:ztbl;17;2;6) set t             / serialize compressed
`:ztbl

q)(`:ztbl/;17;2;6) set t            / splay table compressed
`:ztbl/

q)(`:ztbl/;17;16;6) set t           / splay table encrypted (since v4.0 2019.12.12)
`:ztbl/
```

Anymap write detects consecutive deduplicated (address matching) top-level objects, skipping them to save space (since v4.1t 2021.06.04, v4.0 2023.01.20)

```q
q)a:("hi";"there";"world")
q)`:a0 set a
`:a0
q)`:a1 set a@where 1000 2000 3000
`:a1
q)(hcount`$":a0#")=hcount`$":a1#"
0b
```

Since 4.1t 2023.09.29,4.0 2023.11.03 when writing anymap, empty vectors without attributes are deduplicated automatically (including enum vectors when the enum name is 'sym').
Since 4.1t 2021.06.04,4.0 2023.01.20 improved memory efficiency of writing nested data sourced from a type 77 (anymap) file, commonly encountered during compression of files. e.g.
```q
q)`:a set 500000 100#"abc";system"ts `:b set get`:a" / was 76584400 bytes, now 8390208.
```


### Splayed table

To splay a table `t` to directory `dir`

-   `dir` must be a filesymbol that ends with a `/`
-   `t` must have no primary keys
-   columns of `t` must be vectors or [compound lists](../basics/glossary.md#compound-list)
-   symbol columns in `t` must be fully enumerated

:fontawesome-solid-database:
[Splayed tables](../kb/splayed-tables.md)


### Format

`set` saves the data in a binary format akin to tag+value, retaining the structure of the data in addition to its value.

```q
q)`:data/foo set 10 20 30
`:data/foo
q)read0 `:data/foo
"\376 \007\000\000\000\000\000\003\000\000\000\000\000\000\000"
"\000\000\000\000\000\000\000\024\000\000\000\000\000\000\000\036\000..
```

!!! danger "Setting variables in the KX namespaces can result in undesired and confusing behavior."

    These are `.h`, `.j`, `.Q`, `.q`, `.z`, and any other namespaces with single-character names.

[](){#compression}
### Compression/Encryption

For

```q
(fil;lbs;alg;lvl) set y   / write y to fil, compressed and/or encrypted
(dir;lbs;alg;lvl) set t   / splay t to dir, compressed and/or encrypted
```

Arguments `lbs`, `alg`, and `lvl` are [compression parameters](../kb/file-compression.md#compression-parameters) and/or [encryption parameters](../kb/dare.md#encryption).

Splay table `t` to directory `ztbl/` with gzip compression:

```q
q)(`:ztbl/;17;2;6) set t
`:ztbl/
```

For

```q
(dir;dic) set t            / splay t to dir, compressed
```

the keys of `dic` are either column names of `t` or the null symbol `` `  ``. The value of each entry is an integer vector: `lbs`, `alg`, and `lvl`.

Compression/encryption for unspecified columns is specified either by an entry for the null symbol (as below) or by [`.z.zd`](dotz.md#zzd-compressionencryption-defaults).

```q
q)m1:1000000
q)t:([]a:m1?10;b:m1?10;c:m1?10;d:m1?10)

q)/specify compression for cols a, b and defaults for others
q)show dic:``a`b!(17 5 3;17 2 6;17 2 6)
 | 17 5 3
a| 17 2 6
b| 17 2 6
q)(`:ztbl/;dic) set t               / splay table compressed
`:ztbl/
```

!!! warning "Compression may speed up or slow down the execution of `set`. The [performance impact](../kb/file-compression.md#performance) depends mainly on the data characteristics and the storage speed."

----
:fontawesome-solid-database:
[Database: tables in the filesystem](../database/index.md)
<br>
:fontawesome-solid-book-open:
[File system](../basics/files.md)
<br>
:fontawesome-solid-database:
[File compression](../kb/file-compression.md)
<br>
:fontawesome-regular-map:
[Compression in kdb+](../wp/compress/index.md)
<br>
:fontawesome-solid-database:
[Data at rest encryption (DARE)](../kb/dare.md)
