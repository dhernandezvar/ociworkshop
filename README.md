# 🧑‍💻 OCI Workshop Lab – Fundamentos (3 horas)

---

## 🎯 Objetivo del laboratorio

En este laboratorio vas a construir una arquitectura completa en **Oracle Cloud Infrastructure (OCI)** paso a paso, como si estuvieras trabajando en un entorno real.

A lo largo del ejercicio vas a entender:

- **qué estás haciendo**
- **por qué lo estás haciendo**
- **qué deberías ver en cada paso**

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

Antes de crear cualquier recurso, revisa SIEMPRE esto en la parte superior:

```
Workshop-OCI / TuNombre-OCI
```

🔴 Si el compartment es incorrecto, todos tus recursos quedarán mal ubicados.

---

# 1️⃣ IAM – Entendiendo quién tiene acceso

## 🎯 ¿Qué vamos a hacer?

Explorar cómo OCI gestiona usuarios, grupos y permisos.

## 🧭 Pasos

1. Menú ☰ → Identity & Security → Domains  
2. Click en Default Domain  
3. Ir a User Management → Users  
4. Revisar lista  
5. Ir a Groups  
6. Revisar grupos  

---

# 2️⃣ Crear tu Compartment

## 🎯 ¿Qué vamos a hacer?

Crear tu propio espacio de trabajo aislado.

## 🧭 Pasos

1. Menú ☰ → Identity & Security → Compartments  
2. Click Create Compartment  
3. Name: TuNombre-OCI  
4. Parent: Workshop-OCI  
5. Create  

---

# 3️⃣ Red – Crear la VCN

## 🎯 ¿Qué vamos a hacer?

Crear la red base donde vivirán todos los recursos.

## 🧭 Pasos

1. Menú ☰ → Networking → Virtual Cloud Networks  
2. Click Actions → Start VCN Wizard  
3. Seleccionar “VCN with Internet Connectivity”  
4. Click Start  

### Configuración

- Name: VCN-TuNombre  
- CIDR: 10.0.0.0/16  

Click Create  

---

# 4️⃣ Seguridad de red

## 🎯 ¿Qué vamos a hacer?

Controlar acceso a la VM.

## 🔐 ACL (SSH)

- Source: TU_IP/32  
- Port: 22  

## 🔐 NSG

- NSG-VM-Web → HTTP desde LB  
- NSG-LB → HTTP público  

---

# 5️⃣ VM – Crear servidor

## 🎯 ¿Qué vamos a hacer?

Crear una VM Linux y conectarnos a ella.

## 🧭 Pasos

1. Menú ☰ → Compute → Instances  
2. Click Create Instance  

## 🔐 SSH

Generate a key pair for me  

Descargar llave privada  

## Conexión

```
ssh -i tu_llave.key opc@IP_PUBLICA
```

---

# 6️⃣ Apache

## 🎯 ¿Qué vamos a hacer?

Convertir la VM en servidor web.

```
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```

---

# 7️⃣ Load Balancer

## 🎯 ¿Qué vamos a hacer?

Publicar la aplicación.

- Backend → VM  
- Listener → HTTP 80  

---

# 🎓 Resultado final

✔ VCN  
✔ VM  
✔ Web  
✔ Load Balancer  
