#!/bin/bash

# Atualizando pacotes
echo "Atualizando pacotes do sistema..."
sudo apt update && sudo apt upgrade -y

# Instalando Apache
echo "Instalando Apache..."
sudo apt install apache2 -y

# Iniciando e habilitando Apache
echo "Configurando Apache para iniciar automaticamente..."
sudo systemctl enable apache2
sudo systemctl start apache2

# Instalando Servidor FTP (vsftpd)
echo "Instalando o serviço FTP (vsftpd)..."
sudo apt install vsftpd -y

# Backup do arquivo de configuração original
echo "Fazendo backup do arquivo de configuração vsftpd.conf..."
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bkp

# Configuração básica do FTP
echo "Configurando o serviço FTP..."
sudo bash -c 'cat > /etc/vsftpd.conf <<EOF
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
EOF'

# Reiniciando o serviço FTP
echo "Reiniciando o serviço FTP..."
sudo systemctl restart vsftpd
sudo systemctl enable vsftpd

# Exibindo status dos serviços
echo "Status dos serviços:"
sudo systemctl status apache2 --no-pager
sudo systemctl status vsftpd --no-pager

echo "Instalação concluída! Apache e FTP estão configurados corretamente. 🚀"
