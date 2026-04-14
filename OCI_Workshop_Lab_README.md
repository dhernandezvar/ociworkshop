# 🧑‍💻 OCI Workshop Lab – Fundamentos (3 horas)

---

## 🎯 Objetivo del laboratorio

En este laboratorio vas a construir una arquitectura completa en **Oracle Cloud Infrastructure (OCI)** **paso a paso, como si estuvieras en un entorno real de trabajo**.

No solo vas a crear recursos, sino que vas a entender:

- **qué estás haciendo**  
- **por qué lo estás haciendo**  
- **qué resultado debes ver en cada paso**  

---

## 🧱 Arquitectura que vas a construir

```
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
```

---

## ⚠️ REGLA MÁS IMPORTANTE DEL LAB

Antes de hacer cualquier acción, revisa esto:

```
Workshop-OCI / TuNombre-OCI
```

🔴 **Si esto está mal, TODO lo que crees quedará en el lugar incorrecto.**

---

# 1️⃣ IAM – Entendiendo quién puede acceder

## 🎯 ¿Qué vamos a hacer aquí?

Antes de crear recursos, vamos a entender **quién tiene acceso al entorno**.

👉 En esta sección **no vas a crear nada**, solo observar.

---

## 🧭 Paso 1: Entrar al Identity Domain

1. Menú ☰ → **Identity & Security**
2. Click en **Domains**
3. Click en **Default Domain**

---

## 👤 Paso 2: Ver usuarios

- Ir a **User Management → Users**

---

## 👥 Paso 3: Ver grupos

- Ir a **User Management → Groups**

---

## 🔗 Paso 4: Ver a qué grupo perteneces

- Users → tu usuario → **Groups**

---

# 2️⃣ Crear tu espacio de trabajo (Compartment)

## 🎯 ¿Qué vamos a hacer?

Crear tu propio espacio aislado.

---

## 🧭 Paso a paso

1. Menú ☰ → **Identity & Security → Compartments**
2. Click en **Create Compartment**

---

## 📝 Configuración

- Name: `TuNombre-OCI`
- Parent: `Workshop-OCI`

---

# 3️⃣ Gestión de Accesos

## 🎯 ¿Qué vamos a hacer?

Vamos a obsevar la configuración de políticas de password y MFA.

## 🔑 Password Policy

Ruta:
Domains → Default Domain → Domain Policies → Password Policy

## 🔑 Sign On Policies

Ruta:
Domains → Default Domain → Domain Policies → Sign On Policies → "Security Policy for OCI Console"

---

## 🔐 MFA (My Profile)

- Dominio: Authentication
- Usuario: My Profile → Security

---

# 4️⃣ Cost Analysis

## 🎯 ¿Qué vamos a hacer?

Vamos a obsevar los reports de uso de la nube.

Ruta:
Billing & Cost Management → Cost Analysis

- Compartment: Workshop-OCI
- Periodo: This Month

---

# 5️⃣ Red – Crear la VCN

## 🎯 ¿Qué vamos a hacer?

En esta sección vamos a crear la red principal de nuestro entorno en la nube.

👉 Piensa en la VCN como el equivalente a una red corporativa dentro de OCI, donde vivirán todos nuestros recursos.

Durante este paso:

- Vamos a crear una Virtual Cloud Network (VCN) con un rango de direcciones IP propio
- Se crearán automáticamente dos subredes:
    * Una subnet pública → donde estará nuestra VM accesible desde internet
    * Una subnet privada → pensada para recursos internos (no accesibles directamente)
- Se configurarán los componentes necesarios para conectividad:
    * Internet Gateway → para acceso desde/hacia internet
    * NAT Gateway → para salida a internet desde la subnet privada
    * Service Gateway → para acceder a servicios de Oracle sin salir a internet

👉 En otras palabras, aquí estamos construyendo la base de toda la arquitectura, ya que sin red no podríamos conectar ni publicar ningún recurso.

## 🧭 Paso a paso

1. Menú ☰ → Networking → Virtual Cloud Networks
2. Actions → Start VCN Wizard
3. Seleccionar: VCN with Internet Connectivity

---

## Configuración

- Name: VCN-TuNombre
- CIDR: 10.0.0.0/16

---

# 6️⃣ Seguridad de red

## ACL (SSH)

- Source: TU_IP/32
- Port: 22

---

## NSG

- NSG-VM-Web → HTTP desde LB
- NSG-LB → HTTP público

---

# 7️⃣ VM – Crear servidor

## Paso clave

👉 Generate a key pair for me

Descargar la llave privada.

---

## Conexión

```
ssh -i tu_llave.key opc@IP_PUBLICA
```

---

# 8️⃣ Apache

```
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```

---

# 9️⃣ Load Balancer

- Backend → VM
- Listener → HTTP 80

---

# 🎓 Resultado final

✔ VCN  
✔ VM  
✔ Web  
✔ Load Balancer  
