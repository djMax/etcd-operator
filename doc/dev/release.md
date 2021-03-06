# Release Workflow

**NOTE**: During release, don't merge any PR other than bumping the version.

Let's say we are releasing $VERSION .

## Create git tag

In version/version.go, bump it to:
```go
	Version = $VERSION
```

Send a PR. After it's merged, create a tag:
```
$ git tag $VERSION
$ git push ${upstream_remote} tags/$VERSION
```

Bump version again:
```go
	Version = $VERSION
```
Send another PR and merge it.

## Push Quay image

Login to quay.io using docker if haven't:

```
$ docker login quay.io
```
Follow the prompts.

Refer to [developer_guide.md](./developer_guide.md) on how to fetch dependency and build.

After build, push image:
```
$ IMAGE=quay.io/coreos/etcd-operator:$VERSION hack/build/operator/build
```

Retag "latest":
```
$ docker tag quay.io/coreos/etcd-operator:$VERSION quay.io/coreos/etcd-operator:latest
$ docker push quay.io/coreos/etcd-operator:latest
```

## Github release

Click "releases" in the top bar.
Click "$VERSION" tag.
Click "Edit Tag".
Fill in title and release notes.
If it's not stable release, click "This is a pre-release".
