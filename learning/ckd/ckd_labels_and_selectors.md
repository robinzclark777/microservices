

# Labels

-   key/value pairs
-   defined in `metadata/labels` field in `yaml`


# Selectors

-   simple equality with `<key>=<value>`
-   `and` logical operator is a comma: `<key 1>=<value 1>,<key 2>=<value 2>+`
-   set-based operators:
    -   in: `<key> in (<value 1>, <value 2>, ... <value n>)`
    -   not in: `<key> notin (<value 1>, <value 2>, ...<value n>)`
    -   key exists: `key`
    -   key does not exist: `!key`
-   there is no logical `or`


# Commands

To find objects by label:

    kubectl get [all|pods|services...] --selector <key>=<value>[,<key>=<value>]*

To label a object:

    kubectl label <objec type> <node name> <key>=<value>

