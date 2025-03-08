# ![Logo](docs/leaf.svg) MongoDB Open5GS README

![изображение](https://github.com/user-attachments/assets/cd2cffd2-2161-4aa1-a7d4-ae4116f7b9f8)

# Requirements for Building

- Debian 12 (older without Python and GCC patches) or other APT manager-like Distro
- 48 GB free space
- 16 GB RAM minimum

# Building Mongo Daemon

Need installed packages for building from sources:

```
sudo apt-get install libssl-dev lld python3-venv python3-pip liblzma-dev libcurl4-openssl-dev build-essential git scons
sudo apt-get install python3-dev python3-pip libssl-dev libcurl4-openssl-dev libboost-dev python-dev-is-python3 libssl-dev
```

Cloning repo:

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
python3 buildscripts/scons.py install-mongod --disable-warnings-as-errors
```

After successful building:

Copying binaries

```
sudo cp -r build/install/bin/* /usr/bin
```

Configuration services

```
sudo cp patches/mongod.conf /etc/
sudo chown root:root /etc/mongod.conf
sudo useradd mongodb
sudo mkdir /var/lib/mongodb
sudo mkdir /var/log/mongodb
sudo chown mongodb:mongodb /var/lib/mongodb
sudo chown mongodb:mongodb /var/log/mongodb
sudo chmod 750 /var/lib/mongodb
sudo chmod 750 /var/log/mongodb
sudo cp patches/mongod.service /usr/lib/systemd/system/
```

Starting Daemon:

```
sudo systemctl start mongod
sudo systemctl enable mongod
```

# Building Open5GS from sources:

```
git clone https://github.com/open5gs/open5gs/
cd open5gs
git checkout v2.7.2
git pull
```

Setting up TUN device (not persistent after rebooting)

```
sudo misc/netconf.sh
```

Install the common dependencies for building the source code

```
sudo apt install python3-pip python3-setuptools python3-wheel ninja-build build-essential flex bison git cmake libsctp-dev libgnutls28-dev libgcrypt-dev libssl-dev libmongoc-dev libbson-dev libyaml-dev libnghttp2-dev libmicrohttpd-dev libcurl4-gnutls-dev libnghttp2-dev libtins-dev libtalloc-dev meson
```

Install libidn-dev or libidn11-dev depending on your system

```
if apt-cache show libidn-dev > /dev/null 2>&1; then
    sudo apt-get install -y --no-install-recommends libidn-dev
else
    sudo apt-get install -y --no-install-recommends libidn11-dev
fi
```

To compile with meson:

```
meson build --prefix=`pwd`/install
ninja -C build
```

Check results unit-tests:

All tests must have `All tests passed.`

```
build/tests/attach/attach
build/tests/registration/registration
```

Run all test programs as below.

```
cd build
meson test -v
```

Installing Open5GS in system.

```
ninja install
cd ../
```



## LICENSE


- Open5GS Open Source files are made available under the terms of the GNU Affero General Public License ([GNU AGPL v3.0](https://www.gnu.org/licenses/agpl-3.0.html)).

- MongoDB is free and the source is available. Versions released prior to
  October 16, 2018 are published under the AGPL. All versions released after
  October 16, 2018, including patch fixes for prior versions, are published
  under the [Server Side Public License (SSPL) v1](LICENSE-Community.txt).
  
  See individual files for details which will specify the license applicable
  to each file. Files subject to the SSPL will be noted in their headers.
