# The DaCapo Benchmark Suite

Last updated 2019-06-06

This benchmark suite is intend as a tool for the research community.
It consists of a set of open source, real world applications with
non-trivial memory loads.


## Guidelines for use

When quoting results in publications, the authors of this suite
strongly request that:

* The exact version of the suite be given (number & name)

* The suite be cited in accordance with the usual standards of acknowledging credit in academic research.

* Please cite the [2006 OOPSLA paper](http://doi.acm.org/10.1145/1167473.1167488)

* All command line options used be reported.  For example, if you explicitly override the number of threads or set the number of iterations, you must report this when you publish results. 

For more information see the [DaCapo Benchmark web page](http://dacapobench.org).


## Building

The easiest way to obtain the benchmark suite is to download the pre-built jar file from the DaCapo Benchmark web site above.

If, however, you want to build from source read on...

##### Dependencies:

The suite is built using ant 1.10.  You will need the following tools:

* *[ant 1.10](http://ant.apache.org)* You need to install this yourself if you don't already have it.

* *[javacc](http://javacc.dev.java.net/)* Included in our tools directory.

* *[maven](http://maven.apache.org)* Included in our tools directory.

* *[cvs](http:/www.nongnu.org/cvs)*

* *[svn](http://subversion.apache.org)*

* *[python](https://www.python.org/) (with following libraries)*

    * *[colorama](https://pypi.org/project/colorama/)*
    * *[future](https://pypi.org/project/future/)*
    * *[tabulate](https://pypi.org/project/tabulate/)*
    * *[requests](https://pypi.org/project/requests/)*
    * *[wheel](https://pypi.org/project/wheel/)*

* *[node.js](https://nodejs.org/en/)*

* *[npm](https://www.npmjs.com/get-npm)*

##### System requirement:

Building DaCapo requires latest JDK 11.

If building __cassandra__, __graphchi__, __jython__, and/or __xalan__,
make sure JDK 8 is also installed and
its path set in local.properties file.

Building the whole suite at once on macOS **may** have problem with max filehandle limits.
You may want to set it to a larger value, and launch ant with:
`export JAVA_OPTS="-XX:-MaxFDLimit"`

Set your JAVA_HOME environment variable appropriately:
On Mac OS X something like:
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.3.jdk/Contents/Home/
On Ubuntu 16.04 something like:
`export JAVA_HOME=/usr/lib/jvm/jdk1.11.0/`

Set __ant__ and __maven__ environment variables if necessary.  In particular,
for some jvms it is necessary to explicitly request a larger heap size.
It is necessary to set the maven options because the trade benchmarks
are built by maven (called by ant).  As another example, you may wish
for ant to use a proxy when downloading (there is a lot to be
downloaded).   Some examples:
`export ANT_OPTS="-Xms512M -Xmx512M"`
`export MAVEN_OPTS="-Xms512M -Xmx512M"`
or
`export ANT_OPTS="-Dhttp.proxyHost=xxx.xxx.xxx.xxx -Dhttp.proxyPort=3128"`

##### Run ant:
`ant`         [builds all benchmarks]

`ant dist`    [builds all benchmarks, this is the default]

`ant source`  [builds a source distribution including benchmarks and tools]

`ant bm`      [builds a specific benchmark, bm]

**NOTE**

A log of each directory is created under this benchmark directory
for benchmark build status and build success or failure files
to be stored.  The directory log directory is normally of the
form
`${basedir}/log/${build.time}`
and contains status.txt where each benchmark build status is recorded,
and either pass.txt if all benchmarks build, or fail.txt if one or
more benchmarks fail to build. Note: that either fail.txt or pass.txt
is created when a full build is performed.

**IMPORTANT:** before trying to build the suite:

1. Set your `JAVA_HOME` environment variable appropriately (it must be set and be consistent with the VM that will be used to build the suite).

2. You must set `jdk8home`, in the `default.properties`, to point to a Java s installation.


For more information, run `ant -p` in the benchmarks directory.

## Customization

It is possible to use callbacks to run code before a benchmark starts, when it stops, and when the run has completed.
To do so, extend the class `Callback` (see the file `harness/src/MyCallback.java` for an example).

To run a benchmark with your callback, run:

    java -jar dacapo.jar -c <callback> <benchmark>

## Source Code Structure

### `harness` (The benchmark harness)

This directory includes all of the source code for the DaCapo harness, which is used to invoke the benchmarks, validate output, etc.

	
### `bms` (The benchmarks)

* `bms/<bm>/src` Source written by the DaCapo group to drive the benchmark `<bm>`
* `bms/<bm>/downloads`	MD5 sums for each of the requisite downloads.  These are used to cache the downloads (avoiding re-downloading on each build)
* `bms/<bm>/data` Directory containing any data used to drive the benchmark
* `bms/<bm>/<bm>.cnf`	Configuration file for `<bm>`
* `bms/<bm>/<bm>.patch`	Patches against the orginal sources (if any)
* `bms/<bm>/build.xml`	Local build file for <bm>
* `bms/<bm>/build` _Directory where building occurs.  This is only created at build time._
* `bms/<bm>/dist` _Directory where the result of the build goes.  This is only created at build time._


### `libs` (Common code used by one or more benchmarks.)

Each of these directories more or less mirror the `bm` directories.



## License

The DaCapo Benchmark Suite conmprises several open source or public
domain programs, plus a test harness, some patches to enable the
benchmarks to run under the test harness, and a packaging process. The
benchmarks are distributed under their own licenses and the remaining
component is distributed under the Apache License, version 2.0.

   Copyright 2009 The DaCapo Project,
   Department of Computer Science
   University of Massachusetts,
   Amherst MA. 01003, USA

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
