# Lab GNU # 48 - Rede sem network-manager

Configurações de rede wi-fi, utilizando outra ferramenta sem ser o network-manager.

## NetworkManager

Administra das conexões de rede no computador. 

- nmtui (ferramenta do network manager via ncurses)
- nmcli (ferramenta do network manager via linha de comando)

## Comandos

`$ ip -c a` Mostrar informações de endereço de todas as interfaces de rede

`$ iw dev wlp3s0 scan` Faz um scan de todas as redes wi-fi disponíveis

`$ iw dev wlp3s0 scan | grep SSID` Seleciona apenas nomes de rede

`wpa_passphrase "ssid" "password" > arq.conf` gera arquivo de configuração (deletar linha com password).

## Sem network manager através do arquivo /etc/network/interfaces

`$ wpa_passphrase "ssid" "password"` para obter a string de conexão

Editar o arquivo `/etc/network/interfaces` e inserir os comandos abaixo

```
auto wlan0
iface wlan0 inet dhcp
wpa-ssid ssid
wpa-psk string-de-conexão
```
*PSK* - pre shared key

Após configurar o arquivo reiniciar o serviço de rede

`systemcl restart networking.service` para restartar o serviço de rede.

## Sem network manager e utilizando a linha de comando

`$ wpa_passphrase "ssid" "password" > /etc/wpa.conf` Gera arquivo de configuração

`$ wpa_supplicant -c /etc/wpa.conf -i wlan0` Conecta à rede wi-fi

`$ wpa_supplicant -Dnl80211 -iwlan0 -c/etc/wpa_supplicant.conf` Outra forma de conectar

## Outros comandos

`$ iwconfig` verifica configurações do adaptador wi-fi.

`CTRL-z` manda job para o background (para o job)

`$ jobs` lista os jobs com número

`$ bg 1` manda job #1 para o background (para continuar a execução)

`$ fg 1` manda job #1 de volta para o foreground (continua a execução)

`journalctl -f` verifica a conexão em tempo real

`journalctl -xe` Verifica journalctl com paginação e explicações adicionais

`ip link set wlan0 up` sobe a interface de wi-fi

## Tirar a interface da responsabilidade do NetworkManager

`/etc/NetworkManager/` diretório do network manager

`nmcli dev status` verifica as interfaces gerenciadas pelo network manager

`nmcli dev set wlan0 managed no` tira a interface do domínio do network manager

## Configurando manualmente o endereço IP após conexão wi-fi

`ip addr add 10.0.0.10/24 dev wlan0` atribui endereço à interface

`ip addr del 10.0.0.10/24 dev wlan0` deleta endereço da interface

`ip route` mostra as rotas atualmente configuradas

## Configurando endereço IP via DHCP

`nmap --script broadcast-dhcp-discover -e wlan0` consulta dhcp mas sem atribui endereço

`dhclient wlan0` consulta e conecta utilizando dhcp para configurar o endereço IP

## Rota default

Rota default é o gateway padrão. Interface padrão para qual são enviados pacotes que não pertencem à nossa rede.

## Usando o Netctl

Instalar via apt e criar arquivo de configuração

```
/etc/netctl/wlp1s0-hackers_zone
Description='My WiFi Profile'
Interface=wlp1s0
Connection=wireless
Security=wpa
ESSID=hackers_zone
IP=dhcp
Key=Password@345\!
```

Conectar

```
netctl list
netctl start profile
netctl stop profile
netctl stop-all 
netctl restart --> reinicia um profile
netctl switch-to profile --> muda para o profile
netctl is-active profile --> verifica se o profile está ativo
netctl enable profile --> habilita start do profile durante o boot
```

Também pode-se utilizar o programa `wifi-menu -o` para se conectar a rede wi-fi.

Depurar 

`journalctl -b -u netctl@profile.service` para depurar rede de nome *profile*.

Certificar-se que a interface não está *UP* e que o dhcliente não esteja rodando. Também verifique se o wpa_supplicant não está rodando.
