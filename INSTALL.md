# Preconfigured secrets

In namespace > secret > data:

* `flux-system`
	* `github-commit-status-token`
		* `token`: GitHub PAT with `repo:status` scope
	* `github-webhook-token`
		* `token`: random string used as webhook secret
	* `matrix`
		* `address`: matrix homeserver
		* `token`: matrix access token
			* For a synapse homeserver, registering, joining room and obtaining token can be done via admin API
* `mariadb-galera`
	* `mariadb-galera`
		* `mariadb-galera-mariabackup-password`
		* `mariadb-root-password`
			* These should be <= 32 bytes
* `monitoring`
	* `grafana-envs`
		* `GF_AUTH_GITHUB_CLIENT_SECRET`: GitHub OAuth client secret
	* `matrix-webhook-url`
		* `url`: Slack-compatible webhook url
* `sashimi`
	* `image-update-webhook-token`
		* `token`: random string used as webhook secret
	* `config`
		* `env`: Laravel .env

# Bootstrapping / Upgrading flux

```
flux bootstrap git --url=ssh://git@github.com/sambunhi/infra.git --branch=main --path=clusters/sambunhi --private-key-file <key> --interval=1h --components-extra image-reflector-controller,image-automation-controller
```

`<key>` is a preconfigured deploy key, used for both pull/push of this command and future reconciling.

Get webhook url by inspecting status of `flux get receivers sambunhi-github`, and set up webhook for push event.
