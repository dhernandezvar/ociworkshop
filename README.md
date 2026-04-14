# ociworkshop
Laboratorio exploratorio para fundamentos de OCi

🧑‍💻 Oracle Cloud Infrastructure (OCI) Workshop
Fundamentos – Laboratorio práctico (3 horas)
🎯 Objetivo del laboratorio

En este laboratorio vas a construir una arquitectura real en OCI paso a paso, entendiendo no solo cómo, sino por qué se hace cada cosa.

Al finalizar podrás:

Entender cómo OCI gestiona identidad y acceso (IAM)
Organizar recursos correctamente usando compartments
Crear una red funcional en la nube
Desplegar una VM accesible desde internet
Publicar una aplicación web
Exponerla de forma profesional con un Load Balancer
🧱 ¿Qué vamos a construir?

Durante el laboratorio vas a desplegar:

Una red (VCN) con subnets pública y privada
Una máquina virtual Linux (servidor web)
Un Load Balancer público
Configuración de seguridad:
SSH → Security List (ACL)
HTTP → NSG
⚠️ Regla principal del laboratorio

🔴 Todo debe crearse dentro de tu compartment:

Root
└── Workshop-OCI
    └── TuNombre-OCI

👉 Esto evita conflictos entre estudiantes.

1️⃣ IAM – Entendiendo cómo OCI gestiona usuarios
🎯 ¿Qué vamos a hacer?

Antes de crear recursos, vamos a entender cómo OCI controla el acceso:

Quién puede entrar (usuarios)
Cómo se agrupan (grupos)
Cómo se asignan permisos
🔍 Exploración de Identity Domains
🧭 Paso a paso
Abre el menú ☰
Ve a:
👉 Identity & Security → Domains
Abre:
👉 Default Domain
👤 Usuarios

👉 Ve a: User Management → Users

Aquí verás:

Todos los usuarios del entorno
Tu propio usuario

👉 Esto representa las identidades que pueden acceder a OCI.

👥 Grupos

👉 Ve a: User Management → Groups

👉 Los grupos sirven para asignar permisos de forma escalable.

🔗 Relación usuario-grupo
Regresa a Users
Abre tu usuario
Revisa la sección Groups

👉 Esto te permite ver qué permisos heredas.

📁 Crear tu espacio de trabajo (Compartment)
🎯 ¿Qué vamos a hacer?

Crear tu propio “sandbox” dentro de OCI.

👉 Aquí vivirán TODOS tus recursos.

🧭 Paso a paso
Menú ☰
👉 Identity & Security → Compartments
👉 Create Compartment
Configuración
Name: TuNombre-OCI
Parent: Workshop-OCI
💡 Qué debes entender

Un compartment es como una carpeta lógica donde organizas y aíslas recursos.

2️⃣ Seguridad – Cómo se protege el acceso
🎯 ¿Qué vamos a hacer?

Vamos a revisar cómo OCI asegura el acceso mediante:

Políticas de contraseña
Autenticación multifactor (MFA)
🔑 Password Policy

👉 Ruta:
Domains → Default Domain → Domain Policies → Password Policy

Qué observar
Longitud mínima
Complejidad
Expiración

👉 Esto define qué tan seguras deben ser las contraseñas.

🔐 MFA (Autenticación)

👉 Ruta:
Domains → Default Domain → Authentication

👉 Aquí se definen los métodos de autenticación.

👤 MFA en tu usuario

👉 Ruta:
Identity & Security → My Profile → Security

👉 Aquí ves TU configuración personal.

3️⃣ Cost Analysis – Entendiendo el consumo
🎯 ¿Qué vamos a hacer?

Aprender cómo OCI muestra el consumo de recursos.

🧭 Paso a paso

👉 Billing & Cost Management → Cost Analysis

Qué analizar
Agrupa por servicio
Agrupa por compartment

👉 Esto te permite identificar qué recursos generan costo.

4️⃣ Red – Construyendo la base de todo
🎯 ¿Qué vamos a hacer?

Crear una red completa donde vivirán tus recursos.

👉 Esto incluye:

VCN
Subnets
Gateways
🌐 Crear VCN
🧭 Paso a paso
Menú ☰
👉 Networking → Virtual Cloud Networks
Da clic en:
👉 Actions → Start VCN Wizard
Selección

👉 VCN with Internet Connectivity

💡 Qué hace el wizard

Crea automáticamente:

Subnet pública (internet)
Subnet privada (interna)
Internet Gateway
NAT Gateway
Service Gateway
Configuración
Name: VCN-TuNombre
CIDR: 10.0.0.0/16
🔐 Seguridad de red
🔑 ACL (Security List) – SSH
🎯 ¿Qué vamos a hacer?

Permitir acceso SSH solo desde tu computadora.

🧭 Paso a paso
VCN → Subnets
Selecciona Public Subnet
Abre Security List
Add Ingress Rule
Configuración
Source: tu IP /32
Port: 22
💡 Qué logramos

👉 Solo tú puedes entrar por SSH

🌐 NSG – HTTP
🎯 ¿Qué vamos a hacer?

Permitir tráfico web de forma controlada.

NSG-VM-Web

👉 Permite tráfico HTTP solo desde el Load Balancer

NSG-LB

👉 Permite tráfico HTTP desde internet

💡 Qué logramos
Seguridad en capas
Separación de responsabilidades
5️⃣ VM – Creando el servidor web
🎯 ¿Qué vamos a hacer?

Crear una máquina virtual Linux que actuará como servidor web.

🧭 Creación

👉 Compute → Instances → Create Instance

🔐 SSH Key (CRÍTICO)

👉 Selecciona:

Generate a key pair for me

💡 Qué significa esto

OCI generará:

🔑 Llave privada (tu acceso)
🔓 Llave pública (en la VM)
⚠️ IMPORTANTE

👉 Descarga la llave privada
👉 Guárdala en tu PC

Sin esto NO podrás conectarte

🌐 Networking
Subnet: Public
Public IP: Sí
NSG: NSG-VM-Web
🧭 Conexión
ssh -i llave.key opc@IP_PUBLICA
🌐 Apache – Publicando tu web
🎯 ¿Qué vamos a hacer?

Convertir la VM en un servidor web.

Instalación
sudo dnf install -y httpd
sudo systemctl enable --now httpd
Página
sudo tee /var/www/html/index.html
Validación

👉 http://IP_PUBLICA_VM

6️⃣ Load Balancer – Publicación profesional
🎯 ¿Qué vamos a hacer?

Exponer tu aplicación mediante un punto único de acceso.

Creación

👉 Networking → Load Balancers → Create

Concepto clave

El LB:

Recibe tráfico
Lo distribuye a la VM
Configuración
Backend → VM privada
Listener → HTTP 80
NSG → NSG-LB
Validación

👉 http://IP_LOAD_BALANCER

🎓 Resultado final

Has construido:

Red completa
Seguridad correcta
VM funcional
Web publicada
Load Balancer activo
🚀 Cierre

Este laboratorio representa la base de cualquier arquitectura en OCI.
