# Jedi way to create configmap and secrets from env files

## What you will see here...

If you are here I suppose you known what are configmaps and secret. So I will not explain what they are. Instead I will show some tricks do create them from .env or .properties files (or whatever "key value" pairs file).

## Default way to create confimap and secret from env file

Both kubectl (kubernetes) and oc (openshift) CLI have parameter to create configmap and secret from key value pairs file. The parameter is `--from-env-file`
To use this you could do something like:

```bash
oc create configmap myconfigmap --from-env-file=/path/to/file -n namespace
```

Same way to create a secret instead you could do:

```bash
oc create secret mysecret generic --from-env-file=/path/to/file -n namespace
```

But suppose you have the following example.env file:

```
key1=value
key2=value2
xkey3=value3
pass1=passw0rd
```

How could you?
1.  Process only some lines? Example: create configmap only with lines starting with "key"
2.  Changing delimiter? Example: lines has "key:value" format

## Enhanced way to create confimap and secret from env file

With a bit of bash magic you can filter lines, changing delimiter and achieve our expected outcomes

To create confimap with lines starting with "key" you could do that:

```
oc create configmap mycm_name $(awk -F= /^key/'{printf "--from-literal="$1"="$2 " "}' example.env)
```

To create secret with lines starting with "pass" you could do that:

```
oc create secret generic mysecret_name $(awk -F= /^pass/'{printf "--from-literal="$1"="$2 " "}' example.env)
```

To see and inspect result before execute you could run in dry run mode and set yaml as output

```
oc create configmap mycm_name $(awk -F= /^key/'{printf "--from-literal="$1"="$2 " "}' example.env) --dry-run -o yaml
```

That way you will see something like:

```
W0722 10:52:03.934813   67160 helpers.go:598] --dry-run is deprecated and can be replaced with --dry-run=client.
apiVersion: v1
data:
  key1: value
  key2: value2
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: mycm_name
```

What about changing delimiter? Suppose you .env file is
```
key1:value
key2:value2
xkey3:value3
pass1:passw0rd
```

You will need to change delimiter parameter (`-F`) of awk command. Like this:

```
oc create configmap mycm_name $(awk -F: /^key/'{printf "--from-literal="$1"="$2 " "}' example.env) --dry-run -o yaml
```

 If you dont want to care of about awk syntax take a look at this repository https://github.com/gilvansfilho/dotenv-2-configmap