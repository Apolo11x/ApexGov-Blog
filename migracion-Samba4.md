# Migración de un Dominio Samba 4 a un Nuevo Servidor

Este documento detalla cómo realizar una copia de seguridad de un dominio Samba 4 existente y migrarlo a un nuevo servidor.

---

## 1. Verificación del Estado del Servidor Actual
Antes de iniciar la migración, verifica el estado del dominio:
```bash
samba-tool domain info 127.0.0.1
samba-tool drs showrepl
```
Si hay errores críticos, intenta corregirlos antes de continuar.


---

## 2. Realizar Copia de Seguridad del Servidor Samba 4

Ejecuta los siguientes comandos en el servidor actual para respaldar los datos esenciales:

### 2.1 Respaldo de la Base de Datos de Samba
```bash
sudo systemctl stop samba
sudo tar -czvf samba_backup.tar.gz /var/lib/samba /etc/samba /etc/krb5.conf
```

### 2.2 Copia de Usuarios y Grupos
```bash
sudo getent passwd > usuarios.txt
sudo getent group > grupos.txt
sudo pdbedit -L -v > samba_users.txt
```

---

## 3. Restaurar en el Nuevo Servidor

### 3.1 Configurar el Nuevo Servidor
Asegúrate de que el nuevo servidor tiene Oracle Linux instalado y configurado con la misma versión de Samba.

Instala Samba:
```bash
sudo dnf install samba samba-dc samba-client samba-common-tools -y
```

### 3.2 Restaurar la Copia de Seguridad
```bash
sudo tar -xzvf samba_backup.tar.gz -C /
sudo chown -R root:root /var/lib/samba /etc/samba
```

### 3.3 Restaurar Usuarios y Grupos
```bash
sudo cat usuarios.txt | while read line; do useradd -M $(echo $line | cut -d: -f1); done
sudo cat grupos.txt | while read line; do groupadd $(echo $line | cut -d: -f1); done
```

### 3.4 Restaurar Base de Datos de Usuarios Samba
```bash
sudo pdbedit -i smbpasswd:samba_users.txt
```

### 3.5 Iniciar el Servicio en el Nuevo Servidor
```bash
sudo systemctl enable --now samba
```

---

## 4. Verificación Post-Migración
Ejecuta:
```bash
samba-tool domain info 127.0.0.1
samba-tool drs showrepl
```
Prueba la autenticación:
```bash
smbclient -L localhost -UAdministrator
