# Branching

For branching we use the [git flow](https://nvie.com/posts/a-successful-git-branching-model/) branching model. This means that we have two main branches: `master` and `develop`. The `master` branch is used for production deployments and the `develop` branch is used for staging deployments. For each feature, we create a new branch from `develop` and merge it back into `develop` once it's ready to be deployed to staging. Once we are ready to deploy to production, we merge `develop` into `master`.

For a further staging layer, please consult [this](https://docs.getdbt.com/blog/the-case-against-git-cherry-picking) blogpost.