# repositorio-EC2
# Terraform AWS Flask App

Este proyecto despliega una infraestructura básica en **AWS** utilizando **Terraform**, que incluye:

- Una VPC pública.
- Subred con acceso a internet.
- Una instancia EC2.
- Una API en Flask que consulta datos JSON desde un bucket S3.
- Un archivo HTML básico para buscar ventas por `ID_SALES`.

## Requisitos

- [Terraform](https://www.terraform.io/downloads)
- Una cuenta de AWS
- Un bucket S3 con archivos JSON que contengan la clave `ID_SALES`
- Un par de claves EC2 ya creado en la región correspondiente (`.pem`)

## Estructura del Proyecto

```
terraform/
├── main.tf              # Recursos principales (VPC, EC2, S3 policy)
├── variables.tf         # Variables parametrizables
├── outputs.tf           # Salidas útiles como IP pública
├── provider.tf          # Configuración del proveedor AWS
└── scripts/
    └── init.sh.tmpl     # Script de inicialización EC2 (Flask + HTML)
```

## Descripción de la aplicación

Se lanza una instancia EC2 que:

1. Instala Python, Flask y Boto3.
2. Despliega una aplicación Flask con un endpoint:
   - `/data-json-<ID_SALES>`: Busca archivos `.json` en el bucket y retorna el objeto que coincida con el `ID_SALES`.
3. Expone una página HTML sencilla donde se puede ingresar el ID a buscar.

## Variables

Estas pueden modificarse en `terraform.tfvars` o directamente en `variables.tf`:

| Variable        | Descripción                          | Valor por defecto        |
|----------------|--------------------------------------|--------------------------|
| `aws_region`   | Región de AWS                        | `"us-east-1"`            |
| `key_name`     | Nombre del par de claves EC2         | `"ec2"`                  |
| `instance_type`| Tipo de instancia EC2                | `"t2.micro"`             |
| `bucket_name`  | Bucket S3 con archivos JSON          | `"bucket-json-clear"`    |

## Uso

1. Inicializa Terraform:

```bash
terraform init
```

2. Revisa el plan de infraestructura:

```bash
terraform plan
```

3. Aplica los cambios:

```bash
terraform apply
```

4. Una vez completado, accede a la aplicación en el navegador:

```
http://<IP_PUBLICA>:5000
```

Puedes encontrar la IP pública como salida de Terraform o directamente en AWS.

## Limpieza

Para destruir todos los recursos creados:

```bash
terraform destroy
```

## Autor

Generado automáticamente a partir de la infraestructura incluida en el proyecto.