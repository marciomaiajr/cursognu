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

`jounalctl -f` verifica a conexão em tempo real

`ip link set wlan0 up` sobe a interface de wi-fi

## Tirar a interface da responsabilidade do NetworkManager

`/etc/NetworkManager/` diretório do network manager
`nmcli dev status` verifica as interfaces gerenciadas pelo network manager
`nmcli dev set wlan0 managed no` tira a interface do domínio do network manager

## Configurando manualmente o endereço IP após conexão wi-fi

`ip addr add 10.0.0.10/24 dev wlan0` atribui endereço à interface

## Configurando endereço IP via DHCP

`nmap --script broadcast-dhcp-discover -e wlan0` consulta dhcp mas sem atribui endereço
`dhclient wlan0` consulta e conecta utilizando dhcp para configurar o endereço IP

