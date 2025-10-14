# Terraform Modular Infrastructure

Este repositorio contiene una colección de módulos reutilizables de **Terraform** organizados por capas funcionales para implementar infraestructura en la nube siguiendo buenas prácticas de IaC (Infrastructure as Code).  
Cada módulo está diseñado para ser independiente, fácil de mantener y componible en entornos de desarrollo, pruebas y producción.

---

## 📁 Estructura del repositorio

modules/
├── cluster/
│ └── asg-rolling-deploy/ # Implementa un Auto Scaling Group (ASG) con despliegue gradual (rolling updates)
├── data-stores/
│ └── mysql/ # Despliega una base de datos MySQL administrada (RDS)
├── networking/
│ └── alb/ # Configura un Application Load Balancer (ALB)
└── services/
└── hello-world-app/ # Despliega una aplicación web de ejemplo "¡Hola desde Terraform y AWS!"


---

## 🚀 Descripción de los módulos

### **1. cluster/asg-rolling-deploy**
- Implementa un **Auto Scaling Group (ASG)** con soporte para despliegues controlados.
- Permite reemplazar instancias gradualmente sin tiempo de inactividad.
- Ideal para servicios backend o aplicaciones distribuidas.

### **2. data-stores/mysql**
- Despliega una instancia de **Amazon RDS para MySQL**.
- Incluye configuraciones básicas de red, almacenamiento y backups automáticos.
- Compatible con los módulos de red y aplicaciones del repositorio.

### **3. networking/alb**
- Crea un **Application Load Balancer (ALB)** en AWS.
- Distribuye el tráfico entre instancias EC2 o contenedores en el ASG.
- Permite configurar listeners HTTP/HTTPS y grupos de destino.

### **4. services/hello-world-app**
- Ejemplo de despliegue de una aplicación web simple.
- Se conecta al ALB y puede integrarse con el módulo MySQL.
- Útil para pruebas, validación de pipelines y demostraciones CI/CD.

---

## ⚙️ Requisitos

- **Terraform >= 1.5.0**
- **AWS CLI configurado**
- Credenciales válidas en `~/.aws/credentials`

---

## 🧩 Cómo usar los módulos

Cada módulo puede ser llamado desde una configuración principal de Terraform, por ejemplo:

```hcl
module "hello_world_app" {
  source = "../modules/services/hello-world-app"

  environment = "dev"
  instance_type = "t3.micro"
  vpc_id        = module.network.vpc_id
  subnet_ids    = module.network.public_subnets
}


terraform init
terraform apply
terraform destroy