# yum-zabbix Cookbook

The yum-zabbix cookbook takes over management of the default repositoryids that ship with CentOS systems. It allows attribute manipulation of `zabbix`, `zabbix-non-supported`

## Requirements
### Platforms
- RHEL/CentOS and derivatives

### Chef
- Chef 11+

### Cookbooks
- yum version 3.4.0 or higher
- yum-epel version 3.4 or higher

## Attributes
The following attributes are set by default

```ruby
default['yum']['zabbix']['repositoryid'] = 'zabbix'
default['yum']['zabbix']['enabled'] = true
default['yum']['zabbix']['managed'] = true
default['yum']['zabbix']['gpgkey'] = 'http://repo.zabbix.com/RPM-GPG-KEY-ZABBIX'
default['yum']['zabbix']['gpgcheck'] = true
default['yum']['zabbix']['description'] = 'Zabbix Official Repository - $basearch'
default['yum']['zabbix']['baseurl'] = 'http://repo.zabbix.com/zabbix/3.4/rhel/7/$basearch'
```

```ruby
default['yum']['zabbix-non-supported']['repositoryid'] = 'zabbix-non-supported'
default['yum']['zabbix-non-supported']['enabled'] = false
default['yum']['zabbix-non-supported']['managed'] = false
default['yum']['zabbix-non-supported']['gpgkey'] = 'http://repo.zabbix.com/RPM-GPG-KEY-ZABBIX'
default['yum']['zabbix-non-supported']['gpgcheck'] = true
default['yum']['zabbix-non-supported']['description'] = 'Zabbix Official Repository non-supported - $basearch'
default['yum']['zabbix-non-supported']['baseurl'] = 'http://repo.zabbix.com/non-supported/rhel/6/$basearch/'
```

## Recipes
- default - Walks through node attributes and feeds a yum_resource
- parameters. The following is an example a resource generated by the
- recipe during compilation.

```ruby
  yum_repository 'zabbix' do
    baseurl 'http://repo.zabbix.com/zabbix/3.4/rhel/7/$basearch/'
    description 'Zabbix Official Repository - $basearch'
    enabled true
    gpgcheck true
    gpgkey 'http://repo.zabbix.com/RPM-GPG-KEY-ZABBIX'
  end
```

## Usage Example
To disable the `zabbix` repository through a Role or Environment definition

```
default_attributes(
  'yum' => {
    'zabbix' => {
      'enabled' => false
    }
  }
)
```

Uncommonly used repositoryids are not managed by default. This is speeds up integration testing pipelines by avoiding yum-cache builds that nobody cares about. To enable the `zabbix-non-supported` repository with a wrapper cookbook, place the following in a recipe:

```
node.default['yum']['zabbix-non-supported']['managed'] = true
node.default['yum']['zabbix-non-supported']['enabled'] = true
include_recipe 'yum-zabbix'
```

## More Examples
Point the base and debuginfo repositories at an internally hosted server.

```
node.default['yum']['zabbix']['enabled'] = true
node.default['yum']['zabbix']['baseurl'] = 'https://internal.example.com/centos/7/os/x86_64'
node.default['yum']['zabbix']['sslverify'] = false
node.default['yum']['zabbix-non-supported']['enabled'] = true
node.default['yum']['zabbix-non-supported']['baseurl'] = 'https://internal.example.com/centos/7/updates/x86_64'
node.default['yum']['zabbix-non-supported']['sslverify'] = false

include_recipe 'yum-zabbix'
```

## License & Authors
**Author:** Eric Renfro (<psi-jack@linux-help.org>)

**Copyright:** 2016, Linux-Help.org.

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
