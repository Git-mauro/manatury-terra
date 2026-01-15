# Manatury - Infraestructura Terraform (staging / production)

Este repositorio contiene una estructura ejemplo para gestionar múltiples entornos (staging, production) usando Terraform en AWS.

Resumen rápido
- Separación por entornos: /live/staging/ /live/production/
- Módulos reutilizables: /modules/
- Backend remoto: archivos backend.<env>.conf (S3 + DynamoDB)
- CI: workflows para plan en PR y apply en main (usar OIDC o secrets AWS)

Requisitos
- Terraform >= 1.3
- Cuenta AWS con permisos para S3, DynamoDB y recursos que vayas a crear

Estructura de ejemplo
- modules/network/ : módulo reutilizable (VPC + subnets ejemplo)
- live/staging/api/ : stack concreto para el servicio "api" en staging
- live/production/api/ : stack para producción

Instrucciones principales (local)
1. Copia el archivo de backend correspondiente y personalízalo: 
   cp backend.staging.conf live/staging/api/backend.conf
   # Edita backend.conf y reemplaza el bucket y tabla DynamoDB (REPLACE_ME_BUCKET, REPLACE_ME_DDB_TABLE)

2. Inicializa Terraform con el backend personalizado:
   terraform init -backend-config=backend.conf

3. Plan y apply (ejemplo):
   terraform plan -var-file=terraform.tfvars -out=plan.tfplan
   terraform apply "plan.tfplan"

Notas sobre el backend
- backend.staging.conf y backend.prod.conf contienen valores de ejemplo. Sustituye REPLACE_ME_* por tus recursos (bucket y tabla dynamodb).

Workflows
- .github/workflows/terraform-plan.yml -> corre terraform fmt/validate/plan en PRs
- .github/workflows/terraform-apply.yml -> aplica en main (requiere protección/entorno)

Imágenes turísticas (enlaces CC0 / dominio público)
- Colombia (Wikimedia Commons search): https://commons.wikimedia.org/wiki/Category:Colombia
- Cartagena (Wikimedia Commons): https://commons.wikimedia.org/wiki/Category:Cartagena,_Colombia
- Latinoamérica (Wikimedia Commons): https://commons.wikimedia.org/wiki/Category:Latin_America

Siguientes pasos
- Reemplaza los placeholders del backend (bucket, tabla)
- Personaliza los módulos según tu topología
- Añade protección a la rama main y aprovisionamiento vía CI con aprobaciones humanas para production
