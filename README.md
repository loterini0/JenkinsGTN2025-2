<<<<<<< HEAD
# JenkinsGTN2025-2
=======
# 🚀 Despliegue de Infraestructura con AWS CloudFormation y Jenkins

Este repositorio contiene una plantilla de AWS CloudFormation (`ec2.yml`) para desplegar una infraestructura básica que incluye una instancia EC2 con Jenkins y otra con Apache, utilizando Docker. A continuación, te guiaré paso a paso para desplegar la infraestructura y acceder a la contraseña inicial de Jenkins.

---

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener lo siguiente:

### 1. AWS CLI instalado y configurado:

- Instala AWS CLI siguiendo las [instrucciones oficiales](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
- Configura tus credenciales ejecutando:

  ```bash
  aws configure
  ```
  Proporciona tu **Access Key**, **Secret Key**, región y formato de salida.

### 2. Clave SSH:

- Necesitarás una clave SSH (`prueba.pem`) para conectarte a las instancias EC2. Asegúrate de que esté disponible en tu máquina local.

---

## 🛠 Despliegue con AWS CloudFormation

### 1. Clona este repositorio:

```bash
git clone <URL-del-repositorio>
cd <nombre-del-repositorio>
```

### 2. Despliega la plantilla de CloudFormation:

```bash
aws cloudformation create-stack \
  --stack-name jenkins-apache-stack \
  --template-body file://ec2.yml \
  --capabilities CAPABILITY_NAMED_IAM
```

### 3. Verifica el estado de la pila:

```bash
aws cloudformation describe-stacks --stack-name jenkins-apache-stack --query "Stacks[0].StackStatus"
```

### 4. Obtén las IPs públicas de las instancias:

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=instanciaJenkins,instanciaApache" \
  --query "Reservations[*].Instances[*].PublicIpAddress" \
  --output text
```

---

## 🐳 Acceso a Jenkins y Obtención de la Contraseña Inicial

### 1. Conéctate a la instancia de Jenkins:

```bash
ssh -i /ruta/a/tu/clave-privada.pem ubuntu@<IP-Pública-de-la-Instancia-Jenkins>
```

### 2. Obtén la contraseña inicial de Jenkins:

```bash
sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

- Copia la contraseña que se muestra en la terminal.

### 3. Accede a Jenkins en el navegador:

```
http://<IP-Pública-de-la-Instancia-Jenkins>:8080
```

---

## 🖥 Acceso a la Instancia de Apache

### 1. Conéctate a la instancia de Apache:

```bash
ssh -i /ruta/a/tu/clave-privada.pem ubuntu@<IP-Pública-de-la-Instancia-Apache>
```

### 2. Verifica los contenedores en ejecución:

```bash
sudo docker ps
```

### 3. Accede a la aplicación en el navegador:

```
http://<IP-Pública-de-la-Instancia-Apache>:80
```

---

## 🧹 Limpieza

Para eliminar la infraestructura y evitar costos innecesarios, elimina la pila de CloudFormation:

```bash
aws cloudformation delete-stack --stack-name jenkins-apache-stack
```

---

## 🛠 Troubleshooting

Si encuentras algún problema durante el despliegue:

- **Revisa los logs de UserData:**

  ```bash
  cat /var/log/cloud-init-output.log
  ```

- **Verifica los grupos de seguridad:**
  Asegúrate de que los puertos **22 (SSH), 80 (HTTP) y 8080 (Jenkins)** estén abiertos en los grupos de seguridad asociados a las instancias.

- **Revisa los logs de Docker:**

  ```bash
  sudo docker logs <nombre-del-contenedor>
  ```

---

¡Y eso es todo! Ahora tienes una infraestructura desplegada en AWS con Jenkins y Apache listos para usar. Si tienes alguna pregunta o necesitas más ayuda, no dudes en abrir un issue en este repositorio. ¡Feliz despliegue! 🚀
>>>>>>> 17f9d56 (first commit)
