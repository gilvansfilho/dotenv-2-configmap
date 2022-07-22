# dotenv-2-configmap
Bash script to create openshift or kubernetes configmap from .env or .properties file (or whatever key=value file).



## How To

```bash
git clone https://github.com/gilvansfilho/dotenv-2-configmap.git
cd dotenv-2-configmap
source dotenv-2-configmap
dotenv-2-configmap
```

> Executing `dotenv-2-configmap` without args will print instructions
## Syntax

```
dotenv-2-configmap configmap_name -f file_path [-n namespace] [--dryrun] [--cli oc|kubectl] [-D delimiter]
```

## Examples

### Simple
Create in current namespace a configmap with all `key=values` from file

```bash
dotenv-2-configmap my_configmap -f example.env --dryrun
```

> When using `--dryrun` parameter the command will not be executed but printed. For demostration purposes all examples will use this.

### Changing CLI
You could change target CLI using `--cli` parameter. Default value is `oc`

```bash
dotenv-2-configmap my_configmap -f example.env --cli kubectl --dryrun
```

> Valid values are `oc` and `kubectl`

### Changing file field delimiter
You could change file field delimiter using `-D` parameter. Default value is `=`

```bash
dotenv-2-configmap my_configmap -f example.properties -D : --dryrun
```

### Filtering lines from file   

You could apply filter using awk filter expressions

```bash
dotenv-2-configmap my_configmap -f example.env --filter /^key/ --dryrun
```

### Defining namespace

To execute in a namespace other than current you could use `-n` parameter

```bash
dotenv-2-configmap my_configmap -f example.env -n target_namespace --dryrun
```

