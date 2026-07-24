# protos

Single source of truth for all `.proto` contracts in the Duke-ECE
managed-agents project. Generated Go code is committed under `gen/go/`, so
consumers only need `go get` — no protoc/buf setup required.

## Layout

```
proto/<package>/<version>/*.proto   # the contracts
gen/go/<package>/<version>/         # generated Go, committed — do not edit by hand
```

## Consumers (Go)

```sh
go get github.com/Duke-ECE/protos@latest   # or pin a tag, e.g. @v0.1.0
```

```go
import v1 "github.com/Duke-ECE/protos/gen/go/sandbox/v1"
```

## Adding or changing a proto

1. Edit / add files under `proto/`.
2. `buf lint` and `buf generate` (requires `buf`, `protoc-gen-go`, `protoc-gen-go-grpc`).
3. Commit both the `.proto` and the regenerated `gen/go/` — CI fails if they drift.
4. Breaking changes are rejected in CI (`buf breaking` against main). Additive
   changes (new RPCs, new fields) are fine within the same `vN` package;
   anything breaking needs a new package version (`v2`).

## Versioning

Tags follow semver: `v0.x.y` while the APIs stabilize, `v1.0.0` once they do.
Bump the patch/minor tag after merging additive changes so consumers can pin.
