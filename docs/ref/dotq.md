---
title: The .Q namespace – tools | Reference | kdb+ and q documentation
description: The .Q namespace contains utility objects for q programming
author: Stephen Taylor
---
# :fontawesome-solid-wrench: The `.Q` namespace


_Tools_

<div markdown="1" class="typewriter">
**General**                           **Datatype**
 [addmonths](#addmonths)                         [btoa        b64 encode](#btoa-b64-encode)
 [dd       join symbols](#dd-join-symbols)             [j10         encode binhex](#j10-encode-binhex)
 [f        precision format](#f-precision-format)         [j12         encode base 36](#j12-encode-base-36)
 [fc       parallel on cut](#fc-parallel-on-cut)          [ty          type](#ty-type)
 [ff       append columns](#ff-append-columns)           [x10         decode binhex](#x10-decode-binhex)
 [fmt      precision format](#fmt-precision-format)         [x12         decode base 36](#x12-decode-base-36)
 [ft       apply simple](#ft-apply-simple) 
 [fu       apply unique](#fu-apply-unique)            **Database**
 [gc       garbage collect](#gc-garbage-collect)          [chk         fill HDB](#chk-fill-hdb)
 [gz       GZip](#gz-gzip)                     [dpft dpfts  save table](#dpft-save-table)
 [id       sanitize](#id-sanitize)                 [dpt  dpts   save table unsorted](#dpt-save-table-unsorted)
 [qt       is table](#qt-is-table)                 [dsftg       load process save](#dsftg-load-process-save)
 [res      keywords](#res-keywords)                 [en          enumerate varchar cols](#en-enumerate-varchar-cols)
 [s        plain text](#s-plain-text)               [ens         enumerate against domain](#ens-enumerate-against-domain)
 [s1       string representation](#s1-string-representation)    [fk          foreign key](#fk-foreign-key)
 [sha1     SHA-1 encode](#sha1-sha-1-encode)             [hdpf        save tables](#hdpf-save-tables)
 [V        table to dict](#v-table-to-dict)            [l           load](#l-load)
 [v        value](#v-value)                    [ld          load and group](#ld-load-and-group)
 [view     subview](#view-subview)                  [li          load partitions](#li-load-partitions)
                                   [lo          load without](#lo-load-without)
 **Constants**                         [M           chunk size](#m-chunk-size)
 [A a an   alphabets](#a-upper-case-alphabet)                [qp          is partitioned](#qp-is-partitioned)
 [b6       bicameral alphanums](#b6-bicameral-alphanums)      [qt          is table](#qt-is-table)
 [n nA     nums & alphanums](#n-nums)
                                  **Partitioned database state**
 **Debug/Profile**                     [bv          build vp](#bv-build-vp)
 [bt       backtrace](#bt-backtrace)                [bvi         build incremental vp](#bvi-build-incremental-vp)
 [prf0     code profiler](#prf0-code-profiler)            [cn          count partitioned table](#cn-count-partitioned-table)
 [sbt      string backtrace](#sbt-string-backtrace)         [D           partitions](#d-partitions)
 [trp      extend trap at](#trp-extend-trap-at)           [ind         partitioned index](#ind-partitioned-index)
 [trpd     extend trap](#trpd-extend-trap)              [MAP         maps partitions](#map-maps-partitions)
 [ts       time and space](#ts-time-and-space)           [par         locate partition](#par-get-expected-partition-location)
                                   [PD          partition locations](#pd-partition-locations)
 **Environment**                       [pd          modified partition locns](#pd-modified-partition-locations)
 [K k      version](#k-version-date)                  [pf          partition field](#pf-partition-field)
 [w        memory stats](#w-memory-stats)             [pn          partition counts](#pn-partition-counts)
                                   [pt          partitioned tables](#pt-partitioned-tables)
 **Environment (Command-line)**        [PV          partition values](#pv-partition-values)
 [def      command defaults](#def-command-defaults)         [pv          modified partition values](#pv-modified-partition-values) 
 [opt      command parameters](#opt-command-parameters)       [qp          is partitioned](#qp-is-partitioned)
 [x        non-command parameters](#x-non-command-parameters)   [vp          missing partitions](#vp-missing-partitions)
  
 **IPC**                               **Segmented database state**
 [addr     IP/host as int](#addr-iphost-as-int)           [P           segments](#p-segments)
 [fps fpn  pipe streaming](#fpn-pipe-streaming)           [u           date based](#u-date-based) 
 [fs  fsn  file streaming](#fs-file-streaming)
 [hg       HTTP get](#hg-http-get)                **File I/O**
 [host     IP to hostname](#host-ip-to-hostname)           [Cf          create empty nested char file](#cf-create-empty-nested-char-file)
 [hp       HTTP post](#hp-http-post)                [Xf          create file](#xf-create-file)
</div>


Functions defined in `q.k` are loaded as part of the ‘bootstrap’ of kdb+. Some are exposed in the default namespace as the q language. Others are documented here as utility functions in the `.Q` [namespace](../basics/namespaces.md).

!!! warning "The `.Q` namespace is reserved for use by KX, as are all single-letter namespaces."

    Consider all undocumented functions in the namespace as [exposed infrastructure](../basics/exposed-infrastructure.md) – and do not use them.

In non-partitioned databases the partitioned database state variables remain undefined.


## `A` (upper-case alphabet)
## `a` (lower-case alphabet)
## `an` (all alphanumerics)

```syntax
.Q.A       / upper-case alphabet
.Q.a       / lower-case alphabet
.Q.an      / all alphanumerics
```

Strings: upper-case Roman alphabet (`.Q.A`), lower-case Roman alphabet (`.Q.a`), and all alphanums (`.Q.an`).

```q
q).Q.A
"ABCDEFGHIJKLMNOPQRSTUVWXYZ"
q).Q.a
"abcdefghijklmnopqrstuvwxyz"
q).Q.an
"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ_0123456789"
```


## `addmonths`

```syntax
.Q.addmonths[x;y]
```

Where `x` is a date and `y` is an int, returns `x` plus `y` months.

```q
q).Q.addmonths[2007.10.16;6 7]
2008.04.16 2008.05.16
```

If the date `x` is near the end of the month and (`x.month + y`)’s month has fewer days than `x.month`, the result may spill over to the following month.

```q
q).Q.addmonths[2006.10.29;4]
2007.03.01
```

:fontawesome-solid-book-open:
[Mathematics with temporals](../basics/math.md#mathematics-with-temporals)
<br>
:fontawesome-solid-graduation-cap:
[How to handle temporal data in q](../kb/temporal-data.md)

[](){#addr-ip-address}
## `addr` (IP/host as int)

```syntax
.Q.addr x
```

Where `x` is a hostname or IP address as a symbol atom, returns the IP address as an integer (same format as [`.z.a`](dotz.md#za-ip-address))

```q
q).Q.addr`localhost
2130706433i
q).Q.host .Q.addr`localhost
`localhost
q).Q.addr`localhost
2130706433i
q)256 vs .Q.addr`localhost
127 0 0 1
```

:fontawesome-regular-hand-point-right:
[`.Q.host`](#host-ip-to-hostname) (IP to hostname)
<br>
:fontawesome-solid-book-open:
[`vs`](vs.md)


## `b6` (bicameral-alphanums)

```syntax
.Q.b6
```

Returns upper- and lower-case alphabet and numerics.

```q
q).Q.b6
"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
```

Used for [binhex](#j10-encode-binhex) encoding and decoding.


## `bt` (backtrace)

```syntax
.Q.bt[]
```

Dumps the backtrace to stdout at any point during execution or debug.

```q
q)f:{{.Q.bt[];x*2}x+1}
q)f 4
  [2]  f@:{.Q.bt[];x*2}
           ^
  [1]  f:{{.Q.bt[];x*2}x+1}
          ^
  [0]  f 4
       ^
10
q)g:{a:x*2;a+y}
q)g[3;"hello"]
'type
  [1]  g:{a:x*2;a+y}
                 ^
q)).Q.bt[]
>>[1]  g:{a:x*2;a+y}
                 ^
  [0]  g[3;"hello"]
       ^
```

`>>` marks the current stack frame. (Since V4.0 2020.03.23.)

The debugger itself occupies a stack frame, but its source is hidden. (Since V3.5 2017.03.15.)


## `btoa` (b64 encode)

```syntax
.Q.btoa x
```

```q
q).Q.btoa"Hello World!"
"SGVsbG8gV29ybGQh"
```

Since V3.6 2018.05.18.


## `bv` (build vp)


```syntax
.Q.bv[]
.Q.bv[`]
```

In partitioned DBs, construct the dictionary [`.Q.vp`](#vp-missing-partitions) of table schemas for tables with missing partitions. Optionally allow tables to be missing from partitions, by scanning partitions for missing tables and taking the tables’ prototypes from the last partition. 

After loading/re-loading from the filesystem, invoke `.Q.bv[]` to (re)populate `.Q.vt`/`.Q.vp`, which are used inside `.Q.p1` during the partitioned select `.Q.ps`.
(Since V2.8 2012.01.20, modified  V3.0 2012.01.26)

If your table exists at least in the latest partition (so there is a prototype for the schema), you could use `.Q.bv[]` to create empty tables on the fly at run-time without having to create those empties on disk. 

``.Q.bv[`]`` (with argument) will use prototype from first partition instead of last. (Since V3.2 2014.08.22.)

!!! note "Some admins prefer to see errors instead of auto-manufactured empties for missing data, which is why `.Q.bv` is not the default behavior."

```q
q)n:100
q)t:([]time:.z.T+til n;sym:n?`2;num:n)
q).Q.dpft[`:.;;`sym;`t]each 2010.01.01+til 5
`t`t`t`t`t
q)tt:t
q).Q.dpft[`:.;;`sym;`tt]last 2010.01.01+til 5
`tt
q)\l .
q)tt
+`sym`time`num!`tt
q)@[get;"select from tt";-2@]; / error
./2010.01.01/tt/sym: No such file or directory
q).Q.bv[]
q).Q.vp
tt| +`date`sym`time`num!(`date$();`sym$();`time$();`long$())
q)@[get;"select from tt";-2@]; / no error
```


## `bvi` (build incremental vp)

It offers the same functionality as [`.Q.bv`](#bv-build-vp), but scans only new partitions loaded in the hdb since the last time `.Q.bv` or `.Q.bvi` was run. Since v4.1 2024.09.13.


## `Cf` (create empty nested char file)

!!! warning "Deprecated"

    Deprecated since 4.1t 2022.03.25. Using resulting files could return file format errors since 3.6.

```syntax
.Q.Cf x
```

A projection of [`.Q.Xf`](#xf-create-file): i.e. ``.Q.Xf[`char;]``


## `chk` (fill HDB)

```syntax
.Q.chk x
```

Where `x` is a HDB as a filepath, fills tables missing from partitions using the most recent partition containing the table as a template, and reports which partitions (but not which tables) it is fixing.

```q
q).Q.chk[`:hdb]
()
()
,`:/db/2009.01.04
,`:/db/2009.01.03
```

!!! tip "Q must have write permission for the HDB area to create missing tables"

    If it signals an error similar to

    ```q
    './2010.01.05/tablename/.d: No such file or directory
    ```

    check the process has write permissions for that filesystem.

:fontawesome-solid-street-view:
_Q for Mortals_
[§14.5.2 `.Q.chk`](/q4m3/14_Introduction_to_Kdb%2B/#1457-qchk)


## `cn` (count partitioned table)

```syntax
.Q.cn x
```

Where `x` is a partitioned table, passed by value, returns its count. Populates [`.Q.pn`](#pn-partition-counts) cache.


## `D` (partitions)

```syntax
.Q.D
```

In segmented DBs, contains a list of the partitions – conformant to [`.Q.P`](#p-segments) – that are present in each segment.

`.Q.P!.Q.D` can be used to create a dictionary of partition-to-segment information.

```q
q).Q.P
`:../segments/1`:../segments/2`:../segments/3`:../segments/4
q).Q.D
2010.05.26 2010.05.31
,2010.05.27
2010.05.28 2010.05.30
2010.05.29 2010.05.30
q).Q.P!.Q.D
:../segments/1| 2010.05.26 2010.05.31
:../segments/2| ,2010.05.27
:../segments/3| 2010.05.28 2010.05.30
:../segments/4| 2010.05.29 2010.05.30
```


## `dd` (join symbols)

```syntax
.Q.dd[x;y]
```

Shorthand for `` ` sv x,`$string y``. Useful for creating filepaths, suffixed stock symbols, etc.

```q
q).Q.dd[`:dir]`file
`:dir/file
q){x .Q.dd'key x}`:dir
`:dir/file1`:dir/file2
q).Q.dd[`AAPL]"O"
`AAPL.O
q)update sym:esym .Q.dd'ex from([]esym:`AAPL`IBM;ex:"ON")
esym ex sym
--------------
AAPL O  AAPL.O
IBM  N  IBM.N
```

[](){#def-parse-options}
## `def` (command defaults)

_Default values and type checks for command-line arguments parsed with [`.Q.opt`](#opt-command-parameters)_

```syntax
.Q.def[x;y]
```

Where `x` is a dictionary of default parameter names and values, and `y` is the output of `.Q.opt`.

Types are inferred from the default values provided, which must be an atom type.

```bash
$ q -abc 123 -xyz 321
```
```q
q).Q.def[`abc`xyz`efg!(1;2.;`a)].Q.opt .z.x
abc| 123
xyz| 321f
efg| `a
```

If a command-line value cannot be [converted to the data type](tok.md) of the default value, a [null](../basics/datatypes.md) is produced

```bash
$ q -param1 11 -param2 2000.01.01 -param3 wrong
```
```q
q).Q.def[`param1`param2`param3!(1;1999.01.01;23.1)].Q.opt .z.x
param1| 11
param2| 2000.01.01
param3| 0n
```

:fontawesome-solid-hand-point-right:
[`.z.x`](dotz.md#zx-argv) (argv), [`.z.X`](dotz.md#zx-raw-command-line) (raw command line), [`.z.f`](dotz.md#zf-file) (file), [`.z.q`](dotz.md#zq-quiet-mode) (quiet mode), [`.Q.opt`](#opt-command-parameters) (command parameters), [`.Q.x`](#x-non-command-parameters) (non-command parameters)


## `dpft` (save table)
## `dpfts` (save table with symtable)
## `dpt` (save table unsorted)
## `dpts` (save table unsorted with symtable)


```syntax
.Q.dpft[d;p;f;t]
.Q.dpfts[d;p;f;t;s]
.Q.dpt[d;p;t]
.Q.dpts[d;p;t;s]
```

Where

-   `d` is a directory handle
-   `p` is a partition of a database
-   `f` a field of the table (required to be present in table since 4.1t 2021.09.03) named by `t` below
-   `t`, the name (as a symbol) of a simple table whose columns are vectors or compound lists
-   `s` is the handle of a symtable

saves `t` splayed to partition `p`.

!!! warning "The table cannot be keyed."

    This would signal an `'unmappable` error if there are columns which are not vectors or simple nested columns (e.g. char vectors for each row).

It also rearranges the columns of the table so that the column specified by `f` is second in the table (the first column in the table will be the virtual column determined by the partitioning e.g. date).

Returns the table name if successful.

```q
q)trade:([]sym:10?`a`b`c;time:.z.T+10*til 10;price:50f+10?50f;size:100*1+10?10)
q).Q.dpft[`:db;2007.07.23;`sym;`trade]
`trade
q)delete trade from `.
`.
q)trade
'trade
q)\l db
q)trade
date       sym time         price    size
-----------------------------------------
2007.07.23 a   11:36:27.972 76.37383 1000
2007.07.23 a   11:36:27.982 77.17908 200
2007.07.23 a   11:36:28.022 75.33075 700
2007.07.23 a   11:36:28.042 58.64531 200
2007.07.23 b   11:36:28.002 87.46781 800
2007.07.23 b   11:36:28.012 85.55088 400
2007.07.23 c   11:36:27.952 78.63043 200
2007.07.23 c   11:36:27.962 90.50059 400
2007.07.23 c   11:36:27.992 73.05742 600
2007.07.23 c   11:36:28.032 90.12859 600
```

If you are getting an `'unmappable` error, you can identify the offending columns and tables:

```q
/ create 2 example tables
q)t:([]a:til 2;b:2#enlist (til 1;10))  / bad table, b is unmappable
q)t1:([]a:til 2;b:2#til 1)  / good table, b is mappable
q)helper:{$[(type x)or not count x;1;t:type first x;all t=type each x;0]};
q)select from (raze {([]table:enlist x;columns:enlist where not helper each flip .Q.en[`:.]`. x)} each tables[]) where 0<count each columns
table columns
-------------
t     b
```
`.Q.dpfts` allows the enum domain to be specified. Since V3.6 (2018.04.13)
```q
q)show t:([]a:10?`a`b`c;b:10?10)
a b
---
c 8
a 1
b 9
b 5
c 4
a 6
b 6
c 1
b 8
c 5
q).Q.dpfts[`:db;2007.07.23;`a;`t;`mysym]
`t
q)mysym
`c`a`b
```


## `dsftg` (load process save)

```syntax
.Q.dsftg[d;s;f;t;g]
```

Where

- `d` is `(dst;part;table)` where `table` has `M` rows
- `s` is `(src;offset;length)`
- `f` is fields as a symbol vector
- `t` is `(types;widths)`
- `g` is a unary post-processing function

loops `.Q.M&1000000` rows at a time.

:fontawesome-solid-hand-point-right:
[`.Q.M`](#m-chunk-size) (chunk size)

For example, loading TAQ DVD:

```q
q)d:(`:/dst/taq;2000.10.02;`trade)
q)s:(`:/src/taq;19;0)  / nonpositive length from end
q)f:`time`price`size`stop`corr`cond`ex
q)t:("iiihhc c";4 4 4 2 2 1 1 1)
q)g:{x[`stop]=:240h;@[x;`price;%;1e4]}
q).Q.dsftg[d;s;f;t;g]
```

## `en` (enumerate varchar cols)
## `ens` (enumerate against domain)

```syntax
.Q.en[dir;table]
.Q.ens[dir;table;name]
```

Where

-   `dir` is a symbol handle to a folder or generic null ([`::`](identity.md#null))
-   `table` is a table
-   `name` is a symbol atom naming a sym file in `dir`

When `dir` is a symbol handle, the function

-   creates if necessary the folder `dir`
-   gets `sym` from `dir` if it exists
-   enumerates against in-memory `sym` using the symbols in `table`
-   writes `sym` to file in `dir`
-   returns `table` with columns enumerated (for `.Q.ens`, against `name`)

    !!! warning "Locking ensures two processes do not write to the sym file at the same time"

    The following example uses `.Q.en` to enumerate to both the in-memory and disk `sym` domain, while
    saving the table output using [`set`](get.md#set):
    ```q
    q)t1:([]col1:`a`b`c;col2:1 2 3)
    q)`:/tmp/db/t1/ set .Q.en[`:/tmp/db;t1];
    q)sym                                     / contents of in-memory sym populated from symbols in table
    `a`b`c
    q)get `:/tmp/db/sym                       / on-disk sym same as in-memory sym
    `a`b`c
    q)get `:/tmp/db/t1/col1                   / col1 enumerated against sym domain
    `sym$`a`b`c
    ```
    Providing a new or updated table against an existing `sym` domain will read the existing on-disk sym domain before updating. 
    Both the in-memory and on-disk version are updated to reflect the new state. Continuing with the same example shows the 
    existing `sym` domain being altered:
    ```q
    q)t2:([]col1:`a`d`e;col2:1 2 3)
    q)`:/tmp/db/t2/ set .Q.en[`:/tmp/db;t2];  / enumerate additional table against existing sym domain
    q)sym                                     / in-memory sym now contains additional symbols
    `a`b`c`d`e
    q)get `:/tmp/db/sym                       / on-disk sym same as in-memory sym
    `a`b`c`d`e
    ```

When `dir` is a generic null (since 4.1 2025.01.17), the function

-   does not read/write/lock the `sym` file
-   enumerates against in-memory `sym` using the symbols in `table`, for example
    ```q
    q)t1:([]a:`a`b`c;b:1 2 3)
    q).Q.en[::;t1];
    q)sym
    `a`b`c
    q)t2:([]a:`a`d`e;b:1 2 3)
    q).Q.en[::;t2];
    q)sym
    `a`b`c`d`e
    ```

    !!! note "on-disk sym files should be kept in sync with in-memory enum domain"

`.Q.ens` allows enumeration against domains (and therefore filenames) other than `sym`.

```q
q)([]sym:`mysym$`a`b`c)~.Q.ens[`:db;([]sym:`a`b`c);`mysym]
```

Tables splayed across a directory must be fully enumerated and not keyed. The solution is to enumerate columns of type varchar before saving the table splayed.

:fontawesome-solid-book:
[`dsave`](dsave.md),
[Enum Extend](enum-extend.md),
[`save`](save.md)
<br>
:fontawesome-solid-graduation-cap:
[Enumerating symbol columns in a table](../kb/splayed-tables.md#enumerating-symbol-columns)
<br>
:fontawesome-solid-database:
[Splayed tables](../kb/splayed-tables.md)
<br>
:fontawesome-regular-map:
[Data-management techniques](../wp/data-management.md)
<br>
:fontawesome-regular-map:
[Working with sym files](../wp/symfiles.md#qen)
<br>
:fontawesome-solid-street-view:
_Q for Mortals_
[§14.2.8 Working with sym files](/q4m3/14_Introduction_to_Kdb%2B/#1428-working-with-sym-files)


[](){#f-format}
## `f` (precision format)

```syntax
.Q.f[x;y]
```

Where

-   `x` is an int atom
-   `y` is a numeric atom

returns `y` as a string formatted as a float to `x` decimal places.

Because of the limits of precision in a double, for `y` above `1e13` or the limit set by [`\P`](../basics/syscmds.md#p-precision), formats in scientific notation.

```q
q)\P 0
q).Q.f[2;]each 9.996 34.3445 7817047037.90 781704703567.90 -.02 9.996 -0.0001
"10.00"
"34.34"
"7817047037.90"
"781704703567.90"
"-0.02"
"10.00"
"-0.00"
```

The `1e13` limit is dependent on `x`. The maximum then becomes `y*10 xexp x` and that value must be less than `1e17` – otherwise you'll see sci notation or overflow.

```q
q)10 xlog 0Wj-1
18.964889726830812
```

:fontawesome-regular-hand-point-right:
[`.Q.fmt`](#fmt-precision-format) (precision format with length), [-27!(x;y)](../basics/internal.md#-27xy-ieee754-precision-format) (IEEE754 precision format)
<br>:fontawesome-solid-book-open:
[`\P`](../basics/syscmds.md#p-precision) (precision)


## `fc` (parallel on cut)

```syntax
.Q.fc[x;y]
```

Where

-   `x` is is a unary atomic function
-   `y` is a list

returns the result of evaluating `f vec` – using multiple threads if possible. (Since V2.6)

```q
q -s 8
q)f:{2 xexp x}
q)vec:til 100000
q)\t f vec
12
q)\t .Q.fc[f]vec
6
```

In this case the overhead of creating threads in [`peach`](each.md) significantly outweighs the computational benefit of parallel execution.

```q
q)\t f peach vec
45
```


## `ff` (append columns)

```syntax
.Q.ff[x;y]
```

Where

-   `x` is table to modify
-   `y` is a table of columns to add to `x` and set to null

returns `x`, with all new columns in `y`, with values in new columns set to null of the appropriate type.

If there is a common column in `x` and `y`, the column from `x` is kept (i.e. it will not null any columns that exist in `x`).

```q
q)src:0N!flip`sym`time`price`size!10?'(`3;.z.t;1000f;10000)
 sym time         price    size
 ------------------------------
 mil 10:30:32.148 470.7883 6360
 igf 00:28:17.727 634.6716 7885
 kao 06:52:34.397 967.2398 4503
 baf 10:07:47.382 230.6385 4204
 kfh 00:45:40.134 949.975  6210
 jec 05:12:49.761 439.081  8740
 kfm 16:31:50.104 575.9051 8732
 lkk 04:54:11.685 591.9004 4756
 kfi 13:01:04.698 848.1567 3998
 fgl 05:18:45.828 389.056  9342

q).Q.ff[src] enlist `sym`ratioA`ratioB!3#1
 sym time         price    size ratioA ratioB
 --------------------------------------------
 mil 10:30:32.148 470.7883 6360
 igf 00:28:17.727 634.6716 7885
 kao 06:52:34.397 967.2398 4503
 baf 10:07:47.382 230.6385 4204
 kfh 00:45:40.134 949.975  6210
 jec 05:12:49.761 439.081  8740
 kfm 16:31:50.104 575.9051 8732
 lkk 04:54:11.685 591.9004 4756
 kfi 13:01:04.698 848.1567 3998
 fgl 05:18:45.828 389.056  9342
```



## `fk` (foreign key)

```syntax
.Q.fk x
```

Where `x` is a table column, returns `` ` `` if the column is not a foreign key or `` `tab`` if the column is a foreign key into `tab`.

[](){#fmt-format}
## `fmt` (precision format)

```syntax
.Q.fmt[x;y;z]
```

Where

-   `x` and `y` are integer atoms
-   `z` is a numeric atom

returns `z` as a string of length `x`, formatted to `y` decimal places.

```q
q).Q.fmt[6;2]each 1 234
"  1.00"
"234.00"
```

To format the decimal data in a column to 2 decimal places, change it to string.

```q
q)fix:{.Q.fmt'[x+1+count each string floor y;x;y]}
q)fix[2]1.2 123 1.23445 -1234578.5522
"1.20"
"123.00"
"1.23"
"-1234578.55"
```

Also handy for columns:

```q
q)align:{neg[max count each x]$x}
q)align fix[2]1.2 123 1.23445 -1234578.5522
"       1.20"
"     123.00"
"       1.23"
"-1234578.55"
```

Example: persist a table with float values to file as character strings of length 9, e.g. 34.3 to 
```q
"     34.3"
```
Keep as much precision as possible, i.e. persist 343434.3576 as `"343434.36"`.

```q
q)fmt:{.Q.fmt[x;(count 2_string y-i)&x-1+count string i:"i"$y]y}
q)fmt[9] each 34.4 343434.358
"     34.4"
"343434.36"
```

:fontawesome-regular-hand-point-right:
[`.Q.f`](#f-precision-format) (precision format), [-27!(x;y)](../basics/internal.md#-27xy-ieee754-precision-format) (IEEE754 precision format)
<br>:fontawesome-solid-book-open:
[`\P`](../basics/syscmds.md#p-precision) (precision)

[](){#fpn-streaming-algorithm}
## `fpn` (pipe streaming)
[](){#fps-streaming-algorithm}
## `fps` (pipe streaming)

_[`.Q.fs`](#fs-file-streaming) for pipes_

```syntax
.Q.fps[x;y]
.Q.fpn[x;y;z]
```

Where

-   `x` is a unary function
-   `y` is a filepath to a fifo (named pipe)
-   `z` is an integer

(Since V3.4)

Reads `z`-sized lumps of complete `"\n"` delimited records from a pipe and applies a function to each record. This enables you to implement a streaming algorithm for various purposes such as converting a large compressed CSV file into an on-disk kdb+ database without holding the data in memory all at once or using disk space required for the uncompressed file.

:fontawesome-solid-graduation-cap:
[Streaming data from named pipes](../kb/named-pipes.md#streaming)

!!! tip "`.Q.fps` is a projection of `.Q.fpn` with the chunk size set to 131000 bytes."

[](){#fs-streaming-algorithm}
## `fs` (file streaming)
[](){#fsn-streaming-algorithm}
## `fsn` (file streaming)

```syntax
.Q.fs[x;y]
.Q.fsn[x;y;z]
```

Where

-   `x` is a unary function
-   `y` is a filepath
-   `z` is an integer

loops over file `y`, grabs `z`-sized lumps of complete `"\n"` delimited records, applies `x` to each record, and returns the size of the file as given by [`hcount`](hcount.md). This enables you to implement a streaming algorithm for various purposes such as converting a large CSV file into an on-disk kdb+ database without holding the data in memory all at once.

`.Q.fsn` is almost identical to `.Q.fs` but takes an extra argument `z`, the size in bytes that chunks will be read in. This is particularly useful for balancing load speed and RAM usage.

!!! tip "`.Q.fs` is a projection of `.Q.fsn` with the chunk size set to 131000 bytes."

For example, assume that the file `potamus.csv` contains the following:

```csv
Take, a,   hippo, to,   lunch, today,        -1, 1941-12-07
A,    man, a,     plan, a,     hippopotamus, 42, 1952-02-23
```

If you call `.Q.fs` on this file with the function `0N!`, you get the following list of rows:

```q
q).Q.fs[0N!]`:potamus.csv
("Take, a,   hippo, to,   lunch, today,        -1, 1941-12-07";"A,    man, a,..
120
```

`.Q.fs` can also be used to read the contents of the file into a list of columns.

```q
q).Q.fs[{0N!("SSSSSSID";",")0:x}]`:potamus.csv
(`Take`A;`a`man;`hippo`a;`to`plan;`lunch`a;`today`hippopotamus;-1 42i;1941.12..
120
```

:fontawesome-solid-graduation-cap:
[Loading from large files](../kb/loading-from-large-files.md)


## `ft` (apply simple)

```syntax
.Q.ft[x;y]
```

Where

-   `y` is a keyed table
-   `x` is a unary function `x[t]` in which `t` is a simple table

returns a table with at least as many key columns as `t`.

As an example, note that you can index into a simple table with row indices, but not into a keyed table – for that you should use a select statement. To illustrate the method, the following example shows an indexing function being applied to a keyed table named `sp` (script [`sp.q`](https://raw.githubusercontent.com/KxSystems/kdb/master/sp.q) is used to populate the table).

```q
q)\l sp.q

q)sp 2 3           / index simple table with integer list argument
s  p  qty
---------
s1 p3 400
s1 p4 200

q)s 2 3            / index keyed table fails
'length
```

Now create an indexing function, and wrap it in `.Q.ft`.
This works on both types of table:

```q
q).Q.ft[{x 2 3};s]
s | name  status city
--| -------------------
s3| blake 30     paris
s4| clark 20     london
```

Equivalent select statement:

```q
q)select from s where i in 2 3
s | name  status city
--| -------------------
s3| blake 30     paris
s4| clark 20     london
```


## `fu` (apply unique)

```syntax
.Q.fu[x;y]
```

Where `x` is a unary function and `y` is

-   a list, returns `x[y]` after evaluating `x` only on distinct items of `y`
-   not a list, returns `x[y]`

```q
q)vec:100000 ? 30     / long vector with few different values
q)f:{exp x*x}         / e raised to x*x
q)\t:1000 r1:f vec
745
q)\t:1000 r2:.Q.fu[f;vec]
271
q)r1~r2
1b
```

!!! warning "Not suitable for all unary functions"

    `.Q.fu` applies `x` to the distinct items of `y`.
    Where for any index `i`, the result of `x y i` depends on no other item of `y`, then `.Q.fu` works as intended. Where this is not so, the result is unlikely to be expected or useful.

    To explore this, study `.Q.fu[avg;] (4 3#12?100)10?4`.


## `gc` (garbage collect)

```syntax
.Q.gc[]
```

Run garbage-collection and returns the amount of memory that was returned to the OS.
<!-- (Since V2.7 2010.08.05, enhanced with coalesce in V2.7 2011.09.15, and executes in secondary threads since V2.7 2011.09.21) -->
It attempts to coalesce pieces of the heap into their original allocation units and returns any units ≥64MB to the OS.
Refer to [`\g`](../basics/syscmds.md#g-garbage-collection-mode) (garbage collection mode) for details on how memory is created on the heap.

When secondary threads are configured and `.Q.gc[]` is invoked in the main thread, `.Q.gc[]` is automatically invoked in each secondary thread.
If the call is instigated in a secondary thread, it affects that thread’s local heap only.

Example of garbage collection in the default `deferred` mode, using [`.Q.w[]`](#w-memory-stats) to view memory stats:
```q
q)a:til 10000000      / create an object that is ≥64MB
q).Q.w[]              / view current heap size and how many bytes used of the heap (all objects plus previously allocated object)
used| 134589136
heap| 201326592
peak| 201326592
wmax| 0
mmap| 0
mphy| 17179869184
syms| 689
symw| 37406
q).Q.gc[]             / garbage collection doesnt return any memory to OS
0
q)delete a from `.    / delete the original object, placing it on the heap
`.
q).Q.w[]              / used memory has decreased, heap remains the same
used| 371376
heap| 201326592
peak| 201326592
wmax| 0
mmap| 0
mphy| 17179869184
syms| 690
symw| 37436
q).Q.gc[]             / garbage collection has returned 134217728 to the OS from the heap
134217728
q).Q.w[]              / heap size has reduced, while used memory remains the same
used| 371376
heap| 67108864
peak| 201326592
wmax| 0
mmap| 0
mphy| 17179869184
syms| 690
symw| 37436
```

Depending on your data, memory can become fragmented and therefore difficult to release back to the OS. The following demonstrates an example:

```q
q).Q.w[]              / initial memory stats
used| 371360
heap| 67108864
peak| 67108864
wmax| 0
mmap| 0
mphy| 17179869184
syms| 689
symw| 37406
q)v:{(10#"a";10000#"b")}each til 1000000;   / create 1000000 rows, each containing 2 elements of 10 chars and 10000 chars
q).Q.w[]                                    / both heap and used memory has grown
used| 16456760016
heap| 16508780544
peak| 16508780544
wmax| 0
mmap| 0
mphy| 17179869184
syms| 689
symw| 37406
q).Q.gc[]             / garbage collection has found no slab of contiguous unused memory of ≥64MB to free
0
q)v:v[;0]             / change v to 1000000 rows, each only containing the 1st element of 10 chars (2nd element removed)
q).Q.w[]              / used memory has decreased, heap remains the same
used| 40760016
heap| 16508780544
peak| 16508780544
wmax| 0
mmap| 0
mphy| 17179869184
syms| 690
symw| 37436
q).Q.gc[]             / garbage collection has found no contiguous unused memory of ≥64MB to free
0
q)v:-8!v              / convert v into its serialised form, return vector used by v to heap
q).Q.gc[]             / garbage collection now found unused contiguous memory slab ≥64MB to return to OS
16374562816
q)v:-9!v              / convert serialised form of v back to its original state
q).Q.w[]              / used memory remains the same as before, but heap has reduced
used| 40760016
heap| 134217728
peak| 16508780544
wmax| 0
mmap| 0
mphy| 17179869184
syms| 690
symw| 37436
```

If you have nested data, e.g. columns of char vectors, or much grouping, you may be fragmenting memory.

Since V3.3 2015.08.23 (Linux only) unused pages in the heap are dropped from RSS during `.Q.gc[]`.

Since 4.1t 2022.07.01, `.Q.gc[0]` can be used to perform a subset of operations performed by `.Q.gc[]` (i.e. only return unused blocks >= 64MB to os). 
This has the advantage of running return faster than `.Q.gc[]`, but with the disadvantage of not defragmenting unused memory blocks of a smaller size (therefore may not free as much unused memory).

:fontawesome-solid-hand-point-right:
[`.Q.w`](#w-memory-stats) (memory stats), 
[`\g`](../basics/syscmds.md#g-garbage-collection-mode) (garbage collection mode), 
[`\w`](../basics/syscmds.md#w-workspace) (workspace)


## `gz` (GZip)

```syntax
.Q.gz[::]           / zlib loaded?
.Q.gz cbv           / unzipped
.Q.gz (cl;cbv)      / zipped
```

Where

-   `cbv` is a char vector (or byte vector since 4.1t 2021.09.03,4.0 2021.10.01)
-   `cl` is compression level \[1-9\] as a long

returns, for

-   the [general null](identity.md#null), a boolean atom as whether Zlib is loaded
-   `cbv`, the inflated (unzipped) vector
-   a 2-list, the deflated (zipped) vector

since V4.0 2020.04.16.

```q
q).Q.gz{0N!count x;x}[.Q.gz(9;10000#"helloworld")]
66
"helloworldhelloworldhelloworldhelloworldhelloworldhelloworldhelloworldhellow..
```

:fontawesome-solid-hand-point-right:
[-18!x](../basics/internal.md#-18x-compress-bytes) (ipc compress bytes)


## `hdpf` (save tables)

```syntax
.Q.hdpf[historicalport;directory;partition;`p#field]
```

The function:

* saves all tables to disk, by calling [`.Q.dpft`](#dpft-save-table) (saves as splayed tables to a partition)
* clears in-memory tables
* sends reload message to HDB, by opening a temporary connection and sending [`\l .`](../basics/syscmds.md#l-load-file-or-directory)


## `hg` (HTTP get)

```syntax
.Q.hg x
```

Where `x` is a URL as a symbol atom or (since V3.6 2018.02.10) a string, returns a string for the result of an HTTP[S] GET query.
(Since V3.4)

```q
q).Q.hg`:http://www.google.com
q)count a:.Q.hg`:http:///www.google.com
212
q)show a
"<!DOCTYPE HTML PUBLIC \"-//IETF//DTD HTML 2.0//EN\">\n<html><head>\n<title>4..
q).Q.hg ":http://username:password@www.google.com"
```

If you have configured SSL/TLS, HTTPS can also be used.

```q
q).Q.hg ":https://www.google.com"
```

`.Q.hg` will utilize proxy settings from the environment, lower-case versions taking precedence:

environment variable       | use
---------------------------|----
`http_proxy`, `HTTP_PROXY` | The URL of the HTTP proxy to use
`no_proxy`, `NO_PROXY`     | Comma-separated list of domains for which to disable use of proxy

N.B. HTTPS is not supported across proxies which require `CONNECT`.

Since 4.0 2019.10.22, gzip compression is supported. Requests include the HTTP header "Accept-Encoding: gzip". 
The server then decides whether to gzip the returned payload, which is uncompressed prior to .Q.hg returning.

:fontawesome-solid-graduation-cap:[HTTP](../kb/http.md)

[](){#host-hostname}
## `host` (IP to hostname)

```syntax
.Q.host x
```

Where `x` is an IP address as an int atom, returns its hostname as a symbol atom.

```q
q).Q.host .Q.addr`localhost
`localhost
q).Q.addr`localhost
2130706433i

q)"I"$"104.130.139.23"
1753385751i
q).Q.host "I"$"104.130.139.23"
`netbox.com
q).Q.addr `netbox.com
1753385751i
```

:fontawesome-regular-hand-point-right:
[`.Q.addr`](#addr-iphost-as-int) (IP/host as int), [`$`](tok.md#ip-address) tok (IP address as int)


## `hp` (HTTP post)

```syntax
.Q.hp[x;y;z]
```

Where

-   `x` is a URL as a symbol handle or string (since V3.6 2018.02.10)
-   `y` is a MIME type as a string
-   `z` is the POST query as a string

Returns a string for the result of an HTTP[S] POST query.
(Since V3.4)

Uses proxy settings (if defined) and compression handling, as described in [hg (HTTP get)](#hg-http-get).

```q
q).Q.hp["http://google.com";.h.ty`json]"my question"
"<!DOCTYPE html>\n<html lang=en>\n  <meta charset=utf-8>\n  <meta name=viewpo..
```

:fontawesome-solid-graduation-cap:[HTTP](../kb/http.md)


## `id` (sanitize)

```syntax
.Q.id x
```

Where `x` is

- a **symbol atom**, returns `x` with items sanitized to valid q names

    ```q
    q).Q.id each `$("ab";"a/b";"two words";"2drifters";"2+2")
    `ab`ab`twowords`a2drifters`a22
    ```

- a **table**, returns `x` with column names sanitized by removing characters that interfere with `select/exec/update` and adding `"1"` to column names which clash with commands in the `.q` namespace. Updated in V3.2 to include [`.Q.res`](#res-keywords) for checking collisions.

    ```q
    q).Q.id flip (5#.Q.res)!(5#())
    in1 within1 like1 bin1 binr1
    ----------------------------
    q).Q.id flip(`$("a";"a/b"))!2#()
    a ab
    ----
    ```

- a **dictionary** (since v4.1 2024.09.13), supports the same rules as `table` above

    ```q
    q).Q.id (5#.Q.res)!(5#())
    abs1 | 
    acos1| 
    asin1| 
    atan1| 
    avg1 | 
    ```

Since 4.1t 2022.03.25,4.0 2022.10.26 produces a symbol `a` when the input contains a single character that is not in [.Q.an](#an-all-alphanumerics) (it previously produced an empty sym) e.g.

```q
q).Q.id`$"+"
a  / previous version returned `
```

Table processing also has additional logic to cater for duplicate column names (names are now appended with 1,2,etc. when matched against previous columns) after applying previously defined rules e.g.

```q
q)cols .Q.id(`$("count+";"count*";"count1"))xcol([]1 2;3 4;5 6)
`count1`count11`count12  / previous version returned `count1`count1`count1
q)cols .Q.id(`$("aa";"=";"+"))xcol([]1 2;3 4;5 6)
`aa`a`a1                / previous version returned `aa`1`1
```

Since 4.1t 2022.11.01,4.0 2022.10.26, the same rule is applied when the provided name begins with either an underscore or a numerical character. Previously, it could produce an invalid column name.

```q
q).Q.id`$"_"
`a_
q)cols .Q.id(`$("3aa";"_aa";"_aa"))xcol([]1 2;3 4;5 6)
`a3aa`a_aa`a_aa1
```


## `ind` (partitioned index)

```syntax
.Q.ind[x;y]
```

Where

-   `x` is a partitioned table
-   `y` is a **long** int vector of row indexes into `x`

returns rows `y` from `x`.

When picking individual records from an in-memory table you can simply use the special virtual field `i`:

```q
select from table where i<100
```

But you cannot do that directly for a partitioned table.

`.Q.ind` comes to the rescue here, it takes a table and indexes into the table – and returns the appropriate rows.

```q
.Q.ind[trade;2 3]
```

A more elaborate example that selects all the rows from a date:

```q
q)t:select count i by date from trade
q)count .Q.ind[trade;(exec first sum x from t where date<2010.01.07)+til first exec x from t where date=2010.01.07]
28160313
/ show that this matches the full select for that date
q)(select from trade where date=2010.01.07)~.Q.ind[trade;(exec first sum x from t where date<2010.01.07)+til first exec x from t where date=2010.01.07]
1b
```

!!! tip "Continuous row intervals"

    If you are selecting a continuous row interval, for example if iterating over all rows in a partition, instead of using `.Q.ind` you might as well use

    ```q
    q)select from trade where date=2010.01.07,i within(start;start+chunkSize)
    ````


## `j10` (encode binhex)
## `x10` (decode binhex)
## `j12` (encode base-36)
## `x12` (decode base-36)

```syntax
.Q.j10 s     .Q.j12 s
.Q.x10 s     .Q.x12 s
```

Where `s` is a string, these functions return `s` encoded (`j10`, `j12`) or decoded (`x10`, `x12`) against restricted alphabets:

-   `…10` en/decodes against the alphabet `.Q.b6`, this is a base-64 encoding - see [BinHex](https://en.wikipedia.org/wiki/BinHex) and [Base64](https://en.wikipedia.org/wiki/Base64) for more details than you ever want to know about which characters are where in the encoding. To keep the resulting number an integer the maximum length of `s` is 10.
-   `-12` en/decodes against `.Q.nA`, a base-36 encoding. As the alphabet is smaller `s` can be longer – maximum length 12.

The main use of these functions is to encode long alphanumeric identifiers (CUSIP, ORDERID..) so they can be quickly searched – but without filling up the symbol table with vast numbers of single-use values.

```q
q).Q.x10 12345
"AAAAAAADA5"
q).Q.j10 .Q.x10 12345
12345
q).Q.j10 each .Q.x10 each 12345+1 2 3
12346 12347 12348
q).Q.x12 12345
"0000000009IX"
q).Q.j12 .Q.x12 12345
12345
```

!!! tip

    If you don’t need the default alphabets it can be very convenient to change them to have a blank as the first character, allowing the identity `0` <-> `" "`.

    If the values are not going to be searched (or will be searched with `like`) then keeping them as nested character is probably going to be simpler.



## `K` (version date)
## `k` (version)

```syntax
.Q.K      / version date
.Q.k      / version
```

Return the interpreter version date (`.Q.K`) and number (`.Q.k`) for which `q.k` has been written:
checked against [`.z.K`](dotz.md#zk-version) at startup.

```q
q).Q.K
2020.10.02
q).Q.k
4f
```


## `l` (load)

```syntax
.Q.l x
```

Where `x` is a hsym or symbol atom naming a directory in the current directory, loads
it recursively as in [`load`](load.md), but into the default namespace.

(Implements system command [`\l`](../basics/syscmds.md#l-load-file-or-directory).)


## `ld` (load and group)

```syntax
.Q.ld x
```

Exposes logic used by [`\l`](../basics/syscmds.md#l-load-file-or-directory) to group script lines for evaluation.
Since 4.1t 2022.11.01,4.0 2023.03.28.

```q
q).Q.ld read0`:funcs.q
1                   2                5                    6
"/ multi line func" "f:{\n  x+y\n }" "/ single line func" "g:{x*y}"
```

## `li` (load partitions)

```syntax
.Q.li[partitions]
```

In the current hdb, adds any partition(s) which are both in the list supplied and on disk. Partitions can be a list or atomic variable. For example:

```q
q)`:/tmp/db/2001.01.01/t/ set tt:.Q.en[`:/tmp/db]([]sym:10?`A`B`C;time:10?.z.T;price:10?10f)
q)\l /tmp/db
q)`:2001.01.02/t/`:2001.01.03/t/ set\:tt
q)date
,2001.01.01
q).Q.li[2001.01.02];date
2001.01.01 2001.01.02
q).Q.li[2001.01.02 2001.01.03];select count i by date from t
date      | x
----------| --
2001.01.01| 10
2001.01.02| 10
2001.01.03| 10
```

Since v4.1 2024.09.20.


## `lo` (load without)

```syntax
.Q.lo[`:database;cd;scripts]
```

Where

-   `database` is a hsym or symbol atom (as per parameter to [.Q.l](#l-load))
-   `cd` is a boolean flag indicating whether to cd to the database dir
-   `scripts` is a boolean flag indicating whether to execute any scripts in the database dir

Load a database without changing directory and/or loading scripts in the database (since 4.1t 2023.03.01).

```q
q)\cd
"/tmp"
q)key`:db/2023.02.01
`s#,`trade
q).Q.lo[`:db;0;0]
q)trade
date       sym time         price
------------------------------------
2023.02.01 C   10:15:18.957 6.346716
2023.02.01 B   10:15:18.958 9.672398
2023.02.01 C   10:15:18.959 2.306385
2023.02.01 B   10:15:18.960 9.49975
2023.02.01 A   10:15:18.961 4.39081
q)\cd
"/tmp"
```


## `M` (chunk size)

```syntax
.Q.M
```

Chunk size for [`dsftg`](#dsftg-load-process-save) (load-process-save).


```q
q)0W~.Q.M  / defaults to long infinity
1b
```


## `MAP` (maps partitions)

```syntax
.Q.MAP[]
```

Keeps partitions mapped to avoid the overhead of repeated file system calls during a `select`.
(Since V3.1.)

For use with partitioned HDBS, used in tandem with [`\l dir`](../basics/syscmds.md#l-load-file-or-directory)

```q
q)\l .
q).Q.MAP[]
```

<!-- 
When using `.Q.MAP[]` you can’t access the date column outside of the usual:

```q
select … [by date,…] from … where [date …]
```
 -->

.Q.MAP currently has the following limitations:

- .Q.MAP does not work with linked columns

- .Q.MAP does not work with virtual partition columns

- Use of .Q.MAP with compressed files is not recommended, as the uncompressed maps will be retained in memory

!!! detail "You may need to increase the number of available file handles, and also the number of available file maps (for Linux see `vm.max_map_count`)"

Since 4.1t 2024.01.11 parallelized over tables and partitions with [peach](each.md) when kdb+ running with secondary threads.

## `n` (nums)
## `nA` (alphanums)

```syntax
.Q.n
.Q.nA
```

Strings: numerics (`.Q.n`) and upper-case alphabet and numerics (`.Q.nA`).

```q
q).Q.n
"0123456789"
q).Q.nA
"0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
```

`.Q.nA` is used for [base-36](#j12-encode-base-36) encoding and decoding.


## `opt` (command parameters)

```syntax
.Q.opt .z.x
```

Presents command-line arguments as a dictionary, using the output of [`.z.x`](dotz.md#zx-argv). Defaults can be added using [`.Q.def`](#def-command-defaults).

```bash
$ q -param1 val1 -param2 val2
```
```q
q)params:.Q.opt .z.x
q)show params
param1| "val1"
param2| "val2"
q)params`param1
"val1"
```
Example of a command-line parameter with no value and a parameter with multiple values:
```bash
$ q -param1 -param2 as asd -param3
```
```q
q).Q.opt .z.x
param1| ()
param2| ("as";"asd")
param3| ()
```

:fontawesome-solid-hand-point-right:
[`.z.x`](dotz.md#zx-argv) (argv), [`.z.X`](dotz.md#zx-raw-command-line) (raw command line), [`.z.f`](dotz.md#zf-file) (file), [`.z.q`](dotz.md#zq-quiet-mode) (quiet mode), [`.Q.def`](#def-command-defaults) (command defaults), [`.Q.x`](#x-non-command-parameters) (non-command parameters)


## `P` (segments)

```syntax
.Q.P
```

In segmented DBs, returns a list of the segments (i.e. the contents of `par.txt`).

```q
q).Q.P
`:../segments/1`:../segments/2`:../segments/3`:../segments/4
```


## `par` (get expected partition location)

```syntax
.Q.par[dir;part;table]
```

Where

-   `dir` is a directory filepath
-   `part` is a date

returns the expected location of `table`. (Sensitive to `par.txt`.)

```q
q).Q.par[`:.;2010.02.02;`quote]
`:/data/taq/2010.02.02/quote
```

Can assist in checking `` `p`` attribute is present on all partitions of a table in an HDB

```q
q)all{`p=attr .Q.par[`:.;x;`quote]`sym}each  date
1b
```

!!! warning "Does not look into the segment directories."

    The function calculates only the path, based on the partition and the contents of `par.txt` in a round-robin fashion. It does not check the contents of the segments to see if the partition is there. See [Segmented databases](../database/segment.md#considerations) for details.

## `PD` (partition locations)

```syntax
.Q.PD
```

In partitioned DBs, a list of partition locations – conformant to [`.Q.PV`](#pv-partition-values) – which represents the partition location for each partition.
(In non-segmented DBs, this will be simply ``count[.Q.PV]#`:.``.)
`.Q.PV!.Q.PD` can be used to create a dictionary of partition-to-location information.

```q
q).Q.PV
2010.05.26 2010.05.27 2010.05.28 2010.05.29 2010.05.30 2010.05.30 2010.05.31
q).Q.PD
`:../segments/1`:../segments/2`:../segments/3`:../segments/4`:../segments/3`:../segments/4`:../segments/1
q).Q.PV!.Q.PD
2010.05.26| :../segments/1
2010.05.27| :../segments/2
2010.05.28| :../segments/3
2010.05.29| :../segments/4
2010.05.30| :../segments/3
2010.05.30| :../segments/4
2010.05.31| :../segments/1
```


## `pd` (modified partition locations)

```syntax
.Q.pd
```

In partitioned DBs, [`.Q.PD`](#pd-partition-locations) as modified by [`.Q.view`](#view-subview).


## `pf` (partition field)

```syntax
.Q.pf
```

In partitioned DBs, the partition field.
Possible values are `` `date`month`year`int``.


## `pn` (partition counts)

```syntax
.Q.pn
```

In partitioned DBs, returns a dictionary of cached partition counts – conformant to [`.Q.pt`](#pt-partitioned-tables), each conformant to [`.Q.pv`](#pv-modified-partition-values) – as populated by [`.Q.cn`](#cn-count-partitioned-table).

Cleared by [`.Q.view`](#view-subview).

`.Q.pv!flip .Q.pn` can be used to create a crosstab of table-to-partition-counts once `.Q.pn` is fully populated.

```q
q)n:100
q)t:([]time:.z.T+til n;sym:n?`2;num:n)
q).Q.dpft[`:.;;`sym;`t]each 2010.01.01+til 5
`t`t`t`t`t
q)\l .
q).Q.pn
t|
q).Q.cn t
100 100 100 100 100
q).Q.pn
t| 100 100 100 100 100
q).Q.pv!flip .Q.pn
          | t
----------| ---
2010.01.01| 100
2010.01.02| 100
2010.01.03| 100
2010.01.04| 100
2010.01.05| 100
q).Q.view 2#date
q).Q.pn
t|
q).Q.cn t
100 100
q).Q.pn
t| 100 100
q).Q.pv!flip .Q.pn
          | t
----------| ---
2010.01.01| 100
2010.01.02| 100
```

:fontawesome-solid-graduation-cap:
[Table counts](../kb/partition.md#table-counts)


## `prf0` (code profiler)

```syntax
.Q.prf0 pid
```

Where `pid` is a process ID, returns a table representing a snapshot of the call stack at the time of the call in another kdb+ process `pid`, with columns

```txt
name   assigned name of the function
file   path to the file containing the definition
line   line number of the definition
col    column offset of the definition, 0-based
text   function definition or source string
pos    execution position (caret) within text
```

This process must be started from the same binary as the one running `.Q.prf0`, otherwise `binary mismatch` is signalled.

Since 4.1t 2022.03.25, .Q.prf0 will not try to stop the process if passed a negative `pid`. 
This should be used when a kdb+ process is already stopped under control of something other than `.Q.prf0` (for example, in a debugger or a native-code profiler).
A negative `pid` should not be used in a running process.

:fontawesome-solid-graduation-cap:
[Code profiler](../kb/profiler.md)



## `pt` (partitioned tables)

```syntax
.Q.pt
```

Returns a list of partitioned tables.

## `pv` (modified partition values)

```syntax
.Q.pv
```

A list of the values of the partition domain: the values corresponding to the slice directories actually found in the root.

In partitioned DBs, [`.Q.PV`](#pv-partition-values) as modified by [`.Q.view`](#view-subview).

:fontawesome-solid-street-view:
_Q for Mortals_
[§14.5.3 `.Q.pv`](/q4m3/14_Introduction_to_Kdb%2B/#1453-qpv)


## `PV` (partition values)

```syntax
.Q.PV
```

In partitioned DBs, returns a list of partition values – conformant to [`.Q.PD`](#pd-partition-locations) – which represents the partition value for each partition.
(In a date-partitioned DB, unless the date has been modified by [`.Q.view`](#view-subview), this is simply date.)

```q
q).Q.PD
`:../segments/1`:../segments/2`:../segments/3`:../segments/4`:../segments/3`:../segments/4`:../segments/1
q).Q.PV
2010.05.26 2010.05.27 2010.05.28 2010.05.29 2010.05.30 2010.05.30 2010.05.31
q)date
2010.05.26 2010.05.27 2010.05.28 2010.05.29 2010.05.30 2010.05.30 2010.05.31
q).Q.view 2010.05.28 2010.05.29 2010.05.30
q)date
2010.05.28 2010.05.29 2010.05.30 2010.05.30
q).Q.PV
2010.05.26 2010.05.27 2010.05.28 2010.05.29 2010.05.30 2010.05.30 2010.05.31
```


## `qp` (is partitioned)

```syntax
.Q.qp x
```

Where `x`

-   is a partitioned table, returns `1b`
-   a splayed table, returns `0b`
-   anything else, returns 0

```q
q)\
  B
+`time`sym`price`size!`B
  C
+`sym`name!`:C/
  \
q).Q.qp B
1b
q).Q.qp select from B
0
q).Q.qp C
0b
```


## `qt` (is table)

```syntax
.Q.qt x
```

Where `x` is a table, returns `1b`, else `0b`.


## `res` (keywords)

```syntax
.Q.res
```

Returns the control words and keywords as a symbol vector. ``key `.q`` returns the functions defined to extend k to the q language. Hence to get the full list of reserved words for the current version:

```q
q).Q.res,key`.q
`abs`acos`asin`atan`avg`bin`binr`cor`cos`cov`delete`dev`div`do`enlist`exec`ex..
```

:fontawesome-regular-hand-point-right:
[`.Q.id`](#id-sanitize) (sanitize)


## `s` (plain text)

```syntax
.Q.s x
```

Returns `x` formatted to plain text, as used by the console. Obeys console width and height set by [`\c`](../basics/syscmds.md#c-console-size).

```q
q).Q.s ([h:1 2 3] m: 4 5 6)
"h| m\n-| -\n1| 4\n2| 5\n3| 6\n"
```

Occasionally useful for undoing _Studio for kdb+_ tabular formatting.


## `s1` (string representation)

```syntax
.Q.s1 x
```

Returns a string representation of `x`.

:fontawesome-solid-book:
[`show`](show.md),
[`string`](string.md)


## `sbt` (string backtrace)

```syntax
.Q.sbt x
```

Where `x` is a [backtrace object](#trp-extend-trap-at) returns it as a string formatted for display.

Since V3.5 2017.03.15.

:fontawesome-solid-book-open:
[Debugging](../basics/debug.md)


## `sha1` (SHA-1 encode)

```syntax
.Q.sha1 x
```

Where `x` is a string, returns as a bytestream its SHA-1 hash.

```q
q).Q.sha1"Hello World!"
0x2ef7bde608ce5404e97d5f042f95f89f1c232871
```

Since V3.6 2018.05.18.


## `t` (type letters)

```syntax
.Q.t
```

List of chars indexed by datatype numbers.

```q
q).Q.t
" bg xhijefcspmdznuvts"
q).Q.t?"j"  / longs have datatype 7
7
```



## `trp` (extend trap at)

```syntax
.Q.trp[f;x;g]
```

Where

-   `f` is a unary function
-   `x` is its argument
-   `g` is a binary function

extends [Trap At](apply.md#trap-at) (`@[f;x;g]`) to collect backtrace: `g` gets called with arguments:

1.   the error string
2.   the backtrace object

You can format the backtrace object with [`.Q.sbt`](#sbt-string-backtrace).

```q
q)f:{`hello+x}
q)           / print the formatted backtrace and error string to stderr
q).Q.trp[f;2;{2"error: ",x,"\nbacktrace:\n",.Q.sbt y;-1}]
error: type
backtrace:
  [2]  f:{`hello+x}
                ^
  [1]  (.Q.trp)

  [0]  .Q.trp[f;2;{2"error: ",x,"\nbacktrace:\n",.Q.sbt y;-1}]
       ^
-1
q)
```

`.Q.trp` can be used for remote debugging.

```q
q)h:hopen`::5001   / f is defined on the remote
q)h"f `a"
'type              / q's IPC protocol can only get the error string back
  [0]  h"f `a"
       ^
q)                 / a made up protocol: (0;result) or (1;backtrace string)
q)h".z.pg:{.Q.trp[(0;)@value@;x;{(1;.Q.sbt y)}]}"
q)h"f 3"
0                  / result
,9 9 9
q)h"f `a"
1                  / failure
"  [4]  f@:{x*y}\n            ^\n  [3..
q)1@(h"f `a")1;    / output the backtrace string to stdout
  [4]  f@:{x*y}
            ^
  [3]  f:{{x*y}[x;3#x]}
          ^
  [2]  f `a
       ^
  [1]  (.Q.trp)

  [0]  .z.pg:{.Q.trp[(0;)@enlist value@;x;{(1;.Q.sbt y)}]}
              ^
```

Since V3.5 2017.03.15.

:fontawesome-solid-book-open:
[Debugging](../basics/debug.md)


## `trpd` (extend trap)

```syntax
.Q.trpd[f;x;g]
```

Where

-   `f` is a function of rank
-   `x` is an atom or list of count with items in the domains of f

-   `g` is a binary function

extends [Trap](apply.md#trap) (`.[f;x;g]`) to collect backtrace: `g` is called with arguments:

1.   the error string
2.   the backtrace object

You can format the backtrace object with [`.Q.sbt`](#sbt-string-backtrace).

```q
q).Q.trpd[{x+y};(1;2);{2"error: ",x,"\nbacktrace:\n",.Q.sbt y;-1}]
 3
q).Q.trpd[{x+y};(1;`2);{2"error: ",x,"\nbacktrace:\n",.Q.sbt y;-1}]
 error: type
 backtrace:
   [2]  {x+y}
          ^
   [1]  (.Q.trpd)

   [0]  .Q.trpd[{x+y};(1;`2);{2"error: ",x,"\nbacktrace:\n",.Q.sbt y;-1}]
        ^
 -1
```

Use .Q.trp as a simpler form of .Q.trpd, for unary values.

Since 4.1 2024.03.12.

:fontawesome-solid-book-open:
[Debugging](../basics/debug.md)


## `ts` (time and space)

_Apply, with time and space_

```syntax
.Q.ts[x;y]
```

Where `x` and `y` are valid arguments to [Apply](apply.md) returns a 2-item list:

1.  time and space as [`\ts`](../basics/syscmds.md#ts-time-and-space) would
2.  the result of `.[x;y]`

```q
q)\ts .Q.hg `:http://www.google.com
148 131760
q).Q.ts[.Q.hg;enlist`:http://www.google.com]
148 131760
"<!doctype html><html itemscope=\"\" itemtype=\"http://schema.org/WebPa

q).Q.ts[+;2 3]
0 80
5
```

Since V3.6 2018.05.18.


## `ty` (type)

```syntax
.Q.ty x
```

Where `x` is a list, returns the [type](../basics/datatypes.md) of `x` as a character code:

-   lower case for a vector
-   upper case for a list of uniform type
-   else blank

```q
q)t:([]a:3 4 5;b:"abc";c:(3;"xy";`ab);d:3 2#3 4 5;e:("abc";"de";"fg"))
q)t
a b c    d   e
------------------
3 a 3    3 4 "abc"
4 b "xy" 5 3 "de"
5 c `ab  4 5 "fg"
q).Q.ty each t`a`b`c`d`e
"jc JC"
```

!!! tip "`.Q.ty` is a helper function for `meta`"

    If the argument is a table column, returns upper case for mappable/uniform lists of vectors.

:fontawesome-solid-book:
[`meta`](meta.md)


## `u` (date based)

```syntax
.Q.u
```

-   In segmented DBs, returns `1b` if each partition is uniquely found in one segment. (E.g., true if segmenting is date-based, false if name-based.)
-   In partitioned DBs, returns `1b`.


## `V` (table to dict)

```syntax
.Q.V x
```

Where `x` is

-   a table, returns a dictionary of its column values.
-   a partitioned table, returns only the last partition (N.B. the partition field values themselves are not restricted to the last partition but include the whole range).

:fontawesome-solid-book:
[`meta`](meta.md)


## `v` (value)

```syntax
.Q.v x
```

Where `x` is

-   a filepath, returns the splayed table stored at `x`
-   any other symbol, returns the global named `x`
-   anything else, returns `x`


## `view` (subview)

```syntax
.Q.view x
```

Where `x` is a list of partition values that serves as a filter for all queries against any partitioned table in the database, `x` is added as a constraint in the first sub-phrase of the where-clause of every query.

`.Q.view` is handy when you are executing queries against partitioned or segmented tables. Recall that multiple tables can share the partitioning. `Q.view` can guard against runaway queries that ask for all historical data.

```q
.Q.view 2#date
```

Since 4.1t 2022.03.25,4.0 2023.05.26 this would signal an `invalid partition filter` error if partition value(s) resulted in no matches with [.Q.PV](#pv-partition-values).

`.Q.view`, also used when loading an hdb, now utilizes threads to load .d files (column names) since 4.1t 2023.04.17. 

:fontawesome-solid-hand-point-right:
_Q for Mortals_
[§14.5.8 `Q.view`](/q4m3/14_Introduction_to_Kdb+/#1458-qview)


## `vp` (missing partitions)

```syntax
.Q.vp
```

In partitioned DBs, returns a dictionary of table schemas for tables with missing partitions, as populated by [`.Q.bv`](#bv-build-vp).
(Since V3.0 2012.01.26.)

```q
q)n:100
q)t:([]time:.z.T+til n;sym:n?`2;num:n)
q).Q.dpft[`:.;;`sym;`t]each 2010.01.01+til 5
`t`t`t`t`t
q)tt:t
q).Q.dpft[`:.;;`sym;`tt]last 2010.01.01+til 5
`tt
q)\l .
q)tt
+`sym`time`num!`tt
q)@[get;"select from tt";-2@]; / error
./2010.01.01/tt/sym: No such file or directory
q).Q.bv[]
q).Q.vp
tt| +`date`sym`time`num!(`date$();`sym$();`time$();`long$())
q)@[get;"select from tt";-2@]; / no error
```


## `w` (memory stats)

```syntax
.Q.w[]
```

Returns the memory stats from [`\w`](../basics/syscmds.md#w-workspace) into a more readable dictionary. Refer to [`\w`](../basics/syscmds.md#w-workspace) for an explanation of each statistic.

```q
q).Q.w[]
used| 168304
heap| 67108864
peak| 67108864
wmax| 0
mmap| 0
mphy| 8589934592
syms| 577
symw| 25436
```

:fontawesome-solid-hand-point-right:
[`.Q.gc`](#gc-garbage-collect) (garbage collect)<br>
:fontawesome-solid-book-open:
[Command-line parameter `-w`](../basics/cmdline.md#-w-workspace) (workspace memory limit)
<br>
:fontawesome-solid-book-open:
[System command `\w`](../basics/syscmds.md#w-workspace) (memory stats and workspace memory limit)


## `Xf` (create file)

!!! warning "Deprecated"

    Deprecated since 4.1t 2022.03.25. Using resulting files could return file format errors since 3.6.

```syntax
.Q.Xf[x;y]
```

Where

-   `x` is a mapped nested datatype as either an upper-case char atom, or as a short symbol (e.g. `` `char``)
-   `y` is a filepath

creates an empty nested-vector file at `y`.

```q
q).Q.Xf["C";`:emptyNestedCharVector];
q)type get`:emptyNestedCharVector
87h
```


## `x` (non-command parameters)

```syntax
.Q.x
```

Set by [`.Q.opt`](#opt-command-parameters): a list of _non-command_ parameters from the command line, where _command parameters_ are prefixed by `-`.

```bash
$ q taq.k path/to/source path/to/destn
```
```q
q)cla:.Q.opt .z.X /command-line arguments
q).Q.x
"/Users/me/q/m64/q"
"path/to/source"
"path/to/destn"
```

:fontawesome-solid-hand-point-right:
[`.z.x`](dotz.md#zx-argv) (argv), [`.z.X`](dotz.md#zx-raw-command-line) (raw command line), [`.z.f`](dotz.md#zf-file) (file), [`.z.q`](dotz.md#zq-quiet-mode) (quiet mode), [`.Q.opt`](#opt-command-parameters) (command parameters), [`.Q.def`](#def-command-defaults) (command defaults)
