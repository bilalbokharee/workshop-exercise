## 🧪 Testing all the things 🧪

### Markdown code tests

Testing our markdown code snippets using the awesome [rundoc](https://gitlab.com/nul.one/rundoc) tool.

`rundoc` allows you to run your markdown files as if they were scripts. Every code snippet with code highlighting tag is run with interpreter defined by the tag.

We check the command and success return codes (we filter output for now) against `good` run files so we can pick up and regression with the code snippets. See the `doc-regression-test-files/good-*.json` files for the expected outputs. These files are generated.

A `non zero` exit code signifies some tests failed. See the test logs for details.

Testing Requirements:
- A running OpenShift cluster with tl500 tooling installed to run against

### Running tests in OpenShift

The tests can run on OpenShift:

```bash
oc new-project tech-exercise-test
oc -n tech-exercise-test run test-$(date +"%Y-%m-%d-%H-%M-%S") --image=quay.io/eformat/tech-exercise-test:latest \
  --env="CLUSTER_DOMAIN=${CLUSTER_DOMAIN}" \
  --env="GIT_SERVER=${GIT_SERVER}" \
  --env="TEAM_NAME=${TEAM_NAME}" \
  --env="GITLAB_USER=${GITLAB_USER}" \
  --env="GITLAB_PASSWORD=${GITLAB_PASSWORD}" \
  --env="OCP_USER=${OCP_USER}" \
  --env="OCP_PASSWORD=${OCP_PASSWORD}" \
  --env="OCP_ADMIN_USER=${OCP_ADMIN_USER}" \
  --env="OCP_ADMIN_PASSWORD=${OCP_ADMIN_PASSWORD}" \
  --restart=Never
```

### Running tests locally

The tests run in a container on your laptop:

```bash
podman run \
  -e "CLUSTER_DOMAIN=${CLUSTER_DOMAIN}" \
  -e "GIT_SERVER=${GIT_SERVER}" \
  -e "TEAM_NAME=${TEAM_NAME}" \
  -e "GITLAB_USER=${GITLAB_USER}" \
  -e "GITLAB_PASSWORD=${GITLAB_PASSWORD}" \
  -e "OCP_USER=${OCP_USER}" \
  -e "OCP_PASSWORD=${OCP_PASSWORD}" \
  -e "OCP_ADMIN_USER=${OCP_ADMIN_USER}" \
  -e "OCP_ADMIN_PASSWORD=${OCP_ADMIN_PASSWORD}" \
  quay.io/eformat/tech-exercise-test:latest 
```

### Test Development

To run testing locally for development:

```bash
podman pull quay.io/rht-labs/stack-tl500:3.0.14
podman run -d --name stack quay.io/rht-labs/stack-tl500:3.0.14 zsh -c 'sleep infinity'
podman exec -it stack zsh

git clone https://github.com/rht-labs/tech-exercise.git
cd tech-exercise && cd tests

# Run the test suite
./regression.sh
```

### TO BE DONE

**_FIXME - All the fiddly bits that need more work_**

**_TODO_**

- [ ] regression.sh can generate output files
- [ ] Jenkinsfile replacer test in 3a-Jenkins uses a file :( instead of docs for now
- [ ] trigger and wait for pipelines first run success
- [ ] cleanup() must be cluster-admin

**_DONE_**

- [X] `oc login` manual for now
- [X] GitLab create team and public repos first
- [X] GitLab adding webhooks
- [X] GitLab creds first time we commit / cache
- [X] GitLab secret manual for now
- [X] add a tidy function to delete all ocp resources at end of tests
- [X] waits on resources .. e.g for Nexus, Jenkins pods - ho do we sync this ? hardcode between tests with a test markdown ?
- [X] patch rundoc for upto 4 whitespace in markdown -> html
- [X] remove branch for tests development uj
```bash
# FIXME test branch
--set source_ref=tests
```
- [X] GitLab delete all other API patoken's when creating a new one
- [X] add a tidy function to delete all git resources at end of tests
