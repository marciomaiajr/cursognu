# Lab GNU # 45 - Initramfs, bug gnome-software

## Initramfs

Inital RAM disk.

Arquivo em `/boot/initrd.img-5.15.0-2-amd64`

Utilitário é o `update-initramfs`

Arquivos de configuração em `/etc/initramfs-tools/`

Listar sistema de arquivos com `lsinitramfs /boot/initrd.img-5.15.0-2-amd64`

Ver a linha de comando que foi carregado o kernel `cat /proc/cmdline`

Passar parâmetro break no grup pressionando a tecla "e" e editando o comando linux para anexar `break` a ela. Isso irá fazer você cair no prompt/shell do initramfs.

Dentro do shell podemos utilizar alguns comandos como:

`blkid` para ver a UUID dos discos e partições

`fsck /dev/sda1` para corrigir algum erro no sistema de arquivos

`Ctrl-c` para sair do prompt do initramfs *?!?*

`update-initramfs -u -k all` atualiza (`-u`) initramfs para todos os kernels (`-k all`).

`update-initramfs -c -k all` cria (`-c`) initramfs para todos os kernels (`-k all`). Utilize em caso de não ter um initramfs para um kernel.




