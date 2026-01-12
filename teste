#!/bin/bash

getAPR() {
    COM=dialog --stdout --title "Apropos de comandos" --inputbox "Digite um comando" 10 30
    APR=apropos $COM

    if [[ $? -eq 1 ]]; then
        dialog --title "mensagem de erro" --msgbox "comando nao encontrado... encerrando programa" 10 35
        exit 0
    fi

    dialog --title "Apropos" --msgbox "$APR" 50 50
}

getMAN(){
        COM=$(dialog --stdout --title "manual de comandos" --inputbox "Digite um comando" 10 30)
        MANU=man $COM

        if [[ $? -eq 1 ]]; then
                dialog --title "Mensagem de erro" --msgbox "comando nao encontrado... encerrando programa" 10 35
                exit 0
        else
                dialog --title "Manual" --msgbox  "$MANU" 50 50
        fi
}
getGNU(){
        VERSION=help | egrep GNU
        dialog --title "GNU VERSION" --msgbox "$VERSION" 10 33
}
getMAC(){
        MAC=ifconfig|egrep ether| awk '{print $2}'
        dialog --title "MAC ADRESS" --msgbox "$MAC" 10 30
}
getIP(){

        IP=curl ifconfig.co
        dialog --title "IP PUBLICO" --msgbox "$IP" 10 30
}
getLOCAL(){
        LOCAL=ifconfig | egrep inet | awk '{print $2}' | head -n 1
        dialog --title "LOCAL" --msgbox "$LOCAL" 10 20
}
testWIFI(){
        WIFI=ping -c 4 google.com | egrep  'packet'
        if [[ "$WIFI" == *"0% packet loss"* ]]; then
                dialog --title "WIFI tester" --msgbox  "0 PACOTES PERDIDOS CONECTIVIDADE OK" 10 35
        else
                dialog --title "WIFI tester" --msgbox  "PACOTES PERDIDOS ERRO DE CONECTIVIDADE" 10 35
        fi
}
changePASSWORD(){

 if [ $? -eq 1 ]; then
                dialog --msgbox "Encerrando programa" 10 30
                exit 0
        fi

        senha=$(dialog --stdout --title "troca de senha" --passwordbox "digite sua nova senha" 10 30)
        if [ $? -ne 0 ]; then
                dialog --title "operacao cancelada" --msgbox "troca de senha cancelada" 10 30
                exit 1
        fi
        confirm=$(dialog --stdout --title "confirme sua senha" --passwordbox "digite sua nova senha novamente" 10 30)
        if [ $? -ne 0 ]; then
                dialog --title "operacao cancelada" --msgbox "troca de senha cancelada" 10 30
                exit 1
        fi

        if [ "$senha" != "$confirm" ]; then
                dialog --title "erro" --msgbox "Senhas nao coincidem. tente novamente." 10 30
                exit 1
        fi
        echo -e "$senha\n$confirm" | passwd

        if [ $? -eq 2 ]; then
                dialog --title "Erro" --msgbox "Erro ao alterar a senha. Tente novamente" 10 30
        else
                dialog --title "Sucesso" --msgbox "Senha alterada com sucesso!" 10 30
        fi

}


dirORG(){
        name=dialog --stdout --title "Organizador" --inputbox "Digite o nome do diretorio a ser organizado" 10 30
        dir="$name"

        if [ ! -d "$dir" ]; then
                dialog --title "Erro" --msgbox "O diretorio $dir nao existe" 10 30
                return
        fi

        for i in "$dir"/*; do

                if [ -f "$i" ]; then
                        type=$(file -b "$i")
                        dir_final="$dir/$type"

                        mkdir -p "$dir_final"
                        mv "$i" "$dir_final/"

                        if [ $? -eq 2 ]; then
                                dialog --stdout --title "ERRO" --msgbox "Diretorio $dir nao pode ser organizado" 10 30
                                return
                        fi
                fi
        done

        dialog --stdout --title "sucesso" --msgbox "diretorio $dir organizado com sucesso" 10 30


while true; do
        RES=$(dialog --stdout --title "MENU INICIAL" --menu "selecione uma opcao" 17 44 9 \
        MAC 'mostra o endereco MAC' \
        IP 'mostra o IP publico' \
        LOCAL 'mostra o IP LOCAL' \
        WIFI 'teste de conexao' \
        SENHA 'alterar senha' \
        ORG 'organiza um diretorio' \
        GNU 'mostra a versao do bash' \
        MAN 'mostra o manual de um comando' \
        APR 'mostra o apropos de um comando' )
        if [ $? -eq 1 ]; then
                dialog --msgbox "Encerrando programa" 10 30
                exit 0
        fi


        case $RES in
                MAC) getMAC;;
                IP) getIP;;
                LOCAL) getLOCAL;;
                WIFI) testWIFI;;
                SENHA) changePASSWORD;;
                ORG) dirORG;;
                GNU) getGNU;;
                MAN) getMAN;;
                APR) getAPR;;
        esac
done
