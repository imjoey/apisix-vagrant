<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Apache APISIX vagrant (Experimental)

HashiCorp Vagrant is a tool to maintain isolated environments for developing and testing. Please refer to [Vagrant](https://www.vagrantup.com) for more details. We assumes you have both vagrant and a proper provider already installed.

This project will provide us a dedicated environment for developing APISIX with local code synced.

> Note that this project is still under developement. Any and all feedback is welcome.


## Development environment

Please follow the steps below to build our development environment.

```shell
# Clone this repository
$ git clone https://github.com/imjoey/apisix-vagrant

# Clone APISIX repository
$ git clone https://github.com/apache/apisix

# Build the box and sync your cloned ../apisix code into vagrant machine
$ cd apisix-vagrant && vagrant up

# SSH into vagrant machine and install dependencies
$ vagrant ssh
$ cd /apisix && make deps

# Check APISIX version and start it
$ ./bin/apisix version && ./bin/apisix start

# Check the availability of APISIX Admin API which will return the following data:
#   {
#      "count":"1",
#      "node":{
#         "dir":true,
#         "key":"\/apisix\/services",
#         "nodes":{
#            
#         }
#      },
#      "action":"get"
#   }
$ curl "http://127.0.0.1:9080/apisix/admin/services/" -H 'X-API-KEY: edd1c9f034335f136f87ad84b625c8f1'
```

## Troubleshooting

### How to use http/https proxy in vagrant machine

In order to support proxy for vagrant, we need to install the [`proxyconf`](https://github.com/tmatilai/vagrant-proxyconf) plugin.

```bash
vagrant plugin install vagrant-proxyconf
```

Then vagrant is able to run behind a proxy by either of the following ways.

- Add proxy configurations into your `Vagrantfile`

```
  config.proxy.http     = "http://your-http-proxy"
  config.proxy.https    = "http://your-https-proxy"
  config.proxy.no_proxy = "localhost,127.0.0.1"
```

- Add proxy environment variables for `vagrant` command

```
export VAGRANT_HTTP_PROXY=http://your-http-proxy
export VAGRANT_HTTPS_PROXY=http://your-https-proxy
export VAGRANT_NO_PROXY="localhost,127.0.0.1"
vagrant up
```



## TODO

- [ ] Support testing environment via initializing `cpanminus` and `test-nginx`
- [ ] Support developing and testing Apache APISIX Dashboard repository
- [ ] Support trying Apache APISIX and APISIX Dashboard with official releases
- [ ] More improvements, such as input, output, logic and error handling