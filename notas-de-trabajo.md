# Notas de trabajo:
## Estado Inicial (11/11/2026 14:59):
La ordenador se encontró encendido a las 2:59 PM hora del sistema misma hora que la real.
![](Capturas/captura1.png)

## La memoria RAM (11/11/2026 15:16):
Usando DumpIT, genera esta ventana, automáticamente elige la ruta (donde este instalado) y el nombre del archivo destino. \
![](Capturas/dumpit.png)
El archivo .raw contiene los datos de la memoria RAM de la máquina virtual.


Tras varios intentos fallidos con comandos muy modernos para la versión de Windows usada como:
`Get-FileHash "D:\windows\memoria\DumpIT\FORENSE-06-20261111-141030.raw" -Algorithm SHA256 | Out-File "D:\windows\memoria\DumpIT\FORENSE-06-20261111-141030.rawsha256"`

Por lo tanto, se acabó usando el siguiente comando que extraer el hash del archivo .raw:
`certutil -hashfile "D:\windows\memoria\DumpIT\FORENSE-06-20261111-141030.raw" SHA256`
con este comando podemos leer el hash del archivo en Windows 7.

## Los datos volátiles:
No se han podido guardar los datos volátiles porque el script proporcionado no funcionaba, además he intentado modificarlo para que funcione en Windows 7 pero no llegaba a ejecutarse.

## El tráfico de red:
Para adquirir el tráfico de red de la máquina usamos estos comandos en orden:
Crear carpeta donde va a guardar la captura:
``& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" controlvm "FORENSIC_10" nictracefile1 "C:\Users\Jose Luis\Desktop\CASP-042\red.pcap"``

Iniciar la captura:
``& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" controlvm "FORENSIC_10" nictrace1 on``

Finalizar captura:
``& "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" controlvm "FORENSIC_10" nictrace1 off``

Al terminar se genera red.pcap que puede ser leído a través de Wireshark.

## El triaje:
Se intento usar KAPE para el triaje pero no funciona de manera correcta en Windows 7, por lo que optamos por esta serie de comandos para hacerlo a mano:

**Extraer colmenas del Registro: Usuarios, USBs y Sistema.**
`reg save HKLM\SAM "D:\salida\SAM.hiv"`
`reg save HKLM\SYSTEM "D:\salida\SYSTEM.hiv"`
`reg save HKLM\SOFTWARE "D:\salida\SOFTWARE.hiv"`

**Extraer Visor de Eventos: Inicios de sesión y errores.**
`wevtutil epl System "D:\salida\System.evtx"`
`wevtutil epl Security "D:\salida\Security.evtx"`

**Extraer Prefetch: Rastro de programas ejecutados.**
`Copy-Item -Path "C:\Windows\Prefetch*" -Destination "D:\salida\Prefetch" -Recurse`

**Captura:**
![](Capturas/triaje.png)

## El cifrado:

## El apagado:
