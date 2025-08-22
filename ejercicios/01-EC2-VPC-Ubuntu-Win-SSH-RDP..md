## EJERCICIO-01: Aprovisionamiento de EC2 (Windows 2019 Server, Ubuntu 20.04) en una VPC (subred), creación de un par de claves, conexión a Ubuntu mediante SSH y conexión a Windows mediante RDP.

Este ejemplo muestra:
- Cómo crear pares de claves (claves públicas y privadas) en AWS.
- Cómo crear EC2 (Ubuntu 20.04, Windows 2019 Server).
- Cómo crear una nube virtual privada (VPC), componentes de VPC (subred pública, puerta de enlace de Internet, tabla de rutas) y cómo vincularlos entre sí.
- Cómo crear grupos de seguridad (para SSH y Escritorio remoto).

**Código:** https://github.com/omerbsezer/Fast-Terraform/tree/main/samples/ec2-vpc-ubuntu-win-ssh-rdp

![1 VPC-IG-EC2](https://github.com/user-attachments/assets/29c5a207-bc35-43f1-8c4e-75d77acf77c1)

### Prerrequisito

- Deberías tener previamente instalado el Terraform y configurado el acceso a la cuenta de AWS.

## Pasos

- Se utilizan pares de claves SSH (clave pública y privada) para conectar con el servidor remoto. La clave pública (xx.pub) se encuentra en el servidor remoto. Con la clave privada, el usuario puede conectarse mediante SSH.

- Hay dos maneras de crear pares de claves (pública y privada):
- Crearlos en la nube (AWS)
- EC2 > Pares de claves > Crear par de claves
- Crearlos localmente
- "ssh-keygen -t rsa -b 2048"

- Creación de pares de claves en AWS: Vaya a EC2 > Pares de claves

![imagen](https://user-images.githubusercontent.com/10358317/228974087-b57126ab-6589-48fe-b609-1f18dc2f0c7e.png)

- Tras crear los pares de claves, la clave pública aparece en AWS:

![imagen](https://user-images.githubusercontent.com/10358317/228974292-0d2d16ec-5590-4929-9b4e-8bc0215dcd60.png)

- La clave privada (testkey.pem) se descarga en su dispositivo PC:

![imagen](https://user-images.githubusercontent.com/10358317/228974369-46c54f25-ea80-40dd-9c75-670ece815bf2.png)

- Copia este testkey.pem en el directorio donde se encuentra el archivo main.tf.

![imagen](https://user-images.githubusercontent.com/10358317/228974784-de1b9be4-9083-45ec-a9ab-5a1e54aee2c5.png)


- Cree main.tf en el directorio count y copie el código:
 
``` 
# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
  required_version = ">= 1.2.0"
}

provider "aws" {
	region = "eu-central-1"
}

resource "aws_vpc" "my_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  tags = {
    Name = "My VPC"
  }
}

resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.my_vpc.id
  cidr_block        = "10.0.0.0/24"
  availability_zone = "eu-central-1c"
  tags = {
    Name = "Public Subnet"
  }
}

resource "aws_internet_gateway" "my_vpc_igw" {
  vpc_id = aws_vpc.my_vpc.id
  tags = {
    Name = "My VPC - Internet Gateway"
  }
}

resource "aws_route_table" "my_vpc_eu_central_1c_public" {
    vpc_id = aws_vpc.my_vpc.id
    route {
        cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.my_vpc_igw.id
    }
    tags = {
        Name = "Public Subnet Route Table"
    }
}
resource "aws_route_table_association" "my_vpc_eu_central_1c_public" {
    subnet_id      = aws_subnet.public.id
    route_table_id = aws_route_table.my_vpc_eu_central_1c_public.id
}

resource "aws_security_group" "allow_ssh" {
  name        = "allow_ssh_sg"
  description = "Allow SSH inbound connections"
  vpc_id      = aws_vpc.my_vpc.id
  # for SSH
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  # for HTTP Apache Server
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  # for RDP
  ingress {
    from_port        = 3389
    to_port          = 3389
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
  }
  # for ping
  ingress {
    from_port        = -1
    to_port          = -1
    protocol         = "icmp"
    cidr_blocks      = ["10.0.0.0/16"]
  }
  egress {
    from_port       = 0
    to_port         = 0
    protocol        = "-1"
    cidr_blocks     = ["0.0.0.0/0"]
  }
  tags = {
    Name = "allow_ssh_sg"
  }
}

resource "aws_instance" "ubuntu2004" {
  ami                         = "ami-0e067cc8a2b58de59" # Ubuntu 20.04 eu-central-1 Frankfurt
  instance_type               = "t2.nano"
  key_name                    = "testkey"
  vpc_security_group_ids      = [aws_security_group.allow_ssh.id]
  subnet_id                   = aws_subnet.public.id
  associate_public_ip_address = true
  user_data = <<-EOF
		           #! /bin/bash
                           sudo apt-get update
		           sudo apt-get install -y apache2
		           sudo systemctl start apache2
		           sudo systemctl enable apache2
		           echo "<h1>Deployed via Terraform from $(hostname -f)</h1>" | sudo tee /var/www/html/index.html
  EOF
  tags = {
    Name = "Ubuntu 20.04"
  }
}

resource "aws_instance" "win2019" {
	ami                         = "ami-02c2da541ae36c6fc" # Windows 2019 Server eu-central-1 Frankfurt
	instance_type               = "t2.micro"
        key_name                    = "testkey"
        vpc_security_group_ids      = [aws_security_group.allow_ssh.id]
        subnet_id                   = aws_subnet.public.id  
	associate_public_ip_address = true
        tags = {
		Name = "Win 2019 Server"
	}
}

output "instance_ubuntu2004_public_ip" {
  value = "${aws_instance.ubuntu2004.public_ip}"
}

output "instance_win2019_public_ip" {
  value = "${aws_instance.win2019.public_ip}"
}
``` 

**Código:** https://github.com/omerbsezer/Fast-Terraform/blob/main/samples/ec2-vpc-ubuntu-win-ssh-rdp/main.tf

![imagen](https://user-images.githubusercontent.com/10358317/228973324-4bc1c6ad-1099-4f56-8e6f-c002f719d9d4.png)

- Ejecutar el comando init:

```
terraform init
```

![imagen](https://user-images.githubusercontent.com/10358317/228975022-6ed0663d-dca8-427b-8a9b-7cce25522117.png)

- Validar Archivo:

```
terraform validate
```

![imagen](https://user-images.githubusercontent.com/10358317/228975088-077a0562-ee85-4ad2-8cc9-2f4ccd1c4c94.png)

- Ejecutar el comando "plan":

```
terraform plan
```

- Ejecutar el comando "apply" para crear recursos. Terraform solicita confirmación; escribe "sí":

```
terraform apply
```
![imagen](https://user-images.githubusercontent.com/10358317/228975493-a28ece3e-b00d-436e-aab1-de7b679378f3.png)

![imagen](https://user-images.githubusercontent.com/10358317/229282068-a4f293cf-cde7-4b6b-9e9e-48bd40822aae.png)

- En AWS EC2 > Instancias, Ubuntu 20.04:

![imagen](https://user-images.githubusercontent.com/10358317/228975962-72c0fbe9-e1dc-482c-8edc-573013424953.png)

- Grupos de seguridad (SSG), para SSH (puerto 22), RDP (puerto 3389), HTTP (80), ICMP (para ping):

![imagen](https://user-images.githubusercontent.com/10358317/228976204-c4993141-c8ad-4110-8f05-c1fb1e2c13c2.png)

![imagen](https://user-images.githubusercontent.com/10358317/228976348-f07e284e-be34-4898-9b81-92a756910a56.png)

- En AWS EC2 > Instancias, Ventana 2019 Servidor:

![imagen](https://user-images.githubusercontent.com/10358317/228976557-7951b759-73fd-46a6-b325-08389e52b86f.png)

- Windows tiene el mismo SSG que Ubuntu.

Almacenamiento, almacenamiento en bloque elástico predeterminado:

![imagen](https://user-images.githubusercontent.com/10358317/228976823-4e80c3e5-8ec6-4e4b-b5d2-aa064982954d.png)

En el servicio AWS VPC (nube virtual privada):

![imagen](https://user-images.githubusercontent.com/10358317/228977082-f0634c8e-8701-45e2-aa22-f0b7dc1e3400.png)

Al instalar Ubuntu 20.04, se utilizan los datos de usuario para instalar el servidor Apache. Con el puerto SSG 80 y usando una IP pública, podemos ver el archivo index.html, como el servidor de alojamiento:

![imagen](https://user-images.githubusercontent.com/10358317/228978767-aee0b725-eff2-40b2-8a30-93f40c15a052.png)

- SSH a Ubuntu 20.04 (ssh -i testkey.pem ubuntu@<PublicIPAddress>):

![imagen](https://user-images.githubusercontent.com/10358317/228977324-3393ae14-85d8-48ba-9133-bc7a6f1ef73b.png)

- Ejecutar:

```
sudo apt install net-tools
ifconfig
```

- Se puede ver la IP privada:

![imagen](https://user-images.githubusercontent.com/10358317/228977710-610437ee-c1c2-4a7e-82e7-ac07d38aed62.png)

- Establecer conexión remota a Windows (RDP):

![imagen](https://user-images.githubusercontent.com/10358317/228977887-14d82ad9-41ff-43eb-bdaf-2c3e007df506.png)

- Descargar RDP Aplicación:

![imagen](https://user-images.githubusercontent.com/10358317/228978036-79956ff6-755e-4205-aa17-4ac45f8e6642.png)

- Para obtener la contraseña, sube el archivo testkey.pem Archivo:

![imagen](https://user-images.githubusercontent.com/10358317/228978295-f822464a-a448-434f-a22e-5f623620d69e.png)

![imagen](https://user-images.githubusercontent.com/10358317/228978459-8fabb789-eec4-4582-b0e2-4b44d0e2de43.png)

- Ahora, accedemos a Windows usando RDP:

![imagen](https://user-images.githubusercontent.com/10358317/229282800-7ee0cd35-fd46-4ece-9b04-45fa5c785fef.png)

- Haciendo ping a Ubuntu 20.04 desde Windows:

![imagen](https://user-images.githubusercontent.com/10358317/228979379-1ed193a4-c5e4-4526-8a8e-0ecb2cf1a534.png)

- Abriendo las reglas del firewall para hacer ping de Ubuntu a Windows:
- Windows Defender -> Configuración avanzada -> Reglas de entrada -> Compartir archivos e impresoras (Solicitud de eco - ICMPv4 - Entrada) -> Clic derecho (Habilitar)

![imagen](https://user-images.githubusercontent.com/10358317/228980207-1f699349-9d09-4304-ad1f-d66187a0b9e1.png)

- Haciendo ping a Windows 2019 Server desde Ubuntu 20.04:

![imagen](https://user-images.githubusercontent.com/10358317/228980821-98c1b545-c1c1-41ca-a53c-f5049641c9df.png)

- Visualización de la CPU de Ubuntu RAM:

![imagen](https://user-images.githubusercontent.com/10358317/228981485-341444fb-f806-49fe-9a6c-c7edc0d25f17.png)

![imagen](https://user-images.githubusercontent.com/10358317/228981584-9823ad8d-169f-4ef2-b4bc-61852b95393b.png)

- Destruir infraestructura:

```
terraformar destroy

``` ![imagen](https://user-images.githubusercontent.com/10358317/229281383-6c8e01ea-af3a-4477-9be6-a10096318333.png)

![imagen](https://user-images.githubusercontent.com/10358317/229281239-b1713771-2960-4746-a1bd-f7543913bd66.png)

- Asegúrese de que las instancias estén terminadas. Porque si funcionan, pagamos su tarifa:

![imagen](https://user-images.githubusercontent.com/10358317/228982134-2a648783-0f11-47e9-865d-1dc92dffdd0f.png)

- También podemos monitorizar el uso de CPU, disco y red en AWS EC2:

![imagen](https://user-images.githubusercontent.com/10358317/228983384-7b4a0e36-0605-4f33-901f-2a0ebd79c4f6.png)
