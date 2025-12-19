- **Integrating Vault with the CI/CD pipeline:** You'll configure the CI/CD script to retrieve secrets from Vault dynamically during build or deployment tasks.
- **Configuring Vault:** This involves setting up the Vault environment, defining the Vault server address, logging in using the root token, and enabling and activating a key/value secrets engine in Vault.
- **Managing the secrets lifecycle:** You'll create secrets in Vault for the pipeline, such as API keys and database passwords, and verify their existence.
- **Setup access policies:** You'll define access policies in Vault to control who can access these secrets. For example, you'll create a policy granting read access to the API key.

**CI/CD Script**: Below is TryFinMe's integration script you've been given as a reference. It retrieves the secrets during a deployment task:
```
```bash
# Retrieve API key from HashiCorp Vault
API_KEY=$(vault kv get -field=value TFMSecrets/apiKey)

# Retrieve database credentials from HashiCorp Vault
DB_USERNAME=$(vault kv get -field=username TFMSecrets/dbCredentials)
DB_PASSWORD=$(vault kv get -field=password TFMSecrets/dbCredentials)

# Export API key and database credentials for TryFinMe deployment
export API_KEY
export DB_USERNAME
export DB_PASSWORD

# Use the API_KEY and DB credentials in TryFinMe deployment
echo "API key retrieved: $API_KEY"
echo "Database credentials retrieved: Username - $DB_USERNAME, Password - $DB_PASSWORD"
```
## Configuring Vault
Set up your Vault environment for key management
`export VAULT_ADDR='http://10.48.139.15:8222'`

Checks the overall vault information
`vault status`

Vault has a sealing feature that is activated after certain triggers. For security purposes, the Vault gets sealed after a machine running Vault reboots. We can unseal it by running the seal command with 3 unseal keys
- `vault operator unseal bhviSZYUcVvnfgxTa9UGKd7xWWcItyKcOznDdgM2REuG`
- `vault operator unseal imWHuOEOzDChEzXRqMU8wVrorcddMVDGHA6pQMxMxlFV`
- `vault operator unseal B0nDKPnkk/uBT72svjP2JSQQT3lyM8d/+yMukmmxCIe4`

Log in
`vault login`

enable the Secrets Engine and activate a key/value, where `-path` is the name of the TryFinMe (TFM) environment where the secrets will live:
`vault secrets enable -path=TFMSecrets kv`

Setting up Your Policy
1. `vault policy write cryptops - <<EOF   `
2. `path "TFMSecrets/*" {`
3. `capabilities = ["create", "read", "update", "delete", "list"]`
4. `}`
5. `EOF`

Next, create the policy by running
`vault token create -policy=cryptops`

Create the secrets for the pipeline:
- `vault kv put TFMSecrets/apiKey value="12345-67890-ABCDE-FGHIJ"`
- `vault kv put TFMSecrets/dbCredentials username="db_user" password="s3cureP@ssw0rd!"

Read the secret to check
`vault kv get TFMSecrets/apiKey`