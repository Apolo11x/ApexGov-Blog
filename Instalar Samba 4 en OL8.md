# Instalación de Samba 4 como Directorio Activo en Oracle Linux

## 1. Instalación de Oracle Linux

### Requisitos previos
- Descarga la ISO de Oracle Linux desde el sitio oficial: [Oracle Linux](https://yum.oracle.com/)
- Un equipo físico o máquina virtual con al menos:
  - 2 CPU
  - 4 GB de RAM
  - 20 GB de espacio en disco

### Instalación del Sistema Operativo
1. Inicia desde la ISO de Oracle Linux.
2. Selecciona `Install Oracle Linux 8/9` (según la versión estable que elijas).
3. Configura:
   - Idioma: `Español` o `Inglés`
   - Red: Configura una IP estática.
   - Disco: Usa particionado automático.
4. Completa la instalación y reinicia el sistema.

---

## 2. Configuración del Sistema Base

### Actualización del sistema
Ejecuta:
```bash
sudo dnf update -y
```

### Configuración de nombre de host
```bash
sudo hostnamectl set-hostname dc1.midominio.local
```

Edita `/etc/hosts` y agrega:
```bash
127.0.0.1   localhost
192.168.1.10 dc1.midominio.local dc1
```

---

## 3. Instalación y Configuración de DNS (Bind9)

```bash
sudo dnf install bind bind-utils -y
```

Edita el archivo de configuración `/etc/named.conf` y agrega:
```bash
options {
    listen-on port 53 { 127.0.0.1; 192.168.1.10; };
    directory "/var/named";
    allow-query { any; };
};
```

Configura las zonas en `/etc/named.rfc1912.zones`:
```bash
zone "midominio.local" IN {
    type master;
    file "/var/named/midominio.local.zone";
};
```

Crea el archivo de zona `/var/named/midominio.local.zone`:
```bash
$TTL 86400
@   IN  SOA dc1.midominio.local. root.midominio.local. (
        2024031701 ; Serial
        3600       ; Refresh
        1800       ; Retry
        604800     ; Expire
        86400 )    ; Minimum TTL
@       IN  NS  dc1.midominio.local.
dc1     IN  A   192.168.1.10
```

Reinicia el servicio:
```bash
sudo systemctl enable --now named
```

---

## 4. Instalación de Samba 4 AD-DC

### Instalación de paquetes
```bash
sudo dnf install samba samba-dc samba-client samba-common-tools -y
```

### Provisionar el dominio
```bash
sudo samba-tool domain provision --use-rfc2307 --realm=midominio.local --domain=MIDOMINIO --adminpass='TuPasswordSegura' --server-role=dc
```

### Configuración de servicios
```bash
sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo cp /var/lib/samba/private/smb.conf /etc/samba/
```

Edita `/etc/samba/smb.conf` si es necesario.

### Configuración de Kerberos
Edita `/etc/krb5.conf`:
```bash
[libdefaults]
default_realm = MIDOMINIO.LOCAL
```

### Iniciar servicios
```bash
sudo systemctl enable --now samba
```

### Verificación del dominio
```bash
smbclient -L localhost -UAdministrator
