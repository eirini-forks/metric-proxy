# Releasing Metric Proxy

Note that metric-proxy does NOT use a develop->main workflow. Commits are pushed and PRs are merged directly to main, letting the pipeline test changes after landing on main.

## Steps to release

1. Make sure metric-proxy `main` has what you need.
1. Head to the [cf-k8s-metric-proxy-validation](https://release-integration.ci.cf-app.com/teams/main/pipelines/cf-k8s-metric-proxy-validation) pipeline and make sure the `test-metric-proxy-bump-on-cf-for-k8s` job is green
1. Bump the version appropriately using the `metric-proxy-cut-{major|minor|patch}` job.
    - Note: This will update the metric-proxy/version file and cause the tests to run again
1. Finish by triggering the create-release job
    - Takes care of OSL and runs `deplab`
    - Updates the `values.yml` file to point to new image sha created in the job
1. Head to [metric-proxy releases](https://github.com/cloudfoundry/metric-proxy/releases) and edit the new draft release. Add relevant release notes and then publish the release.
    - This pushes the docker image with the corresponding release version tag

### Pipeline resources
* The pipeline is flown to https://release-integration.ci.cf-app.com/teams/main/pipelines/cf-k8s-metric-proxy-validation via `cd ~/workspace/metric-proxy && ./ci/configure`
* Yaml config lives in https://github.com/cloudfoundry/metric-proxy/blob/main/ci/validation.yml

### TODO Document/automate creation of statsd_exporter image

[story link](https://www.pivotaltracker.com/story/show/176179500)

https://github.com/cloudfoundry/cf-k8s-prometheus/blob/master/exporters/statsd_exporter/Dockerfile