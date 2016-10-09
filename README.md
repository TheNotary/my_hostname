# my_hostname

This cookbook demonstrates including a recipe from another cookbook.  It does so by creating a wrapper cookbook around the [hostname cookbook](https://supermarket.chef.io/cookbooks/hostname). 

## Variables

Things specific to the machine you'll being deployed onto are found in `attributes`.  These attributes are just automatically loaded up.  It's interesting to note that the attribute in the case of this repo isn't being used from a recipe in this cookbook, rather it is used in a dependency (insert mind blown graphic).  This cookbook is very much a just a wrapper for that cookbook in it's current state.  

## Dependency cookbooks

Dependencies must be marked in `metadata.rb` for Opscode Chef to work.  Because Chef doesn't support dependency tracking out of the box yet, the dependencies must also be marked in Berksfile as well so that when `berks vendor .dependency_cookbooks/` is run, the correct dependencies can be installed into the `.dependency_cookbooks/` folder.  

## Usage

In its most straightforward usage, one can use chef-solo in the root of this directory to apply the infrastructure to a local machine to be configured.  

1. Install the dependencies:

```
$ berks vendor .dependency_cookbooks/
```

2. Apply the infrastructure to a local machine to be configured:

```
$ chef-solo -c .solo.rb
```

This path will utilize the `.solo.rb` and `.node.json` config files.  I chose to hide them with a dot because this repo is really designed to be 'just a cookbook' and as such would required in from a server configuration repo containing the appropriate `solo.rb` and `node.json` files in addition to metadata.rb.

As specified in `.node.json`, the `recipes/default.rb` script will be applied which simply invokes the `hostname` cookbook, the logic of which is not tracked into this repo.  


## Testing

Kitchen is the testing thing for chef.  The config file is `.kitchen.yml`.  To test things perform...

```
$ kitchen test
```

This will actually utilize vagrant, download and cache an ubuntu vagrant box (per `.kitchen.yml`) and then deploy the configurations to the VM before ultimately running the simple and straightforward test suite located in `test/` and `spec/`.

