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
4. Descripción: Compartimento para Laboratorio  
5. Parent: Workshop-OCI  
6. Create
7. Ingresar al compartment Workshop-OCI
8. Validar que exista el nuevo compartment TuNombre-OCI en la sección Child Compartments
9. Refrescar la pantalla para que cargue la nueva estructura de compartments 

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

Solamente coloca los siguientes datos (el resto déjalos igual):

- Name: VCN-TuNombre  
- CIDR: 10.0.0.0/16  

Click Next
Aparecerá una pantalla con la previsualización de las configuraciones de la VCN (toma un tiempo para revisarlas)
Click Create

Una vez creada, explora la nueva VCN para revisar los recursos creados:
- Subnets
- Gateways
- Security Lists

---

# 4️⃣ Seguridad de red

## 🎯 ¿Qué vamos a hacer?

Controlar acceso a la VM.

## 🔐 ACL (SSH)

1. Valida la IP desde que estás conectado en el siguiente enlace: https://whatismyipaddress.com/. Guarda la IP que te aparece en pantalla.
2. Menú ☰ → Networking → Virtual Cloud Networks →  VNC-TuNombre → Security → "Default Security List for VCN-TuNombre"
3. Click en Security Rules
4. En la sección Ingress Rules, agrega una regla 
- Source CIDR: TU_IP/32  
- Port: 22,80
5. Click en Add Ingress Rule
6. Revisa que las reglas de ingreso hayan sido creadas
7. Ubica la regla que tiene Source CIDR 0.0.0.0/0 y puerto 22, márcala y en el botón de "..." click en "remove". Con esto te asegurarás que solamente desde tu propia IP podrás conectarte a la VM que crearás posteriormente.   

---

# 5️⃣ VM – Crear servidor

## 🎯 ¿Qué vamos a hacer?

Crear una VM Linux y conectarnos a ella.

## 🧭 Pasos

1. Menú ☰ → Compute → Instances  
2. Click Create Instance
3. Nombre de la VM: VM-TuNombre
4. Compartimento: TuNombre-OCI
5. Availability Domain: Déjalo por defecto
6. Operating System: Oracle Linux 9
7. En Shape, cámbiala a AMD con las siguientes características: 
<img width="752" height="315" alt="image" src="https://github.com/user-attachments/assets/1ff335a8-796c-4fdb-9cdc-f2604aef9b19" />

8. Click en Select Shape
9. De vuelta a la pantalla principal, click en Next
10. Deja Security Options por default y click en Next
11. En Networing utiliza los siguientes valores:
    - VNIC Name: TuNombre-VNIC
    - Primary NetworK: Select Existing Network
    - Compartment VNC:  TuNombre-OCI
    - VCN: VCN-TuNombre
    - Compartment Subnet: TuNombre-OCI
    - Subnet: public-subnet-VCN-TuNombre
     🔐 En Advanced Options:
    - Generate a Key Pair for Me
    - Click en Download private Key
    - Click en Download public Key
    - Guarda las llaves en una carpeta de fácil acceso. La llave privada será requerida para conectarnos a la VM posteriormente.
    - Click Next
12. En Storage ubica el Boot Volume y cambia el selector "Specify a custom boot volume size and performance setting". Coloca 100 GB en el campo Boot Volume Size.
13. Click en Next.
14. Valida la información que se indica en Review.
15. Cuando estés listo, click en Create para el inicio de la creación de la VM
    
 
## Conexión Segura 🔐
Una vez que el job de creación indique el status running, ubica tu VM en:
1. Menú ☰ → Compute → Instances (siempre bajo tu compartment TuNombre-OCI)
2. En pantalla aparecerán las direcciones pública y privada de tu VM. Toma la pública para conectarnos.
3. En tu PC Windows, abre un CMD (o cualquier otra terminal que te permita acceso SSH de forma remota.
4. Con el siguiente comando Windows, quitaremos los permisos heredados de tu llave privada:
```
icacls "D:\Temp\TuLlave.key" /inheritance:r
```
6. Con el siguiente comando Windows, agregaremos el permiso para que solo tu usuario pueda leer la llave:
```
"D:\Temp\TuLlave.key" /grant:r "%USERNAME%":F
```
7. Desde el terminal, ejecuta el siguiente comando:
```
ssh -i tu_llave.key opc@IP_PUBLICA
```
8. En tu VM, ejecuta el siguiente comando para validar que estás en tu VM Creada en OCI, esto retornará el nombe de la VNIC
```
hostname
```
# 6️⃣ Instalación de tu primar aplicación utilizando Apache Server

## 🎯 ¿Qué vamos a hacer?

Convertir la VM en servidor web.

1. Dentro de la terminal conectada a tu vm, vamos a agregar el puerto de acceso de forma permanente en el firewalld interno: 
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```
2. Ahora procederemos con la instalación del Apache Server
```
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```
3. Para comprobar que el Apache quedó funcionando, deberías poder acceder desde tu navegador con el siguiente enlace: http://ippublicadetuvm. Se vería lo siguiente:

<img width="953" height="394" alt="image" src="https://github.com/user-attachments/assets/f0d417a3-cec0-427a-ab0e-643b40093975" />

4. Crear un página personalizada:
```
sudo nano /var/www/html/index.html
```
5. Ahora, dentro del archivo, colocamos el siguiente código HTML:
```
<!DOCTYPE html>
<html>
<head>
    <title>Mi Primera Página Web</title>
</head>
<body>
    <h1>Mi Primera Página Web</h1>
    <p>Esta es una página dummy de prueba.</p>
</body>
</html>
```
8. Para grabar el resultado, presionamos "Ctrl + O" + Enter.
9. Para salir de Nano, presionamos "Ctrl + X"
10. Finalmente reiniciamos el servicio de Apache Server 
```
sudo systemctl restart httpd
```
11. Finalmente, volvemos a acceder desde el navegador a la siguiente URL: http://ippublicadetuvm. Se vería algo así:
<img width="346" height="86" alt="image" src="https://github.com/user-attachments/assets/dc14613b-055b-4fd0-b84a-0007899f52a4" />

---

# 7️⃣ Load Balancer

## 🎯 ¿Qué vamos a hacer?

En esta parte vamos a publicar nuestra aplicación detrás de un **Load Balancer público**.

Hasta ahora ya tienes una VM con Apache funcionando. Sin embargo, en una arquitectura real no se expone directamente el servidor a internet. En su lugar, se utiliza un **Load Balancer** como punto de entrada para recibir el tráfico de los usuarios y enviarlo al servidor que ejecuta la aplicación.

Durante este paso vamos a:

- Crear un **Load Balancer público**
- Definir un **backend** (nuestra VM)
- Crear un **listener HTTP en el puerto 80**

👉 Al final, ya no accederás a la VM directamente, sino a través del Load Balancer.

---

## 🧭 Paso a paso

### Ir al servicio de Load Balancers

1. Abre el menú principal (☰)
2. Da clic en:  
   **Networking → Load Balancers → Load Balancer**

👉 Aquí verás la lista de balanceadores (probablemente vacía).

---

### Verificar el compartment

Antes de continuar, valida en la parte superior tu compartimento:

```
Workshop-OCI / TuNombre-OCI
```

🔴 Es importante que el Load Balancer se cree en el mismo compartment que la VCN y la VM.

---

### Crear el Load Balancer

Da clic en: **Create Load Balancer**

👉 Esto permitirá acceso desde internet.

---

### Configuración básica

Completa los campos:

- **Name**: `lb-web`
- **Visibility**: Public
- **Assign Public IP Address**: Ephimeral

### Seleccionar red

- **VCN**: `VCN-TuNombre`
- **Subnet**: **Public Subnet**

👉 El Load Balancer debe estar en la subnet pública para recibir tráfico externo.

---
### Bandwith, Security,Acceleration y Management

👉 Deja los valores por defecto y click en Next

---
### Crear el Backend Set

- **Specify a load balancing policy:** Weighted Round Robin

---

### Agregar la VM como backend

1. En la sección "Select backend servers", haz clic en **Add Backend**
2. Selecciona tu VM y click en **Add Instances**
3. Cuando regresas al flujo principal de creación del Load Balancer, en la instancia agregada valida que el puerto configurado por defecto sea 80
4. En el campo "Specify health check policy", revisa que el protocolo coloca TCP y puerto 80.
5. El resto de valores déjalos sin cambios.
6. Click en Next

---

### Configuración del Listener

1. Haz clic en **Create Listener**

Configura:

- **Tipo de Protocolo:** HTTP
- **Puerto:** 80

El resto de los valores déjalos por defecto y click en Next

---
### Manage logging

Revisa los valores definidos por defecto y click en Next

---
### Review and Create

👉 Revisa de nuevo los valores definidos y haz clic en **Create** y espera a que esté ACTIVE y "Overall health" en el Balanceador esté .

---

### Validar que esté funcionando:

Abre en tu navegador:

```
http://IP_PUBLICA_DEL_LOAD_BALANCER
```

---

## ✅ Resultado esperado

Debes ver:

La página inicial de Apache Server
  
