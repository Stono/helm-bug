# Demonstrating a helm bug

- kubectl apply -f crd.yaml
- helm upgrade example --install . 
```
❯ helm upgrade example --install . 
Release "example" does not exist. Installing it now.
NAME: example
LAST DEPLOYED: Wed Oct  6 10:43:23 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

- edit [templates/instance1.yaml](templates/instance1.yaml) to be `spec.name=invalid`, and remove [templates/instance2.yaml](templates/instance2.yaml) from the chart
- helm upgrade example --install . 

```
❯ helm upgrade example --install . 
Error: UPGRADE FAILED: cannot patch "example1" with kind Example: Example.stono.io "example1" is invalid: spec.name: Invalid value: "": spec.name in body should match '^valid$'
```

- Attempt to (but unable to) roll back to previous release:
```
❯ helm rollback example 1
Error: no Example with the name "example2" found
```