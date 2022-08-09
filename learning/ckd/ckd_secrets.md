

# Secret

-   Used to store secrets like passwords
-   Not secure since it relies on base64 encoding
-   A better option for secrets handling is Hashicorp Vault or something similar


# YAML Definition Example

To define a `Secrets` map, base64 encode the values beforehand:


# Commands

To create a `Secret` in place:

    kubectl create secret generic <secret-name> --from-literal=<key>=<value> \
    	[--from-literal=<key>=<value>]
    # OR
    kubectl apply -f <secrets definition file> 

