# Puppet module: monit

This is a Puppet module for monit based on the second generation layout ("NextGen") of Example42 Puppet Modules.

Made by Alessandro Franceschi / Lab42

Official site: http://www.example42.com

Official git repository: http://github.com/example42/puppet-monit

Released under the terms of Apache 2 License.

This module requires functions provided by the Example42 Puppi module (you need it even if you don't use and install Puppi)

For detailed info about the logic and usage patterns of Example42 modules check the DOCS directory on Example42 main modules set.


## USAGE - Basic management

* Install monit with default settings

        class { 'monit': }

* Install a specific version of monit package

        class { 'monit':
          version => '1.0.1',
        }

* Disable monit service.

        class { 'monit':
          disable => true
        }

* Remove monit package

        class { 'monit':
          absent => true
        }

* Monitor a service (SSH in this example) by its pid file

        class { 'monit::checkpid':
          process => 'sshd',
          pidfile => '/var/run/sshd.pid'
        }

## USAGE - Overrides and Customizations
* Use custom sources for main config file

        class { 'monit':
          source => [ "puppet:///modules/example42/monit/monit.conf-${hostname}" , "puppet:///modules/example42/monit/monit.conf" ], 
        }


* Use custom source directory for the whole configuration dir

        class { 'monit':
          source_dir       => 'puppet:///modules/example42/monit/conf/',
          source_dir_purge => false, # Set to true to purge any existing file not present in $source_dir
        }

* Use custom template for main config file. Note that template and source arguments are alternative. 

        class { 'monit':
          template => 'example42/monit/monit.conf.erb',
        }

* Automatically include a custom subclass

        class { 'monit':
          my_class => 'example42::my_monit',
        }

* Include additional tests for a service config file

        class { 'monit::checkpid':
          process          => 'sshd',
          pidfile          => '/var/run/sshd.pid',
          additional_tests => 'if failed host 192.168.1.100 port 22 then alert'
        }

## USAGE - Example42 extensions management 
* Activate puppi (recommended, but disabled by default)

        class { 'monit':
          puppi    => true,
        }

* Activate puppi and use a custom puppi_helper template (to be provided separately with a puppi::helper define ) to customize the output of puppi commands 

        class { 'monit':
          puppi        => true,
          puppi_helper => 'myhelper', 
        }

* Activate automatic monitoring (recommended, but disabled by default). This option requires the usage of Example42 monitor and relevant monitor tools modules

        class { 'monit':
          monitor      => true,
          monitor_tool => [ 'nagios' , 'monit' , 'munin' ],
        }

* Activate automatic firewalling. This option requires the usage of Example42 firewall and relevant firewall tools modules

        class { 'monit':       
          firewall      => true,
          firewall_tool => 'iptables',
          firewall_src  => '10.42.0.0/24',
          firewall_dst  => $ipaddress_eth0,
        }


## CONTINUOUS TESTING

Travis {<img src="https://travis-ci.org/example42/puppet-monit.png?branch=master" alt="Build Status" />}[https://travis-ci.org/example42/puppet-monit]
