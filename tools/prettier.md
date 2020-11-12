# Prettier

Global path to use for global prettier in IntelliJ & co via WSL:

```
/home/g-rath/.nodenv/versions/14.4.0/lib/node_modules/prettier
```

You can get this by:

```
dirname $(realpath $(nodenv which prettier))
```
