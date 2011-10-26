s3-parallel-put  Parallel uploads to Amazon AWS S3
==================================================

s3-parallel-put speeds the uploading of many small files to Amazon AWS S3 by
executing multiple PUTs in parallel.  It does not check for prior existence of
the file on S3 and therefore is only suitable as an initial data upload tool.


Dependencies
------------

* [Boto](http://code.google.com/p/boto/)


Usage
-----

```bash
s3-parallel-put --bucket=BUCKET --processes=PROCESSES SOURCE DESTINATION
```

This starts a parallel put from `SOURCE` to `BUCKET`/`DESTINATION` with
`PROCESSES` parallel processes.  Each PUT is printed *after* it has completed.

The program reads your credentials from the environment variables
`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

There are a few other options:

`--dry-run` causes the program to print what it would do, but not to connect
to S3 nor attempt to upload any files.  It is strongly recommended that you
test the program with this option before transferring any real data.

`--limit=N` causes the program to upload no more than N files.  Combined
with `--dry-run`, this is also useful for testing.

`--verbose` causes more output to be printed, including progress of individual files.

`--quiet` causes less output.

`--secure` and `--insecure` control whether a secure connection is used.


Architecture
------------

* A walker process generates (filename, key_name) pairs and inserts them in
  `put_queue`.
* Multiple putter processes consume these pairs in parallel, uploading the
  files to S3 and sending file-by-file statistics to `stat_queue`.
* A statter process consumes these file-by-file statistics and generates
  summary statistics.


Bugs
----

* Absolutely no error checking whatsoever.


To Do
-----

* Add some help text to `--help` option.

* Add option to test for key and value (MD5) before uploading.

* Automatically parallelize uploads of large files by splitting into chunks.


Licence
-------

Copyright (C) 2011  Tom Payne

This program is free software: you can redistribute it and/or modify it
under the terms of the GNU General Public License as published by the Free
Software Foundation, either version 3 of the License, or (at your option) any
later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with
this program.  If not, see <http://www.gnu.org/licenses/>.


vim: set spell spelllang=en textwidth=76:
