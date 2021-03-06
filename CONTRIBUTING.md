# Contributing

## Local
For pull requests or local commits:

    ./test/before_install_build && ./test/script
    docker run --rm -ti local/jruby jruby_version
    travis lint #if you changed .travis.yml
    git checkout -b tmp-9.0.5f #name your branch according to your changes
    #git add ... git commit ... git push ... open pull request

For repository owners only:

    git commit -m "BUA-732 Upgrade Ubuntu to 20161010"
    git tag -d latest && git tag 9.0.5f && git push origin tmp-9.0.5f && git push --tags

-- Wait for Travis to pass OK: https://travis-ci.org/elgalu/jruby-in-docker/builds
-- Make sure changes got merged into master by elgalubot: https://github.com/elgalu/jruby-in-docker

    git checkout master && git pull && git branch -d tmp-9.0.5f && git push origin --delete tmp-9.0.5f

-- Re-add TBD_* section in CHANGELOG.md starting with TBD_DOCKER_TAG
-- Upgrade release tag in github.com with latest CHANGELOG.md

## Retry
Failed in Travis? retry

    git tag -d 9.0.5f && git push origin :9.0.5f
    #git add ...
    git commit --amend && git tag 9.0.5f && git push --force origin tmp-9.0.5f && git push --tags

## Docker push from Travis CI
Travis [steps](https://docs.travis-ci.com/user/docker/#Pushing-a-Docker-Image-to-a-Registry) involve `docker login` and docker credentials encryptions.

### Requirements

* Ruby
* `gem install travis --no-rdoc --no-ri`
* `travis login --user elgalu`
* Encrypt environment variables with travis cli

### Encrypt
    travis env set DOCKER_EMAIL me@example.com
    travis env set DOCKER_USERNAME elgalubot
     travis env set DOCKER_PASSWORD secretsecret #1st space in purpose
     travis env set GH_TOKEN secretsecret

### Bot setup
#### github.com
- bot: Fork the repo
- owner: Add bot as collaborator
- bot: Accept collaborator invitation
- bot: Generate personal token

#### hub.docker
- owner: Add bot as collaborator

#### travis-ci.org
- owner: Enable the project
- owner: Run all the required `travis env set`
