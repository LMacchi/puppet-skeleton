## Puppet Skeleton Usage

In your development workstation:
```
[root@agent1 ~]# puppet agent --configprint module_skeleton_dir
/opt/puppetlabs/puppet/cache/puppet-module/skeleton
```

Go to the module_skeleton_dir and clone this repo:

```
[root@agent1 ~]# cd /opt/puppetlabs/puppet/cache/puppet-module/skeleton
[root@agent1 skeleton] git clone https://github.com/LMacchi/puppet-skeleton
```

Done! The skeleton will be used each time a new module gets created:

```
[root@agent1 ~]# puppet module generate laura/test
We need to create a metadata.json file for this module.  Please answer the
following questions; if the question is not applicable to this module, feel free
to leave it blank.

Puppet uses Semantic Versioning (semver.org) to version modules.
What version is this module?  [0.1.0]
-->

Who wrote this module?  [laura]
-->

What license does this module code fall under?  [Apache-2.0]
-->

How would you describe this module in a single sentence?
--> "A test module"

Where is this module's source code repository?
--> https://github.com/LMacchi/test

Where can others go to learn more about this module?  [https://github.com/LMacchi/test]
-->

Where can others go to file issues about this module?  [https://github.com/LMacchi/test/issues]
-->

----------------------------------------
{
  "name": "laura/test",
  "version": "0.1.0",
  "author": "laura",
  "summary": "\"A test module\"",
  "license": "Apache-2.0",
  "source": "https://github.com/LMacchi/test",
  "project_page": "https://github.com/LMacchi/test",
  "issues_url": "https://github.com/LMacchi/test/issues",
  "dependencies": [
    {"name":"puppetlabs-stdlib","version_requirement":">= 1.0.0"}
  ],
  "data_provider": null
}
----------------------------------------

About to generate this metadata; continue? [n/Y]
-->

Notice: Generating module at /root/test...
Notice: Populating templates...
Finished; module generated in test.
test/spec
test/spec/classes
test/spec/classes/init_spec.rb
test/spec/fixtures
test/spec/fixtures/manifests
test/spec/fixtures/modules
test/spec/spec_helper.rb
test/metadata.json
test/Gemfile
test/Puppetfile
test/files
test/examples
test/examples/init.pp
test/templates
test/manifests
test/manifests/init.pp
test/README.md
test/Rakefile
```

Run `rake --tasks` to get a list of available tasks
