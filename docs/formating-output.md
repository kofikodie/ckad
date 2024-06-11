# Formatting Output with kubectl

The default output format for all kubectl commands is the human-readable plain-text format.

The -o flag allows us to output the details in several different formats.


kubectl [command] [TYPE] [NAME] -o <output_format>

Here are some of the commonly used formats:


1. `-o json` Output a JSON formatted API object.

2. `-o name` Print only the resource name and nothing else.

3. `-o wide` Output in the plain-text format with any additional information.

4. `-o yaml` Output a YAML formatted API object.

For more details, refer:

https://kubernetes.io/docs/reference/kubectl/overview/

https://kubernetes.io/docs/reference/kubectl/cheatsheet/