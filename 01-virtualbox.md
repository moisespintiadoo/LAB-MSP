# 1. Preparar VirtualBox y las máquinas virtuales

## Comprobar la virtualización

Administrador de tareas → Rendimiento → CPU → **Virtualización: Habilitado**.
Si no lo está, activar Intel VT-x o AMD-V/SVM en la BIOS.

> Si las VM van muy lentas o aparece una tortuga verde en VirtualBox, la causa suele ser la
> *integridad de memoria* de Windows (Hyper-V) forzando un modo lento.

## Crear la red del laboratorio

Archivo → Herramientas → **Network Manager** (`Ctrl+H`) → pestaña **NAT Networks** → **Crear**:

| Campo | Valor |
|---|---|
| Nombre | `LabMSP` |
| IPv4 Prefix | `10.10.10.0/24` |
| DHCP | Activado |

Una red NAT permite que las VM se vean entre sí y tengan salida a Internet.

## Crear `MSP-Server`

**Nueva** →

| Parámetro | Valor |
|---|---|
| Nombre | `MSP-Server` |
| ISO | `ubuntu-24.04.x-live-server-amd64.iso` |
| Instalación desatendida | Omitir |
| RAM / CPU | 4096 MB / 4 |
| Disco | 80 GB, dinámico (sin "Preasignar tamaño completo") |

Configuración → Red → Adaptador 1 → **Red NAT** → `LabMSP`.

## Crear `Client-W10`

| Parámetro | Valor |
|---|---|
| Nombre | `Client-W10` (o `Cliente-W11`) |
| Tipo | Windows 10 (64-bit) / Windows 11 (64-bit) |
| Instalación desatendida | Omitir |
| RAM / CPU | 4096 MB / 4 |
| Disco | 80 GB, dinámico |
| Red | Red NAT `LabMSP` |

Con la plantilla de Windows 11, VirtualBox activa EFI, TPM 2.0 y Secure Boot automáticamente.

**Cuenta local en Windows:** si el asistente obliga a usar cuenta Microsoft, `Shift+F10` →
`start ms-cxh:localonly` (Windows 11) o desconectar la red durante la instalación (Windows 10).
