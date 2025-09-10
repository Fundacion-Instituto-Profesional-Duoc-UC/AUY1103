## 🚀 Comandos básicos de Terraform Workspaces

Comandos ejemplos para aplicar Workspaces:

```bash
terraform workspace list
terraform workspace new develop <subcommand> [options] [args]
terraform apply -var-file="develop.tfvars"
terraform workspace select staging
terraform apply -var-file="testing.tfvars"