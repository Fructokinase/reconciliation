# Data Commons Reconciliation Service

Data Commons Reconciliation Service provides Knowledge Graph entity reconciliation services. It gets deployed in a Kubernetes cluster.

## About Data Commons

[Data Commons](https://datacommons.org/) is an Open Knowledge Graph that
provides a unified view across multiple public data sets and statistics. It
includes [APIs](https://docs.datacommons.org/api/) and visual tools to easily
explore and analyze data across different datasets without data cleaning or
joining.

## License

Apache 2.0

### GitHub Development Process

In <https://github.com/datacommonsorg/reconciliation>, click on "Fork" button to fork the
repo.

Clone your forked repo to your desktop.

Add datacommonsorg/reconciliation repo as a remote:

```shell
git remote add dc https://github.com/datacommonsorg/reconciliation.git
```

Every time when you want to send a Pull Request, do the following steps:

```shell
git checkout master
git pull dc master
git checkout -b new_branch_name
# Make some code change
git add .
git commit -m "commit message"
git push -u origin new_branch_name
```

Then in your forked repo, you can send a Pull Request. If this is your first
time contributing to a Google Open Source project, you may need to follow the
steps in [contributing.md](contributing.md).

Wait for approval of the Pull Request and merge the change.

## Usage

### ResolveEntities

Example curl command (entity described by subgraph):

```bash
curl -X POST https://api.datacommons.org/v1/recon/entity/resolve -d '{"entities":{"source_id":"newId/SantaClaraCountyId","sub_graph":{"nodes":{"newId/SantaClaraCountyId":{"pvs":{"wikidataId":{"typed_values":{"type":"TEXT","value":"Q110739"}}}}}}}}'
```

Example curl command (entity described by IDs):

```bash
curl -X POST https://api.datacommons.org/v1/recon/entity/resolve -d '{"entities":{"source_id":"newId/SantaClaraCountyId","entity_ids":{"ids":{"prop":"geoId","val":"06085"}}}}'
```

### ResolveCoordinates

Example curl command: 

```bash
curl -X POST https://api.datacommons.org/v1/recon/resolve/coordinate -d '{"coordinates": [{"latitude":"37.42","longitude":"-122.08"},{"latitude":"32.41","longitude":"-102.11"}]}'
```

### ResolveIds

Example curl command: 

```bash
curl -X POST https://api.datacommons.org/v1/recon/resolve/id -d '{"in_prop":"wikidataId","out_prop":"dcid","ids":["Q110739","Q30"]}'
```

## Generate Protobuf Libraries

Install command line tools: `protoc`, `protoc-gen-go`, `protoc-gen-go-grpc`. Then run the following command in root directory:

```bash
protoc \
--proto_path=proto \
--go_out=internal \
--go-grpc_out=internal \
--go-grpc_opt=require_unimplemented_servers=false \
proto/*.proto
```

## Test

Firstly, ensure appropriate GCP credentials:

```bash
gcloud auth login
gcloud auth application-default login
```

Then, run all unit tests and integration tests from the root directory:

```bash
go test ./...
```

To update golden files for the integration tests, use the `generate_golden` flag in `./test/integration/setup.go`.

## Support

For general questions or issues about tool development, please open an issue
on our [issues](https://github.com/datacommonsorg/reconciliation/issues) page. For all
other questions, please send an email to `support@datacommons.org`.

**Note** - This is not an officially supported Google product.
