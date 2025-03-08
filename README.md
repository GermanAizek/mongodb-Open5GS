# ![Logo](docs/leaf.svg) MongoDB Open5GS README

![изображение](https://github.com/user-attachments/assets/cd2cffd2-2161-4aa1-a7d4-ae4116f7b9f8)

# Requirements for Building

- Debian 12 (Debian older if not apply patches Python-Fix and GCC-14)
- 128 GB free space
- 32 GB RAM

Need packages:

```
sudo apt-get install libssl-dev lld python3-venv python3-pip liblzma-dev libcurl4-openssl-dev build-essential git scons
sudo apt-get install python3-dev python3-pip libssl-dev libcurl4-openssl-dev libboost-dev python-dev-is-python3 libssl-dev
```

# Building

```
git clone https://github.com/GermanAizek/mongodb-Open5GS
cd mongodb-Open5GS
git checkout open5gs
```

Configuration build using patches:

If CPU don't support AVX (old CPU or new low power CPU):

```
git apply < patches/0001-no-AVX-CPU-instructions.patch
```

If use Python 3.12 and newer versions:

```
git apply < patches/0002-python-3.12-and-newer-fix.patch
```

If use GCC 14 and newer versions:

```
git apply < patches/0003-fix-build-GCC-14-and-newer.patch
```

If you want to reduce size mongo binaries:

```
git apply < patches/0004-reduce-output-binary-size.patch
```

Starting building:
```
python3 buildscripts/scons.py DESTDIR=/usr/local/mongo install-mongod --disable-warnings-as-errors
```

## LICENSE

MongoDB is free and the source is available. Versions released prior to
October 16, 2018 are published under the AGPL. All versions released after
October 16, 2018, including patch fixes for prior versions, are published
under the [Server Side Public License (SSPL) v1](LICENSE-Community.txt).
See individual files for details which will specify the license applicable
to each file. Files subject to the SSPL will be noted in their headers.
