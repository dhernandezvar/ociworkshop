# 🧑‍💻 OCI Workshop Lab – Fundamentos (3 horas)

---

## 🎯 Objetivo del laboratorio

En este laboratorio vas a construir una arquitectura completa en **Oracle Cloud Infrastructure (OCI)** paso a paso.

Vas a entender:
- Qué estás haciendo
- Por qué lo estás haciendo
- Qué deberías ver en cada paso

---

## ⚠️ Regla principal

Todos los recursos deben crearse en:

```
Workshop-OCI / TuNombre-OCI
```

---

# 1️⃣ IAM

## 🎯 ¿Qué vamos a hacer?

Explorar cómo OCI gestiona usuarios, grupos y acceso.

## 🧭 Pasos

1. Menú ☰ → Identity & Security → Domains  
2. Click en Default Domain  
3. Ir a User Management → Users  
4. Revisar lista  
5. Ir a Groups  
6. Revisar grupos  

---

# 2️⃣ Compartments

## 🎯 ¿Qué vamos a hacer?

Crear tu espacio de trabajo aislado.

## 🧭 Pasos

1. Menú ☰ → Identity & Security → Compartments  
2. Click Create Compartment  
3. Name: TuNombre-OCI  
4. Parent: Workshop-OCI  
5. Create  

---

# 3️⃣ Red (VCN)

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

## 🔐 Security List (SSH)

## 🎯 ¿Qué vamos a hacer?

Permitir acceso SSH solo desde tu computadora.

## 🧭 Pasos

1. Entrar a la VCN  
2. Click Subnets  
3. Abrir Public Subnet  
4. Click Security Lists  
5. Click Add Ingress Rule  

### Configuración

- Source: TU_IP/32  
- Protocol: TCP  
- Port: 22  

---

## 🔐 NSG

## 🎯 ¿Qué vamos a hacer?

Controlar tráfico HTTP.

### NSG-VM-Web

1. VCN → Network Security Groups  
2. Create  
3. Name: NSG-VM-Web  
4. Add rule: Port 80 desde NSG-LB  

### NSG-LB

1. Create  
2. Name: NSG-LB  
3. Rule: Port 80 desde 0.0.0.0/0  

---

# 4️⃣ VM

## 🎯 ¿Qué vamos a hacer?

Crear un servidor web en la nube.

## 🧭 Pasos

1. Menú ☰ → Compute → Instances  
2. Click Create Instance  

### Configuración

- Name: vm-web  
- Image: Oracle Linux  

### Networking

- VCN: VCN-TuNombre  
- Subnet: Public  
- Public IP: Enabled  

### SSH

Seleccionar:
Generate a key pair for me  

Descargar Private Key  

---

## Conexión

```
ssh -i llave.key opc@IP_PUBLICA
```

---

# 5️⃣ Apache

## 🎯 ¿Qué vamos a hacer?

Convertir la VM en servidor web.

## 🧭 Pasos

```
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```

Crear página:

```
sudo tee /var/www/html/index.html
```

---

# 6️⃣ Load Balancer

## 🎯 ¿Qué vamos a hacer?

Publicar la aplicación como en producción.

## 🧭 Pasos

1. Networking → Load Balancers  
2. Create  

### Configuración

- Public  
- Backend: VM privada  
- Listener: HTTP 80  

---

# 🎓 Resultado final

✔ Red  
✔ VM  
✔ Web  
✔ Load Balancer  
