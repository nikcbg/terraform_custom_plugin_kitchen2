# terraform_custom_plugin_kitchen2

### Purpose of the repository 
- The repository uses custom plugin and test it with `kichen` framework.

#### List of files in the repository

File name                            | File description 
------------------------------------ | --------------------------------------------------------------
`scripts/provision.sh` | script that installs Go and Terraform on virtual machine. 
`test/integration/default/default_test.rb` | test to check the output of the virtual machine.
`.gitignore` | which files and directories to ignore. 
`.kitchen.yml` | configuration file for `kitchen` test framework.
`Gemfile` | used for `ruby` dependencies.
`Vagrantfile` | configuration file that creates and installs software on VM.
`main.tf` | Terraform configuration file to use custom plugin. 

### How to use this repository. 
- Install `terraform` by following this [instructions](https://www.terraform.io/intro/getting-started/install.html).
- Install `vagrant` by following this [instructions](https://www.vagrantup.com/downloads.html)
- Install `virtualbox` by following this [instructions](https://www.virtualbox.org/wiki/Downloads)
- Clone the repository to your local computer: `git clone git@github.com:nikcbg/terraform_custom_plugin_kitchen2`.
- Go to the cloned repo on your computer: `cd terraform_custom_plugin_kitchen2`.

### Compiling custom plugin

Command execution | Command purpose
------------------|------------------------------
`vagrant up` | to create and configure the VM.
`vagrant ssh` | to login to the VM we just cretaed.
below steps are executed on the Vagrant VM after you login and | are for compiling the custom plugin
`go get github.com/petems/terraform-provider-extip` | to clone the repository with custom plugin.
`cd ~/go/src/github.com/petems/terraform-provider-extip`| to go into the directory where the repo is cloned.
`make build` | to compile the plugin from the downloaded repository.
`ls -al ~/go/bin/` | to list the compiled plugin. 
`mkdir -p /vagrant/terraform.d/plugins/linux_amd64` | to create new directory for the lugin.
`cp ~/go/bin/terraform-provider-extip /vagrant/terraform.d/plugins/linux_amd64/` | to copy the plugin to the required path

### Performing the test with `kitchen`
- you still need to be login to the VM to perform the test.

Command execution | Command purpose
------------------|------------------------------
`cd /vagrant` | to go into `vagrant` directory
`sudo -s` | to become root on the VM
`gem install bundler` | to install `gem` which is package manager for ruby.
`bundle install` | to install `kitchen` framework.
`bundle exec kitchen list` | to list `kitchen` instances.
`bundle exec kitchen converge`	| to create `kitchen` environment.
`bundle exec kitchen verify`	| to execute `kitchen` test.
`bundle exec kitchen destroy`	| to destroy `kitchen` environment.
`bundle exec kitchen test`	| to automatically build, test and destroy `kitchen` environment.

- If the command/s complete successfully the output should look like the one below:
```
Profile: tests from {:path=>"/vagrant/test/integration/default"} (tests from {:path=>".vagrant.test.integration.default"})
Version: (not specified)
Target:  local://

  ✔  check_output: xxx.xxx.xxx.xxx
     ✔  xxx.xxx.xxx.xxx should not eq "127.0.0.1"
     ✔  xxx.xxx.xxx.xxx should not eq ""
     ✔  xxx.xxx.xxx.xxx should not eq "nil"


Profile Summary: 1 successful control, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped

```


### TO DO: 
- Check if everything works as expected. 
