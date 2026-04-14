# ociworkshop
🧑‍💻 OCI Workshop Lab – Fundamentos (3 horas)
🎯 Objetivo del laboratorio

En este laboratorio vas a construir una arquitectura completa en Oracle Cloud Infrastructure (OCI) paso a paso, como si estuvieras en un entorno real de trabajo.

No solo vas a crear recursos, sino que vas a entender:

qué estás haciendo
por qué lo estás haciendo
qué resultado debes ver en cada paso
🧱 Arquitectura que vas a construir
Internet
   │
   ▼
Load Balancer (Public)
   │
   ▼
VM (Apache Web Server)
   │
   ▼
VCN (10.0.0.0/16)
 ├── Public Subnet
 └── Private Subnet
⚠️ REGLA MÁS IMPORTANTE DEL LAB

Antes de hacer cualquier acción, revisa esto:

👉 En la parte superior de la consola debe decir:

Workshop-OCI / TuNombre-OCI

🔴 Si esto está mal, TODO lo que crees quedará en el lugar incorrecto.

1️⃣ IAM – Entendiendo quién puede acceder
🎯 ¿Qué vamos a hacer aquí?

Antes de crear recursos, necesitamos entender quién tiene acceso al entorno.

Aquí no vas a crear nada — solo observar.

🧭 Paso 1: Entrar al Identity Domain
En la esquina superior izquierda, haz clic en el menú ☰
En el menú que aparece, busca la sección:
👉 Identity & Security
Da clic en:
👉 Domains

Ahora verás una lista de dominios.

Haz clic en:
👉 Default Domain
👤 Paso 2: Ver usuarios
En el menú izquierdo del dominio, busca:
👉 User Management
Da clic en:
👉 Users

🔍 Aquí estás viendo:

Todos los usuarios que pueden entrar a OCI
Tu propio usuario

👉 Busca tu nombre en la lista.

👥 Paso 3: Ver grupos
En el mismo menú izquierdo:
👉 Da clic en Groups

🔍 Aquí estás viendo:

Grupos de usuarios
Cómo se organizan los permisos
🔗 Paso 4: Ver a qué grupo perteneces
Regresa a: Users
Haz clic en tu usuario
Busca la sección:
👉 Groups

👉 Aquí puedes ver qué permisos tienes indirectamente.

2️⃣ Crear tu espacio de trabajo (Compartment)
🎯 ¿Qué vamos a hacer?

Crear tu propio “espacio aislado” donde trabajarás todo el laboratorio.

🧭 Paso a paso
Abre el menú ☰
Da clic en:
👉 Identity & Security → Compartments

Verás la lista de compartments existentes.

Haz clic en el botón:
👉 Create Compartment
📝 Completa la información
Name: TuNombre-OCI
Parent Compartment: selecciona Workshop-OCI

👉 Este punto es clave: asegúrate de NO dejar Root.

Haz clic en:
👉 Create
✅ Qué debes ver

Tu estructura debe quedar así:

Root
 └── Workshop-OCI
      └── TuNombre-OCI
3️⃣ Seguridad – Cómo se protege el acceso
🎯 ¿Qué vamos a hacer?

Entender cómo OCI protege el acceso:

contraseñas
autenticación multifactor
🔑 Password Policy
Regresa a:
👉 Identity & Security → Domains
Abre: Default Domain
En el menú izquierdo:
👉 Domain Policies → Password Policy

🔍 Observa:

longitud mínima
complejidad
expiración
🔐 MFA (Dominio)

👉 En el menú izquierdo:
👉 Authentication

👤 MFA (Tu usuario)
Menú ☰
👉 Identity & Security → My Profile
Da clic en:
👉 Security
4️⃣ Cost Analysis
🎯 ¿Qué vamos a hacer?

Ver cómo OCI muestra el consumo de recursos.

🧭 Paso a paso
Menú ☰
👉 Billing & Cost Management → Cost Analysis
Configura
Compartment: Workshop-OCI
Periodo: This Month
🔍 Qué observar

Cambia las vistas y analiza:

qué servicios aparecen
si hay consumo
5️⃣ Red – Crear la VCN
🎯 ¿Qué vamos a hacer?

Crear la red donde vivirán todos los recursos.

🔴 Paso 1: Verificar compartment

👉 Debe decir:

Workshop-OCI / TuNombre-OCI
🧭 Paso 2: Ir a networking
Menú ☰
👉 Networking → Virtual Cloud Networks
🧭 Paso 3: Iniciar wizard
Busca el botón:
👉 Actions
Da clic
Selecciona:
👉 Start VCN Wizard
🧭 Paso 4: Seleccionar tipo

Selecciona:

👉 VCN with Internet Connectivity

👉 Da clic en Start

🧭 Paso 5: Configurar
Name: VCN-TuNombre
CIDR: 10.0.0.0/16

👉 Deja todo lo demás por defecto

🧭 Paso 6: Crear

👉 Haz clic en Create

⏳ Espera a que termine

🔐 6️⃣ Seguridad de red
🎯 ¿Qué vamos a hacer?

Configurar quién puede entrar a la VM.

🔑 ACL (Security List) – SSH

👉 Permitir acceso solo desde tu PC

Paso a paso
Entra a tu VCN
Da clic en:
👉 Subnets
Selecciona Public Subnet
Baja hasta:
👉 Security Lists
Da clic en la lista
Da clic en:
👉 Add Ingress Rule
Configura
Source: tu IP /32
Port: 22
🌐 NSG – HTTP

👉 Controlar acceso web

Crear NSG-VM-Web
VCN → Network Security Groups
Create
Regla
Source: NSG-LB
Port: 80
Crear NSG-LB
Source: 0.0.0.0/0
Port: 80
7️⃣ VM – Crear servidor
🎯 ¿Qué vamos a hacer?

Crear una VM y conectarnos a ella.

🧭 Paso a paso
Menú ☰
👉 Compute → Instances
👉 Create Instance
🔐 Paso crítico: SSH Key

Selecciona:

👉 Generate a key pair for me

Descarga

👉 Haz clic en:

Download Private Key

Guárdala en tu PC.

⚠️ Sin esto NO podrás conectarte.

🌐 Networking
Subnet: Public
Public IP: Sí
NSG: NSG-VM-Web
🧭 Crear

👉 Click en Create

🧭 Conectarse
ssh -i tu_llave.key opc@IP_PUBLICA
8️⃣ Apache – Publicar web
Instalar
sudo dnf install -y httpd
sudo systemctl enable --now httpd
Crear página
sudo tee /var/www/html/index.html
Validar

👉 http://IP_PUBLICA_VM

9️⃣ Load Balancer
🎯 ¿Qué vamos a hacer?

Exponer la app como en producción.

🧭 Paso a paso
Networking → Load Balancers
Create
Configurar
Backend → VM privada
Listener → HTTP 80
NSG → NSG-LB
✅ Validación final

👉 http://IP_LOAD_BALANCER

🎓 Resultado final

✔ Red completa
✔ VM funcionando
✔ Web publicada
✔ Load Balancer activo

🚀 Cierre

Este laboratorio simula un escenario real de despliegue en la nube.
