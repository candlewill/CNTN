## Install sparrowhawk

1. Install openfst
```shell
wget http://www.openfst.org/twiki/pub/FST/FstDownload/openfst-1.6.7.tar.gz
tar zxvf openfst-1.6.7.tar.gz
cd openfst-1.6.7
./configure --enable-grm --prefix=`pwd`/build
make
make install
```

2. Install thrax
```shell
cd thrax
wget http://www.openfst.org/twiki/pub/GRM/ThraxDownload/thrax-1.2.6.tar.gz
tar zxvf thrax-1.2.6.tar.gz
cd thrax-1.2.6
./configure CPPFLAGS=-I/home/heyunchao/Programs/sparrowhawk/depend/openfst/openfst-1.6.7/build/include LDFLAGS="$LDFLAGS -L/home/heyunchao/Programs/sparrowhawk/depend/openfst/openfst-1.6.7/build/lib/" --prefix=`pwd`/build
make -j 12
make install
```

3. Install google/re2
```shell
cd depend
git clone https://github.com/google/re2.git
cd re2
```

Then, use Make or CMake to build:

```shell
make prefix=`pwd`/build
make test
make install
make testinstall
```
or CMake:
```shell
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX:PATH=`pwd`/lib ..
make
make install
```

4. Install protobuf 2.5.0 (It's possible newer version is also supported, but I have not test it!)
```shell
wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
tar zxvf protobuf-2.5.0.tar.gz
cd protobuf-2.5.0
./configure --prefix=`pwd`/build
make install
```

5. Install sparrowhawk
The path should be edited according to your system.

```
export PATH="$PATH:/home/heyunchao/Programs/sparrowhawk/depend/Protocol/protobuf-2.5.0/build/bin"
CPPFLAGS="-ggdb -O0 -I/home/heyunchao/Programs/sparrowhawk/depend/openfst/openfst-1.6.7/build/include -I/home/heyunchao/Programs/sparrowhawk/depend/thrax/thrax-1.2.6/include -I/home/heyunchao/Programs/sparrowhawk/depend/re2/build/lib/include -I/home/heyunchao/Programs/sparrowhawk/depend/Protocol/protobuf-2.5.0/build/include" \
LDFLAGS="$LDFLAGS -L/home/heyunchao/Programs/sparrowhawk/depend/openfst/openfst-1.6.7/build/lib -L/home/heyunchao/Programs/sparrowhawk/depend/thrax/thrax-1.2.6/lib -L/home/heyunchao/Programs/sparrowhawk/depend/re2/build/lib/lib -L/home/heyunchao/Programs/sparrowhawk/depend/Protocol/protobuf-2.5.0/build/lib" \
./configure --prefix=`pwd`/build
make
make install
```

Note: In my case, I met some problem. And I dealt with it based on this article: http://www.laruence.com/2009/11/18/1154.html . Regenerate the `./configure` file using `aclocal; autoconf; automake --add-missing;`.

All done!

## Run Test

```
../../build/bin/normalizer_main --config=sparrowhawk_configuration.ascii_proto --multi_line_text < test.txt   2>/dev/null
```

## Build wfst
Once Thrax is installed you should go into the classify directory and do:

```
thraxmakedep tokenize_and_classify.grm
make
```

Similarly in the verbalize directory:

```
thraxmakedep verbalize.grm
make
```

If you didn't install thrax as sudo user, you should edit the `PATH` variables to use the `thraxmakedep` command.

```
export PATH=$PATH:<Your-Thrax-BIN-Path>
```

And finally in the verbalize_serialization directory, do the same as above.

## Example Result

Input
```
The train left at 3:30 from Penn Station on Jan. 3, 2010. Mr. Snookums
was on the train carrying $40.25 (£30.60) of Belgian chocolate in a 3kg box that
was 20cm wide.
```

Output
```
The train left at three thirty from Penn Station on January the third twenty ten sil
Mr. Snookums was on the train carrying forty dollars and twenty five cents sil thirty pounds and sixty pence sil of Belgian chocolate in a three kilograms box that was twenty centimeters wide sil
```

## Author

Yunchao He

2018/04/09

@xiaomi