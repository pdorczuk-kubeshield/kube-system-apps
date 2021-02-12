# Kubeshield-Sealed Secrets

Kustomize manifests that installs ArgoCD with an NGINX ingress and OAuth authentication through Github.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

You need access to a Kubernetes cluster and Kustomize installed locally on your machine.

```
Give examples
```

Create a Secret with your GitHub OAuth application secret.

```
echo -n "<CLIENT_SECRET>" | kubectl create secret -n operations generic argocd-secret --dry-run=client --from-file=oauth.argocd.github.clientSecret=/dev/stdin -o yaml > secret.yaml
```

Seal the secret

```
kubeseal --controller-namespace operations --controller-name=sealedsecrets-sealed-secrets --format yaml <secret.yaml >sealedsecret.yaml
```

Delete the original secret so you don't accidentally commit it.

```
rm secret.yaml
```

### Installing

Deploy the Helm chart with Kustomizations.

```
kustomize build --enable_alpha_plugins . | kubectl create -f-
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
