# Filtering results

The following information is based on these [filtering rules](https://docs.driftctl.com/0.22.0/usage/filtering/rules).

Filter rules can be used not only to **describe** resources, but also to **ignore** resources.

You can use both inclusion and exclusion logic.

**Filter rules** allow you to build complex expressions to include and exclude a set of resources in your workflow. You can build a complex include and exclude expression, powered by the expression language [JMESPath](https://jmespath.org).

Filters are applied on a normalized structure which contains the following fields:

* **Type**: Type of the resource, for example, `aws_s3_bucket`
* **Id**: Id of the resource, for example, `my-bucket-name`
* **Attr**: every resource attribute (see the [terraform attributes reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3\_bucket#attributes-reference) for a full list of supported attributes of a bucket)

**Note**: If you want to filter on `Attr`, enable deep mode to have access to the details of the resource.

The following code shows examples of include and exclude commands.[​](https://docs.driftctl.com/0.22.0/usage/filtering/rules#examples)

```
# Will include only S3 bucket in the search
$ snyk iac describe --filter "Type=='aws_s3_bucket'"
# OR (beware of escape your shell special chars between double quotes)
$ snyk iac describe --filter $'Type==\'aws_s3_bucket\''
# Excludes only s3 bucket named 'my-bucket-name'
$ snyk iac describe --filter $'Type==\'aws_s3_bucket\' && Id!=\'my-bucket-name\''
# Ignore buckets that have tags terraform equal to 'false'
$ snyk iac describe --deep --filter $'!(Type==\'aws_s3_bucket\' && Attr.tags.terraform==\'false\')'
# Ignore buckets that don't have tag terraform
$ snyk iac describe --deep --filter $'!(Type==\'aws_s3_bucket\' && Attr.tags != null && !contains(keys(Attr.tags), \'terraform\'))'
# Ignore buckets with an ID prefix of 'terraform-'
$ snyk iac describe --filter $'!(Type==\'aws_s3_bucket\' && starts_with(Id, \'terraform-\'))'
# Ignore buckets with an ID suffix of '-test'
$ snyk iac describe --filter $'!(Type==\'aws_s3_bucket\' && ends_with(Id, \'-test\'))'
# Ignore GitHub archived repositories
$ snyk iac describe --to github+tf --deep --filter '!(Attr.Archived)'
```
