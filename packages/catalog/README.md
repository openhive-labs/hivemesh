# Catalog

Owns provider-local resource registration, resource descriptors, exposure policy, filtered catalog calculation, and catalog-change events.

The core calculation is:

```text
registry intersect exposure policy intersect visibility lease intersect availability
```

Hidden resources and fields are omitted. This package never returns local endpoints, administrative routes, upstream credentials, private prompts, or internal adapter configuration.

It may project visible model resources to the gateway's `/v1/models` view, but it does not grant execution authority; the lease package remains responsible for invocation admission.
