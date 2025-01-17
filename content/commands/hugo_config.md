---
date: 2021-05-07
title: "hugo config"
slug: hugo_config
url: /commands/hugo_config/
---
## hugo config

Print the site configuration

### Synopsis

Print the site configuration, both default and custom settings.

```
hugo config [flags]
```

### Options

```
  -h, --help   help for config
```

### Options inherited from parent commands

```
      --config string              config file (default is path/config.yaml|json|toml)
      --configDir string           config dir (default "config")
      --debug                      debug output
  -e, --environment string         build environment
      --ignoreVendor               ignores any _vendor directory
      --ignoreVendorPaths string   ignores any _vendor for module paths matching the given Glob pattern
      --log                        enable Logging
      --logFile string             log File path (if set, logging enabled automatically)
      --quiet                      build in quiet mode
  -s, --source string              filesystem path to read files relative from
      --themesDir string           filesystem path to themes directory
  -v, --verbose                    verbose output
      --verboseLog                 verbose logging
```

### SEE ALSO

* [hugo](/commands/hugo/)	 - hugo builds your site
* [hugo config mounts](/commands/hugo_config_mounts/)	 - Print the configured file mounts

###### Auto generated by spf13/cobra on 7-May-2021
